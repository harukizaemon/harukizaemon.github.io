---
layout: post
title: "When is a String not a String?"
alias: /2005/02/when-is-a-string-not-a-string.html
categories:
---
So I've been stumbling along writing this book and today I wrote some code where, rather than using good old `String`s, I instead chose to use [`CharSequence`](http://java.sun.com/j2se/1.4.2/docs/api/java/lang/CharSequence.html)s. Why?

Well for one thing, I wanted to be able to process both `String`s and `StringBuffer`s alike and, thanks to JDK 1.4 and JSR-51, that's possible as they both implement `CharSequence`- very nifty indeed. More importantly though, I wanted to be able to process data from files as well as in-memory.

Now, admittedly I could have written my code to process `Reader`s or `InputStream`s instead and used a `StringReader`, etc. But really, when it came down to it, the algorithm I was writing suited strings and characters not bytes and buffers and all that hoo-ha and I wanted to keep it that way.

So, I thought to myself, I would do the opposite. I would write a `CharSequence` that wrapped a `Reader`. That shouldn't be too difficult now should it? But then another thought occured to me. Before I leap into writing a whole bunch of code that I'd need to test and *gasp* maintain, maybe someone else had already done the hard work for me; maybe even the JDK!?

A quick flick of the wrist and [IntelliJ](http://www.intellij.com) brought up a class heirarchy for me and what do you know? along with `String` and `StringBuffer` was the little used [`CharBuffer`](http://java.sun.com/j2se/1.4.2/docs/api/java/nio/CharBuffer.html) from the [NIO](http://java.sun.com/j2se/1.4.2/docs/guide/nio/) package. Another little present courtesy of the JSR-51 group.

A `CharBuffer` facilitates, among other things, memory mapped I/O, meaning you can address a file on disk as if it were a contiguous array of characters in memory. And, because it conveniently implements `CharSequence`, can be passed into my code just as any of the purely in-memory implementations.

The other thing I like about `CharSequence` is that it's an interface. This makes it possible to decorate them and gather performance statistics really easily. Yes, yes, all you [AOP](http://aosd.net/) weenies sit down and stop waving your arms about. I know your new (ok not so new anymore) fandangled whizzbang toy can do that too. But I don't have to learn a new language or tools to do it. One day perhaps but not this one in particular. Believe me, my paradigm has been shifted quite enough and I'm giving it a rest for a while ;-)

The only thing missing is that, surprisingly, the `Character` class doesn't implement `CharSequence`. I'm not sure how useful that really would be but I like the idea if only for completeness sake. It certainly wouldn't be hard.

Anyway, I think I'll be looking for opportunities to use my new best friend where I would usually have used a `String`. The only caveat is that a `String` is guaranteed to be immutable while a `CharSequence` isn't. Oh and you can't be guaranteed that `equals` will work between them either. No matter, I can still see the glint of those shiny golden nails from here :)
