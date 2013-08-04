---
layout: post
title: "Why I prefer constructor-based depency injection"
alias: /2004/04/why-i-prefer-constructor-based-depency.html
categories:
---
On the current project I'm on, and no doubt on many of yours, we have restrictions on the number of parameters you may declare for a method. This, hopefully, forces developers to [re-evaluate what they are passing around](/blog/2004/03/26/code-normalisation). For example, use a `DateRange` instead of `dateFrom` and `dateTo`.

Unfortunately there is a simple way to **subvert** that process by simply declaring everything as "setters" which typically have only one parameter. Then all the lovely detail becomes much harder to see as it's hidden in the morass of the aptly name mutators (yet another reason I [dislike getters and setters](/blog/2003/12/15/what-are-your-intentions)).

Declaring service dependencies in constructors allows me to see them all in one go. It immediately becomes apparent if a class depends on "too many". Something that is much harder to see when you use setters. It also allows us to construct objects in a valid state with all the obvious benefits.

Another advantage to using a constructor is that only the creator need know about the dependencies. In our case it's our `ServiceRegistry`. Once an object has been constructed (either by the registry configuration for singleton services defined by interfaces or by calling `ServiceRegistry.newInstance(Class)` allowing the construction of any class that depends on a service) client code is unaware of the dependency.

I've recently been converting large swathes of code on this project from static lookups and setter-based dependency injection (DI) to constructor based and in the process finding stuff that was not previously apparent. These include classes that depend on too-many services, classes that depend on services they just shouldn't and classes that are quite fragile because callers didn't realise they had to call the setters in a particular order! None of these was in anyway obvious previously.

The down-side of course is that constructor parameters make inheritence a pain. If you have a deep and/or wide class heirarchy, you will need to declare the dependencies in the constructors of all the sub-classes. Not only is this tedious but it necessarily exposes the dependency to classes that probably don't directly make use of the service.

My counter to this is simple: don't have deep and/or wide class heirarchies. I rarely find the need for them and they're usually a code smell. The fact that we _can_ extend classes to inherit behaviour doesn't mean we _should_. No doubt a topic for another rant :-)

As always there are exceptions. Sometimes you just can't get around the need for setter-based DI, usually when you are constrained by someone elses API. Most noteably in our case is the fact that (for reasons I don't want to go into urgh!) we're forced to use versions of third-party libraries ([Struts](http://jakarta.apache.org/struts/)) that want to directly construct our classes with no factory mechanism. This means we have `Actions` that must have a default constructor.

You may have noticed a distinct lack of the 'T' word. I deliberately stayed away from describing how all this makes classes more testable and is a natural consequence of doing TDD anyway because if you're doing TDD you already know this and if you don't like TDD (really?) I didn't want to put you off the idea by implying this was all about testing, which it's not.
