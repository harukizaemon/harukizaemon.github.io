---
layout: post
title: "Functional programming in object oriented languages"
alias: /2010/03/functional-programming-in-object-oriented-languages.html
categories:
---
In my current job, I spend about 40% of my time with my underpants on the outside, digging around in production code, generally making stuff better. The other 60% of the time is R&D. The R&D part has some very concrete objectives but there is certainly leeway to explore different ways of developing software.

Like many programmers, my first formal introduction to OO was all about classes and inheritance. What mattered most was getting the "structure" right. Next, I came to understand the importance of encapsulation, and after that polymorphism.

Over the past 6-12 months or so I've become more and more interested in functional programming concepts if not functional programming languages. I've always been a big fan of declarative programming, business rules, etc. and yet I've also always been a big fan of OO. Even when I was an assembler programmer I tended to structure my code and data as if it were object-oriented, even if the `self` pointer was explicit.

More recently I re-read [SICP](http://mitpress.mit.edu/sicp/), learned [Haskell](http://www.haskell.org/) which I unashamedly love, played with [Clojure](http://clojure.org/) on and off, and briefly looked at [Scala](http://www.scala-lang.org/). Co-incidentally, I also read [Domain Driven Design](http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215), and [Clean Code](http://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) along with a number of other very interesting articles on functional programming, immutability in general and recursion in object-oriented languages.[^1][^2][^3][^4] All of which got me thinking, once again, that perhaps I'd been doing this OO thing all wrong.

Here I'd like to present a few observations from my exploration into functional programming in an object-oriented world.

<!--more-->
* Immutable objects good; Mutable objects bad
* An object is a collection of partially applied functions
* An object's API provides a clear separation between Commands and Queries
* An object is a snapshot of state and possible outcomes
* An object is a persistent data structure

Clear as mud hey?

Immutable objects good; Mutable objects bad
-------------------------------------------

> "Classes should be immutable unless there's a very good reason to make them mutable....If a class cannot be made immutable, limit its mutability as much as possible." -- Joshua Bloch, Effective Java

I won't go into a lot of detail as to why I believe this to be true. There are plenty of arguments to be found both for and against. Suffice it to say, the direction I'm taking and the conclusions I draw from my experiences are predicated on this belief. What I will say however, is that the effect it has on my designs quite often gives me the same sense of satisfaction as when practising Test Driven Design.

It's not as though immutability in object-oriented programming is anything new. Many years ago I wrote code where all my value objects were immutable as was the occasional service class, etc. The difference in my current approach is that everything is immutable except for a tiny layer at the fringes where I need to save data for later retrieval, consume HTTP requests, etc. Yes, even my "entities" are immutable.

An object is a collection of partially applied functions
--------------------------------------------------------

> "The ideal number of arguments for a function is zero ... More than three requires very special justification -- and then shouldn’t be used anyway." -- Bob Martin, Clean Code

In functional languages, partial application of a function allows us to define a new function by "pre-populating" some of the arguments. For example, we can take a very simple Haskell function that calculates the amount of applicable sales tax:

``` haskell
applicableSalesTax percentage amount = (percentage / 100) * amount
```

and then partially apply it to create another function that has a fixed sales tax:

``` haskell
applicableGST = applicableSalesTax 10
```

The function `applicableGST` partially applies `applicableSalesTax` with the value 10. Anytime we call `applicableGST` it will invoke `applicableSalesTax` with a rate of 10% and whatever amount we pass to it.

Now consider an object-oriented approach (in Ruby). Imagine a very simple `SalesTax` class that holds the rate:

``` ruby
class SalesTax

  def initialize(percentage)
    @rate = percentage / 100.0
  end

  def applicable_for(amount)
    (amount * @rate)
  end

  def included_in(amount)
    amount - excluded_from(amount)
  end

  def excluded_from(amount)
    amount / (1 + @rate)
  end

end
```

Here, the constructor sets up the context within which each of the methods then operates, just the way we always read good OO code should be.

I've starting to think of constructor arguments as the mechanism for partially applying all the methods on an object. Considering an object as a partial application of a set of methods is really quite interesting to me. It almost dictates that methods MUST operate, in some way, on the state of the object -- just as we always read good OO code should -- only there's a nice explanation as to why: If they didn't operate on the object's state, they wouldn't be partially applied.

When I apply this principle in my designs I find I have smaller, more cohesive classes -- ie the methods are all related more closely to the shared state. I also find I have far fewer private methods and more often than not, none at all. (This coincidentally fits in nicely with my relatively long held belief that [private methods are a smell](http://www.harukizaemon.com/2004/02/dont-touch-my-privates.html) though this is a perhaps a topic for another discussion.) I also find that the constructor then becomes a meaningful, nay critical, part of my API.

An object's API provides a clear separation between Commands and Queries
------------------------------------------------------------------------

> "Methods should return a value only if they are referentially transparent and hence possess no side effects." -- Bertrand Meyer

Interestingly, when we deal with immutable objects we really have little choice but to do just this.

If we want an object to "answer something" (a query) no modification is expected and we simply return a (potentially calculated) value:

``` ruby
def full_name
  "#{@first_name}, #{@last_name}"
end
```

If we want an object to "do something" (a command) we'll be expecting some kind of representation of the new state as the result:

``` ruby
def set_first(new_last_name)
  Person.new(@first_name, new_last_name)
end
```

If we assume we only want to return one thing from a method, and that a change in state necessitates returning a handle to the new state, then a method can only ever be either a query or command but not both.

After discussing this with a colleague, they suggested that in a sense commands are now queries, eg. that ask the question: "what would a system look like if I asked you to do something?" Given [this definition of Commands and Queries](http://martinfowler.com/bliki/CommandQuerySeparation.html) I can certainly see it from that perspective.

In the example just given we merely returned a newly created object but it could just as easily have inserted a record into a database. So, I still like to think of commands in the original sense except they now return a handle to the new state. Much like an HTTP PUT/POST/DELETE request does when following the principles of [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer).

(As an aside, one consequence of this approach is that objects are [closed under modification](http://en.wikipedia.org/wiki/Closure_%28mathematics%29). That is, whenever we modify an object, we receive the result in the form of a new object of the same type.)

An object is a snapshot of state and possible outcomes
------------------------------------------------------

> "Domain Events represent the state of entities at a given time when an important event occurred and decouple subsystems with event streams. Domain Events give us clearer, more expressive models in those cases." -- Eric Evans

I presume it would be largely uncontroversial to describe immutable objects as a snapshot of some state. When I also consider an object as a collection of partially applied functions, I begin to think of an object -- and therefore the system as a whole -- as a snapshot not only of state but also possible outcomes.

I suspect thinking about the difference between a system that undergoes state changes in situ and one that in effect represents every possible outcome simultaneously without needing to materialise them all in advance leads to a very different way of modelling a problem.

While pondering this, I recalled that as a kid I loved [Choose Your Own Adventure books](http://en.wikipedia.org/wiki/Choose_Your_Own_Adventure)? At the end of each page (or few pages) you are presented with a set of choices: Do you turn left? Do we Open the door? Do you run away? Depending on the choice we make, the story takes a different course. Objects seem a bit like pages in a Choose Your Own Adventure Book, frozen in time. The methods are like the choices we make -- each one takes us to a different page, itself frozen in time. I used to bookmark pages with my thumb, like a save-point. If I didn't like the way the story was headed I simply turned back to the last known "good" point in the book.

An object is a persistent data structure
----------------------------------------

A [persistent data structure](http://en.wikipedia.org/wiki/Persistent_data_structure) is one that efficiently preserves the previous version of itself when it is modified. One of the simplest persistent data structures is the singly-linked (cons) list but almost any tree structure can be adapted to be persistent.

If we're creating snapshots in order to preserve the initial state we'll need to make a copy that can be modified. It's reasonable to assume that all these redundant copies will take precious memory and computing cycles and in doing so create quite a bit of garbage. Assuming a naive copying strategy that makes deep, nested copies, this will certainly be the case. Thankfully, there are some pretty neat optimisations we can make by because every object is immutable.

When thought of as a tree with the references to other objects as the children, an object can be treated just like a persistent data structure. If we need to make a copy and modify something, all we need do is create a shallow copy and change the necessary fields. In this way, each copy is like a delta from the previous, sharing as much state as possible and reducing not only the time to copy but also the amount of memory needed.

When I first started down this path, I used constructors for this purpose. For example, if I wanted to create a new `Money` object, I'd create a new version using regular object construction:

``` ruby
class Money

  attr_reader :currency, amount

  def initialize(currency, amount)
    @currency = currency
    @amount = amount
  end

  def add(other)
    Money.new(@currency, @amount + @currency.convert(other.currency, other.money))
  end

end
```

I found this approach worked fine for small, value-object sized, classes however for objects that reference more than two or three values, constructing new instances became cumbersome. Moreover, if an object had internal state that was hidden but necessary when copying, the constructor API would become polluted. Even in languages such as Java that provide method overloading, or Ruby with first-class associative arrays, it still felt overly complicated to construct a new instance just to change a single value.

Instead I've started using an approach that involves cloning. Whenever I need to make a change, I perform a shallow copy, update the appropriate fields and return the result. In a language such as Ruby, this is quite simple to implement and make safe. Instead of using object construction, anytime I wish to make a change I do something like this:

``` ruby
def add(other)
  transform do
    @amount = @amount + currency.convert(other.currency, other.money)
  end
end
```

The `add` method now looks similar to a method you'd write if you were able to modify state; we assign a new amount based on some calculation. The `transform` method[^5] is the key here by making a shallow copy of the object, running the block within the context of the copy, then freezing the result to prevent modification before returning it to the caller. I'm finding this approach to have a number of advantages.

My constructor API isn't "polluted" with internal implementation concerns. The constructor remains a part of the public API.

The construction of the modified copy isn't leaking into the implementation of the method itself. Instead, the method can focus on the job at hand.

Because the method only ever needs to concern itself with the data upon which it operates, the rest of the class can vary relatively independently. When we were explicitly constructing a new object, the `add` method had to concern itself with also copying the currency. When using `transform` that problem goes away. No matter how many other fields need to be copied, the `add` method remains unchanged.

In a sense each method describes the delta between the current state and the new state. Just like a persistent data structure. It reminds me of branching in a Version Control System.

More to explore
---------------

As I mentioned very early on, I'm working with designs that are almost completely immutable, even at the entity level. This approach results in some really positive benefits but also presents some interesting implementation challenges as well as challenging some of my long held beliefs.

Immutable designs _feel_ easier to reason about. I have no empirical evidence for this just a gut feel. When I'm trying to work out why something isn't working as expected, more often than not I can just read through the code and work out what happened. I'm not trying to keep a mental mode of all the side effects.

When dealing with any kind of RDBMS, immutable objects lend themselves to a model where changes to the database are written as events rather than updates to individual records. Unlike a traditional ORM, I don't have the "luxury" of modifying an object to assign an ID. This hasn't presented much of a problem as yet though I suspect it might become more interesting as the object model increases in complexity.

I've been contemplating trying out an OODB such as [MagLev](http://github.com/MagLev/maglev) to see how that might fit in. I suspect it should be no more difficult than with mutable objects and perhaps even simpler.

I'm also working on a project at the moment that uses an immutable RDF store wrapped in an object model to make it actually useable. So far it's fit really nicely.

I'm wary of my code turning into a collection of function objects. Ie. objects that effectively have a single `doIt` method. It doesn't happen too often to be a concern but it certainly does happen and I'm still not certain how I feel about it.

I sometimes find myself exposing state I wouldn't otherwise have needed when I used mutable objects. It feels a tad icky but thus far I haven't found it to be much of an issue.

Testing has also been interesting. I find I'm doing far less mocking and much more state-based testing. I find the tests I'm writing to be far more declarative than when I do interaction based testing. Define the initial state of the system, run the code, and compare against the expected state. Even (especially?) at the unit level this has been very effective. Again, I'm not sure how I feel about this but so far, it's worked out well.

Of course not everything is immutable. The database isn't nor is the file system though in both cases I try my best to treat them as if they were by only ever writing new records/files. The system runtime isn't immutable either -- the act of creating a new object proves that.

Perhaps it really is a natural progression from here to the use of more functional languages as some of my colleagues tell me I should. However there does seem to be a general lack of distinction between the benefits of functional programming concepts and functional programming languages. I'm thoroughly enjoying incorporating functional programming concepts into object-oriented languages.

[^1]: [Why Functional Programming Matters](http://www.cs.chalmers.se/~rjmh/Papers/whyfp.pdf)
[^2]: [Why Why Functional Programming Matters Matters](http://weblog.raganwald.com/2007/03/why-why-functional-programming-matters.html)
[^3]: [Functional Objects](http://www.ccs.neu.edu/home/matthias/Presentations/ecoop2004.pdf)
[^4]: [Why Object Oriented Languages Need Tail Calls](http://lambda-the-ultimate.org/node/3702)
[^5]: [Hamster - Efficient, Immutable, Thread-Safe Collection classes for Ruby](http://github.com/harukizaemon/hamster/)
