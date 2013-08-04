---
layout: post
title: "My foray into C# - Part II"
alias: /2004/01/my-foray-into-c-part-ii.html
categories:
---
Well it's just gone 7pm and I've pretty much finished the conversion of [Simian](/simian) to C#.

All in all it took around 8 hours to convert 2000 lines of real source (around 6,500 raw source lines) of Java code to C#.

I haven't quite finished due to the blatently stupid file/directory handling in .Net. But I'm almost there. I've hard-`d it to run from the current directory for testing purposes. But 99% of it has been completed. Even the output is identical which is nice to see.

Performance? Well again, I stress I'm running under mono on gentoo so that's probably not the best test but it seems to take around 50% longer to run and consume around 50% more memory than the Java version. I'll have to run some tests on windows and see how that performs.

Some, hopefully,  interesting bits to add to my last blog...

`Case` statements don't allow you to fall through to the next one if you have defined any code for it. Instead you have to explicitly say you want to `goto` the particular case you need. Hmmm...not sure about that.

C# doesn't allow you to put array specifiers `[]` after the variable name. It only allows them to go after the type. This is good. Only because it's the way I code anyway but I did find some code that somehow managed to slip through unnoticed.

What's up with structs? Sheesh! How are they in any way different to an Object except in the underlying implementation (ie they're probably allocated on the stack or something)? More warm and fuzzies for the C fraternity? Get rid of them I say.

Marking something as `readonly` doesn't actually seem to mean you must have assigned it a value. This may be a quirk of the mono compiler. I don't know. But if not, that's just wrong. The thing I love about `final` in Java is that it stops me from accidentally forgetting to assign a value to something. Took me 10 minutes to track down a `NullReferenceException` because I had accidentally deleted a line (an assignment) from a constructor. Using `const` is sometimes an option but it has a few caveats: `const` can only be used if you want a static variable; and; more importantly, the value has to be known at compile time! This pretty much means it's only useful for simple, primitive values assigned in the declaration. I can't use `const` for say a collection or an instance of any object that requires `new`. Nor can I use it for values assigned in constructors. Which is exactly the problem I had.

`instanceof` becomes `is`. How nice of them to simplify something I hardly ever use and generally consider poor practice anyway LOL.

I started off thinking that having to specify override for things was a good thing. Now I'm not convinced. Basically, if I haven't thought that someone might want to override a method I'm pretty much screwed. Well they are. Luckily this is my own code base so I can refactor all I like. Shame about all those libraries and frameworks out there. If in doubt, you could always mark the methods `virtual`. Now that's all very well and good but if the majority of the time I want stuff marked as `virtual`, wouldn't it make sense to have that be the default? I think I'll need a macro in my IDE for all these keywords: v[TAB]; ro[TAB]; etc. hehehe. But again, maybe it's just what I'm used to?

Don't get me started on the `FileInfo`, `DirectoryInfo`, `File`, `Directory`, yada yada yada. Even the regular expression libraries seem to have all these extra, essentially static, classes. The .Net libraries look like they were designed (and I use the term loosely) by high-school students. First year CompSci students at best.

`Readers` become `TextReaders`, `Writers` become `TextWriters`. In and out are reserved words! probably should have named my parameters something more meaningful anyway?

Auto-boxing? Didn't use it. Well not that I know of. That's the problem. I have no idea. Scares the hell out of me. Let's see, we want a language with lots of optimizations to keep C/C++ developers happy so they can feel like they're writing on to the metal, but for everyone else, lets make it really easy to not know you're creating bazillions of objects all over the place. Hmmm...

All in all it was really easy and relatively painless. I started from nothing and probably still know nothing but it all seems to work. I can't help but think that even in the early days of Java, the libraries may have been less functional, but they weren't all over the place. More to the point, remember that C# came from Java (oh go on tell me it didn't) so in my opinion there are no excuses. What, since C# started, a myriad languages have popped up that are at least as functional (probably more so) and far easier to use.

So, whilst it's an interesting thing that we'll all have to get used to, I'm happy in my Java land for the forseable future.

Time for a G&T to top off a good afternoon.
