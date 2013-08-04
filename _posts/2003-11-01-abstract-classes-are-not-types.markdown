---
layout: post
title: "Abstract classes are not types"
alias: /2003/11/abstract-classes-are-not-types.html
categories:
---
I have a rule of thumb that says, in most cases, any class `X` that is abstract MUST be named `AbstractX`. Abstract static factories are one instance I can think of where I intentionally break this rule. But the [checkstyle](http://checkstyle.sf.net) check I wrote to enforce this understands that abstract classes are allowed to end in the word `Factory`.

The next part of the rule says that no variables, fields, parameters, etc. may be of a type who's name begins `Abstract`.

Now that I've adhered to this rule, I find I'm passing `AbstractShapes` and `AbstractVehicles` around which doesn't really seem to make sense to me.

Why? Because IMHO abstract classes are not types. Abstract classes in Java are a convenience of the language that, for one thing, facilitate code sharing through inheritence. But inheritence is by no means the only, nor the best, way to share common code. We can use delegation (eg composition), decoration (see me in my new AOP colours), etc.

So what do I do instead? In C++ I would have used a pure-virtual class. In Java I use interfaces.

Have you ever seen code that declared a variable of type `java.util.AbstractList`? No? Why not? It's there, along with HashMap and TreeSet, etc. Because `AbstractList` is not a type. `List` is. `AbstractList` provides a convenient base from which to implement custom `Lists`. But I still have the option of implementing my own from scratch if I so choose. Maybe because I need some behaviour for my list such as lazy loading or dynamic expansion, etc. that wouldn't be satisfied by the default implementation.

Classes are a combination of [data and behaviour]({% post_url 2003-12-03-arent-classes-supposed-to-have-both-data-and-behaviour %}). Common practice is to hide data in such a way that access to the data is encapsulated within methods. This makes good sense for a number of reasons including the fact that maybe the particular piece of data is actually a calculated field and not a stored value. Just because I choose to implement the age of a `Person` as a stored value, shouldn't mean that every `Mammal` must be implement in this way. What if I wanted to calculate the age of a `Dog` based on date of birth and the notion that 1 dog year is approximately 7 human years. That's easy you say, make `Mammal` an abstract class and the `getAge()` method abstract. Well yes that would work but now lets extend that reasoning out to other attributes and behaviour of `Mammals` and soon we end up with a pure-abstract class. In other words an interface.

Attemping to use abstract classes as types also makes it difficult to implement decorators (yes I know we have AOP frameworks), especially if we accept the idea that methods not intended to be overidden should be declared final!

All of this leads inexorably (gotta love the wakowski brothers for bringing this word back into the english language) to the conclusion that abstract classes are not types. They are someones idea of the way in which a convenient base class implementation might behave.

IMHO interfaces are good. TDD almost always leads to the definition of more interfaces. Use them and leave abstract classes to reduce duplication of common code among related imnplementations of a given type.
