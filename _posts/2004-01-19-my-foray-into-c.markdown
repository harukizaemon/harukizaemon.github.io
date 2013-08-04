---
layout: post
title: "My foray into C#"
alias: /2004/01/my-foray-into-c.html
categories:
---
Well after a relaxing 10 days riding my motorbike around Tasmania I return to Melbourne, chilled (literally), relaxed and raring to go. I think it's about time to convert [Simian](/simian) to .net as had been suggested by many users. So here is a little blog of my experiences.

To start with, I've never written a line of C#. EVER. So I'm totally ignorant about all language constructs (except that having read the CLR book it looks like Java with uppercase method names) including, importantly, the libraries.

I'm also a linux weenie so I'm using [mono](http://www.go-mono.org) under [Gentoo](http://www.gentoo.org). The combination is pretty good. Mono comes with `mcs` the compiler, `monodoc`, a purpose built browser for the framework libraries and `mono` for running your assemblies. At least I think that's the new fandangled name for executables. Anyone?

I had first investigated writing an automated tool. The process seemed mechanical enough to automate yet difficult enough to end up doing some curly stuff. I also looked at some off the shelf converters but in the end decided doing it myself by hand was probably a good way to learn the ins and outs of the C# language and at the same time indulge my years of Java bias :-D.

My approach is simple: Open my project in [IntelliJ](http://www.intellij.com) (my editor of choice) and one by one, copy the `.java` files to corresponding `.cs` files, run `mcs` and hack away until it compiles cleanly. I have to also admit that I'm not going to turn my code into C# style stuff during this conversion. All my method names, variable names, class names, interfaces are remaining untouched. I'm not adding an `I` to my interfaces nor am I giving all my method names an uppercase first letter. Stuff that LOL.

So here we go...

The first thing is the package declaration. This becomes a namespace. Only, some crack smoking monkey decided that it should be a proper block which means I have to have open and closing curly braces around my entire source file. Not so bad but that means everything gets indented one more level. YUK! So I choose to just leave the indenting ASIS. Not so bad. Still seems unecessarily verbose to me.

Next. `String` becomes `string`. What's with that? Method names get uppercase but `String` (a class) gets a lowercase? I got over it. Find+Replace is my friend.

Once again, Find+Replace for `boolean`. It's a `bool` in C#. I'm guessing a hang-over from C++. Again, I can live with this.

What's the problem with declaring a class as `final`? Ahhh. A quick search of the 'net reveals that it's `sealed`. Ok. Another keyword to remember. Not too bad I guess. On we go...

Crap. How do I declare a constant? In java it's `static final`. In C# it's, quite logically I guess, `const`. Bulk Find+Replace for that. Done

Ok. Now I want an immutable (readonly) instance field. In Java I just mark something as `final`. In C# it turns out, I have to use `readonly`. Egads! Another keyword to remember.

It's interesting. When I first started using Java I thought it odd that a few keywords, such as `final`, seemed to be used for slightly different meaning but it soon became apparent that really the meaning was the same: It's final; Unchangeable; Immutable; Not modifiable. C# seems to want me to use a different keyword for every little thing. This is beginning to irritate me but it's still not difficult to do.

Starting to lose enthusiasm for this. I see all this syntactic sugar entering the Java language, obviously driven by C# and I wonder if my already less than perfect Java will become a quagmire of lexical sludge.

On I go...

Syntax Error. Hmmm... Maybe it's not called `throws` in C#? Quick IM to Mike Melia. C# has no checked exceptions. I knew this. But what I didn't know is that you can't even declare that a mehod throws an exception. Any exception. Hmmm. Oh well. Believe it or not, it's my custom Assert class (starting simple) and it's only `IllegalStateException` anyway so strictly speaking it doesn't have to be declared. I'll just delete it.

Ok what's the problem now? class `IllegalStateException` not found. What's an equivelant in C# I wonder. Trudging through the documentation I give up. This doco is really hard to follow. JavaDoc seems much easier to read. Maybe it's just a matter of what I'm used to. I'll just throw `ApplicationException` instead.

Still giving me grief. I prefix the class name with `System.`. That does the trick. Hmmm. That's a bit stupid. Having to import the `System` namespace? Go figure. I choose to add a `using` for it. I don't like putting package/namespace names into my code.

I discover that C# doesn't allow importing classes. Only namespaces. This is ok by me. I always figure that once you have a dependency on a class in a package, you really have a dependency on all classes in the package in a way. What I don't like is that now I don't have any idea, just by looking at the top of the class, what other classes it depends on.

Woohoo. A clean compile. It's complaining that there is no entry point but I can live with that. I haven't specified a main anywhere. At least we got a compile.

It's now a little after 12.30am. A bit of success has brought back my enthusiasm. I'll keep going.

Time for an interface. Bleh. Can't have `public` keyword on interfaces. Yeah Yeah I know. They're all public anyway. But as in Java, it annoys me that a missing visibility modifier means one thing for a class and another for an interface. At least in Java I can safely put the `public` there and it doesn't complain. Find+Replace. Done.

Now time to do one of the classes that implements the interface.

Ok. This is ridiculous. If I have an abstract class that implements an interface but doesn't implement all the methods, I still have to define the methods in the abstract class as `abstract`. I've defined the class as abstract. Can't the compiler work this out? Sheesh. Talk about needless typing.

Uh. Now I discover I have to explicitly say `virtual` on all my methods to allow them to be overriden. I have no problem with saying `override` when I actually do override something, that's kinda cool, but I'm guessing the `virtual` bit is a hang-over from C++ and/or it makes it easier to optimize the output of the compiler because I the developer have to tell the compiler that there is no need to create jump-vector-tables for the method. Whatever the reason, combined with the previous stuff to do with interfaces, it's really starting to drive me nuts.

Ok. It's now 2:45am and I'm honking along. Thankfully the compiler catches all the bits I forget to change. Sometimes the messages aren't particularly helpful but that may just be the mono compiler. Nothing to do with C# per-se.

I've done all the simple classes I could find. All the ones with few dependencies. Now it's time for some of the more meatier ones. I think I'll keep going until 3.30am and then call it a night.

I have a bunch of [decorators](http://www.exciton.cs.rice.edu/JavaResources/DesignPatterns/DecoratorPattern.htm) (I love decorators) but boy is it a pain in the butt to implement in C#. It's like alphabet soup after I've added all the necessary keywords: `virtual`, `override`, `sealed`, etc. Do Microsoft developers get paid by keystrokes? Anyway I'm getting there. It really is becoming quite mechanical now.

C# uses C++ style class extension. So instead of saying `extends` you use a colon `:`. you also call the super-class constructor using the C++ style colon after the constructor name instead of on the first line of the constructor. That's not too bad. Pretty much the same thing anyway. Oh and just to be different, `super` becomes `base`.

I've just spent the last 20 minutes learning about the collections classes. I'm beginning to think the .Net libraries were an R&D project that made it into the wild a little too early. 57 (I exaggerate a little hehe) different collection classes and none seem to do what I want.

Aha! There it is. `StringCollection`. Basically a `Set` implementation for strings. Ok now to iterate over them...

Grrrr. No `Iterators`. `IEnumerator`? Gimme a break. Instead of `hasNext()` followed by `next()` they use `MoveNext()` which returns `true` if it was successfull and a `Current` property that is `null` if there isn't a current value. Ok. but I can change my `for` loops into `foreach` which is kinda cool and surely makes up for it. As I mentioned before, that's one of the features I'm looking forward to in J2SDK 1.5.

I've just converted some code that parses numbers and some that performs file I/O. Do you think that I could work out what exceptions might be thrown? I'm going head-first into the unchecked exceptions debate here and state outright that it is plain broken and wrong that the only way I can find out what exceptions can be thrown is to pray and hope that they were documented. My god! Not only that, but the I/O exceptions don't seem to extend any sensible base class. So now instead of hoping I've caught all the necessary exceptions, I'm forced to catch `Exception`.

I'm very glad I'm converting from fully tested, well designed (if I do say so myself, which I do hehehe) Java code because I truly believe at this stage that C# and the .Net libraries are woeful.

I admit, I've now around 3 1/2 hours of C# experience which clearly makes me an expert, NOT, but I struggle to see how you could take Java, and make it worse and less mature. They managed it though.

Ok, well now I'm screwed. `IEnumerator` doesn't allow you to remove from a collection whilst iterating. In fact it expressly says this isn't allowed. I've written a bunch of custom collection classes (for performance reasons) that I was hoping I could just ignore for now but looks like I'm going to have to convert them as well just to get the behaviour I need.

It's now 3.45am. Time for bed. My brain hurts. Back into it tomorrow me thinks. I've done around 45% of the code base in a couple of hours. Not bad. It's pretty easy. I wonder what the Microsoft conversion tool is like :-)

It's not too bad so far. A few quirky things here and there. It surely looks like they've tried to make all their existing developers happy by keeping lots of language constructs the same as those in Microsoft flavours of C/C++, VB, etc. I can understand that. In fact I think in some ways it's remarkable that they think that way about their developers. But still it's a bit of a heinz 57 varieties in places.
