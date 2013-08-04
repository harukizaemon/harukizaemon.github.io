---
layout: post
title: "Why Object-Oriented Languages Need Tail Calls"
alias: /2009/12/why-object-oriented-language-need-tail-calls.html
categories:
---
Disclaimer: I unashamedly stole the title after reading [another article](http://projectfortress.sun.com/Projects/Community/blog/ObjectOrientedTailRecursion) on the same topic.

Some of you may know of a [little project](http://github.com/harukizaemon/hamster) I've been working on in my, albeit very limited, spare time. Hamster started out as an implementation of [Hash Array Mapped Trees](http://lamp.epfl.ch/papers/idealhashtrees.pdf) (HAMT) for Ruby and has since expanded to include implementations of other [Persistent Data Structures](http://en.wikipedia.org/wiki/Persistent_data_structure) such as Sets, Lists, Stacks, etc.

For those that aren't up with HAMTs or persistent data structures in general, they have a really neat property: very efficient copy-on-write operations. This allows us to create immutable data-structures that only need copying when something changes, making them a very effective when writing multi-threaded code.

Hamster also contains an implementation of [Cons Lists](http://en.wikipedia.org/wiki/Cons) with all the usual methods you'd expect from a Ruby collection such as `map`, `select`, `reject`, etc. thrown in for good measure.

One of the things I really wanted to investigate was laziness. So, for example, when evaluating:

{% highlight ruby %}
Hamster.interval(1, 1000000).filter(&:odd?).take(10)
{% endhighlight %}

Rather than generate a list with a million values, evaluate them all against the filter, and then select the first ten, Hamster lazily generates the list, the evaluation of `filter`, and even `take`. In fact, as it stands, the example code won't _actually_ do anything; you would need to call `head` to kick-start anything happening at all. This behaviour extends, to the extent possible, to all other collection methods.

Hamster also supports infinite lists. For example, the following code produces an infinite list of integers:

{% highlight ruby %}
def integers
  value = 0
  Hamster.stream { value += 1 }
end
{% endhighlight %}

Now we can easily generate a list of odd numbers:

{% highlight ruby %}
integers.filter(&:odd?)
{% endhighlight %}

Again, rather than generate every possible integer and filter those into odd numbers, the list is generated as necessary.

OK, so enough with the apparent shameless self-promotion. Let's get to the point.

My first implementation of lists used recursion for collection methods. The code was succinct, and, IMHO elegant. It conveyed the essence of what I was trying to achieve. It was easier to understand and thus, I would surmise, easier to maintain. The problem was that for any reasonably large list, stack overflows were common place. The lack of Tail-Call-Optimisation (TCO) meant that the recursive code would eventually blow whatever arbitrary stack limits were in place. The solution: convert the recursive code to an equivalent iterative form.

Once all methods had been re-implemented using iteration, the code ran just fine on large lists; no more stack overflow errors. The downside was, the code had almost doubled in size--1~2 lines of code became 2~4 or in some cases even more. The code was now harder to read and far less intention revealing. In short, the lack of Tail-Call-Optimisation lead to less maintainable and I'd hazard a guess, more error prone code.

The story however, doesn't end there. Take another (albeit contrived) example that partitions integers into odds and evens:

{% highlight ruby %}
partitions = integers.partition(&:odd?)
odds = partitions.car
evens = partitions.cadr
{% endhighlight %}

You would expect `odds` to contain `[1, 3, 5, 7, 9, ...]`, and `evens` to contain `[2, 4, 6, 8, 10, ...]`. But the way I initially implemented the code it didn't. Here's an example to show what happened:

{% highlight ruby %}
odds.take(5)    # => [1, 3, 5, 7, 9]
evens.take(5)   # => [2, 12, 14, 16, 18]
{% endhighlight %}

Confused? So was I until it dawned on me that I had broken a fundamental principle: immutability. The underlying block that generates the list of integers has state! Enumerating the odd values first produces the expected results but once we get around to enumerating the even values, the state of the block is such that it no longer starts at 1--reversing the order of enumeration produces a corresponding reversal of the error. Pure functional Languages such as Haskell have mechanisms for dealing with this but in Ruby, the only construct I really have available to me is explicit caching of generated values.

Once I had cached the values all was well, or so I thought. I started to write some examples that used files as lists:

{% highlight ruby %}
File.open("my_100_mb_file.txt") do |io|
  io.to_list.map(&:chomp).map(&:downcase).each do |line|
    puts line
  end
end
{% endhighlight %}

Running the code above took forever to run, much slower than the non-list equivalent. I expected a little slow down sure, but nothing like that which I was seeing.

At first I suspected garbage collection--perhaps the virtual machine was being crushed by the sheer number of discarded objects; I could find no evidence for this. Next, I suspected synchronisation--anything with state needs synchronisation. Again, I found no evidence for this either. A bit more fiddling and a few dozen print statements later--Ruby has no real profiling tools that I'm aware of, something that frustrates me no end at times--I realised what the problem was.

When I failed to find any evidence of garbage collection as the culprit, it had seemed a bit odd but I wasn't sure why I felt that way and thus moved on. Had I stopped and thought about it for a while I may have realised that in fact that was _exactly_ the problem: there was NO evidence of garbage collection at all. How could that be? Processing hundreds of thousands of lines in a 100MB text file using a linked list was sure to generate lots of garbage. Once a line had been processed, the corresponding list element should no longer have been referenced and thus made available for garbage collection, unless... unless for some mysterious reason each element was still being referenced.

My caching implementation worked like this: As each value is generated, it's stored in an element and linked to from the previous element: `[A] -> [B] -> [C]`. At face value this works well--if you never hold a references to "A" or "B", they will become available for garbage collection. So what could possibly have been going wrong? Each line was being processed and then discarded. Surely, that meant each corresponding element should have become available for garbage collection?

Now recall that I had converted the recursive code to an iterative equivalent. This had now come back to bite me, hard!--though to be fair the recursive code would have suffered in a similar and perhaps more obvious way. The call to `map` runs in the context of the very first line which, because of the caching, directly and indirectly references every other line that is processed! The lack of Tail-Call-Optimisation in Ruby means that whether I use recursion or iteration, if I process all elements from the head of a stream, the garbage collector can never reclaim anything because the head element is always referenced until the end of the process!

Some of my colleagues have suggested that I just get over it and use a "real" language like [Clojure](http://clojure.org/). Whilst I understand the sentiment, the point of Hamster is not necessarily to implement a functional language in Ruby. Rather, it is to see what can be done in object-oriented languages and, in this case, Ruby.

Hamster has allowed me to demonstrate that functional language idioms can, for the most part, translate quite well into object-oriented equivalents. However, the lack of Tail-Call-Optimisation severely limits what is possible.

**Update 2009/12/30**: MacRuby supports a limited form of TCO as well. I received similar results as for YARV (see below) the differences being you're not limited to the call being the _last_ statement in the method and there's a bug where you receive a segmentation fault rather than a stack overflow.

**Update 2009/12/27**: According to [this redmine ticket](http://redmine.ruby-lang.org/issues/show/1256), YARV has some limited TCO support which is disabled by default. I performed the necessary incantations to enable it, only to discover the true meaning of "limited": optimise calls to the same method in the same instance _iff_ the call is the last statement in the method.
