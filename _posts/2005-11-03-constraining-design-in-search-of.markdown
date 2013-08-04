---
layout: post
title: "Constraining Design in Search of Elegance"
alias: /2005/11/constraining-design-in-search-of.html
categories:
---
Rather than play with the latest shiny new framework, language, etc. my vice is continually trying new and/or whacky -- for me at least-- programming/design techniques. To be fair, I don't tend to thrust these upon unsuspecting clients as **the** new way of doing things. Rather, I'll start a new pet project of some kind and try to push the _paradigm_ as far as possible and see where it -- or I -- break down.

Most of the time these "techniques" manifest themselves as a set of _constraints_ or _rules_ for coding; something akin to a manual Checkstyle or PMD for design rather than coding conventions. So just like constraining method lengths and Cyclomatic complexity help shape a design by forcing me to actually _think_ about what I'm trying to achieve, I like to play with other types of constraints.

For example, [James](http://www.redhillconsulting.com.au/blogs/james) and I had a go at 100% TDD for a Swing application many, many moons ago. Again not so much because we think 100% TDD is necessarily a good (or bad) thing but more in attempt to push the idea as far as possible, just to see how it and we would react under pressure so to speak. The results were actually very interesting and made me want to find someone prepared to pay me money to build a rich-client application (Not using Eclipse RCP!)

After I started doing some Objective-C some months back, I was forced to deal with the fact that there was no such thing as a private method -- everything in Objective-C is _effectively_ public. I then started to think about what it might be like to apply the same thing to my Java code; what if I was forced to make every method public? How would that affect my design.

Interestingly it had a largely positive effect: I started making smaller classes with well defined behaviour that made sense to be public. In a way, I was forced to make myclass behaviour more [cohesive](http://www.agiledata.org/essays/classNormalization.html#3ONF). The problem was of course that, as with anything that is approached in such a strict and unforgiving manner, there are times when it just makes more sense to have a private method that can be re-used or more often, helps to make the code more readable.

So for the most part, I was happy with the outcome: assuming behaviour is public yet not wishing to expose all of that behaviour forces me to compose large classes from smaller ones thereby hiding "private" implementation detail in public methods on other, re-usable, classes.

More recently I decided to try a couple of other things out on a project. These were:

* No `if`'s;
* No casting to expose "conditional" behaviour; and
* No getters &amp; setters -- i.e. [tell don't ask](/blog/2005/08/30/pull-me-push-me-anyway-you-want-me).

Nothing special here really, but I wanted to see how far I could push the ideas and what effect that had on my design and productivity.

The first -- _no `if`'s_-- is relatively simple to address through the use of polymorphism and `Map`s as jump-vector tables; techniques most people probably use anyway.

The second -- _No casting to expose "conditional" behaviour_ -- is a little harder. To completely eradicate. Firstly, I was using Java 5 with "generics" to remove the need to cast items in collections -- although if you're not careful, the code can get quite messy with all the "devils horns" -- however, the real problem comes when you have a group of classes all related via an interface where _some_ of the implementations provide certain behaviour and others don't. This was most noticeable when implementing anything that resembled a [Composite Pattern](http://en.wikipedia.org/wiki/Composite_pattern).

Enter the [Courtesy Implementation](http://www.martinfowler.com/bliki/CourtesyImplementation.html). In every case where I ended up needing to implement a method that didn't "make sense" it would have been a programmatic error to have called the method on an instance of that particular class anyway so I simply threw an `UnsupportedOperationException`. At this point I wished that would happen by default; that by implementing an interface you could somehow say you weren't going to support certain methods. A pretty stupid thing to want really, given I'm working in a strictly-typed language.

And finally, _tell don't ask_. The code -- comprising 50 main classes and slightly fewer test classes (tsk tsk) -- has zero (0) getters and setters. Admittedly I wasn't implementing a [CRUD](http://en.wikipedia.org/wiki/CRUD_%28acronym%29) application so that made things easier but there were times when I _thought_ I would need a getter but managed to get around that need quite elegantly (IMHO) through diligent use of simple call-backs (no _real_ [closures](http://en.wikipedia.org/wiki/Closure_%28computer_science%29) in Java unfortunately); more complex [Visitors](http://en.wikipedia.org/wiki/Visitor_pattern); implementing `Comparable`; etc. In other words, lots of interfaces.

One thing that did arise though was the constant fight against cyclic-dependencies. One example where this is particularly obvious is with visitor. The "standard" way to write a visitor is something along the lines of:

```
public interface Visitor {public void visitX(X x);public void visitY(Y y);}public class X {public void accept(Visitor visitor) {...}}public class Y {public void accept(Visitor visitor) {...}}
```

The problem with this is that the `Visitor` depends on `X` and `Y` yet the classes `X` and `Y` both depend on `Visitor`: cyclic dependence.

Interestingly, the "usual" way to implement the visitor would probably look something like this:

```
public class PrintingVisitor implements Visitor {public void visitX(X x) {out.println("X:" + x.getName());}public void visitY(Y y) {out.println("Y: " + y.getTotal());}}
```

But recall that I had imposed the constraint of _no getters_ so instead re-worked it like this:

```
public interface Visitor {public void visitX(String name);public void visitY(int total);}public class PrintingVisitor implements Visitor {public void visitX(String name) {out.println("X:" + name);}public void visitY(int total) {out.println("Y: " + total);}}
```

Like anything, this is open to abuse, but there are no longer any cyclic dependencies and the only time the guts of `X` and `Y` are exposed is when calling on the visitor.

The example just shown is very simple and won't work in all cases of course but it does highlight how a bit of extra thinking can lead to interesting, and hopefully richer and more useful, designs; the constraints I had imposed seemed to force me to think more about the behaviour and less about the data. For some of you freaks who find that very easy to do I'm very happy, but for mere mortals such as myself, having a few rules-of-thumb to know when I'm being "dumb" makes life that much easier.

Another really interesting thing for me in all of this is that statically-typed languages such as Java force me to make all my interfaces _explicit_. That is, I need to create physical interfaces so as to remove the cyclic-dependencies between concrete classes. However, with languages such as Ruby, Smalltalk etc, the interfaces are _implicit_. In a sense, Ruby and Smalltalk allow me to define interfaces at the method level; a much smaller level of granularity than is practical with Java. When I asked a old smalltalker friend of mine about how he would have dealt with cyclic-dependencies, he replied "we never worried about it; if the object responded to a message that's all that mattered." I repeated this answer to another non-smalltalk friend of mine to which he mused "and they probably created a lot of spaghetti!"

So, while many people are clamouring for more-and-more functionality from their programming language, API's etc. I think sometimes I'd actually prefer less; so long as long as it's the right set of course ;-)
