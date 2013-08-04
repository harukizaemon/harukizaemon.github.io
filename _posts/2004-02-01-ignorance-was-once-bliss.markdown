---
layout: post
title: "Ignorance was once bliss"
alias: /2004/02/ignorance-was-once-bliss.html
categories:
---
It's always nice to find something about which I know bugger all. So today, instead of my usual rhetoric, magic answers and dubious words of advice, I roll on my back and display my soft under belly.

You see, I'm trying to make a class thread-safe. Almost all of my classes are inherintly thread-safe by virtue of the fact they are immutable. This class however updates internal data structures such as `Maps` and `Sets`.

Although `Maps` and `Sets` can be made internally thread-safe (by calling `Collections.synchronizedX(anX))`, that doesn't really help when performing multiple calls (`add()`, `remove()`, etc.) on multiple structures (including instance variables) and treating them as one atomic-ish operation.

Now, I don't want to just put synchronization blocks everywhere and hope for the best. I want to validate that the class is actually thread-safe. But I soon realised I have very little idea how to unit test for concurrency and thread-safety.

A quick search of the 'Net turns up a few interesting links:

* [GroboUtils](http://groboutils.sourceforge.net/)
* [Identify Problems with Testing Multithreaded Code](http://www.ftponline.com/javapro/2003_11/online/rnettleton_11_26_03/)
* [JUnit best practices](http://www.javaworld.com/javaworld/jw-12-2000/jw-1221-junit_p.html)

But these only address the problems associated with running multiple threads within a test. Unfortunately,  simply running multiple threads doesn't actually prove that we have achieved thread-safety.

Because of the non-deterministic nature of thread scheduling, it is very difficult to ensure that we have had multiple threads accessing a single object concurrently, save adding hooks into the object itself to make it sleep or give up control at strategic points in the code.

Immediately, my brain kicks into golden hammer mode and decides that this problem looks like a [BCEL](http://jakarta.apache.org/bcel/) or [ASM](http://asm.objectweb.org/) nail. But, being the naturally lazy git that I am, writing code or having to deal with [AspectJ](http://www.aspectj.org) doesn't really appeal to me right now. Nor can I come up with any heuristics to determine where I would inject code anyway.

As you all know by now, I scoff at the idea that something is [too hard to test](/blog/2003/12/07/my-stuff-is-too-complex-to-test). That said, I'm still left pondering how the hell I'm going to validate that my code is thread-safe? More to the point, how am I going to (re)design my class so that it's easy to test?
