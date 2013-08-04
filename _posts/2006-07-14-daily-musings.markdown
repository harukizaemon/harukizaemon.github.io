---
layout: post
title: "Daily Musings"
alias: /2006/07/daily-musings.html
categories:
---
Be warned, this is somewhat of a stream-of-consciousness even if a little edited.

I was sitting at work today pondering, among other things, some particularly awful code that we happened upon -- so what's new? The code in question relied upon multiple columns in a table to hold configuration information when really what was needed were multiple rows -- something like a properties file but in a database. I then started to think that I pretty much always start out wanting my data to be "properly" normalised without regard to performance considerations. As Knuth is oft-quoted as saying, "premature optimisation is the root of all evil."

One thing I really like is when a development environment allows me to easily layer on performance enhancing behaviour such as caching, without unduly interfering in the core design of the system. In a Rails for example, I can build a completely dynamic application and strategically add caching where necessary for content that is, for the most part, static. If the data on which that content relies changes, expire the cache and it'll be re-generated.

At the same time, I was pondering why it is that I like Ruby so much and why I believe it really makes development environments such as Rails possible; in a way that Java just doesn't. Although a topic for another blog entry, one of the reasons I considered was it's dynamic (ie non-compiled) nature. That I can change a piece of code and immediately run it without a compilation step is, IMHO, a big factor. (I'll grant you that Eclipse goes a long way towards achieving this for Java with its continuous compilation but I feel there is more to it than just that.)

It then occurred to me that compilation, or, more specifically, the translation of source-code into some executable form and stored in some more persistent manner (such as files on a hard disk) is really a form of caching.

To see what I mean, imagine you started with a fully, 100% interpreted language where each statement is re-parsed each time it is executed. Ie. no caching. To this you then decide to convert the source-code into an intermediate, in-memory, form to remove the need to re-parse the source-code each time. This is a first-order cache. Finally, you decide to write this intermediate form to a more persistent medium so as to remove entirely the need for conversion from raw source-code to an executable form. This is a second-order caching. You could of course further translate the intermediate form into native machine code or any number of intermediate formats. And, of course, once in memory, the intermediate format could be dynamically translated into machine code -- such as a Just-In-Time compiler or Hot-Spot compiler does.

The point I'm trying to make however is not _how_ efficient one can make the execution process but simply that it is entirely possible to execute the raw source-code with a very simple parser and interpreter. The fact that we choose to further translate that source-code to forms specifically designed to improve execution efficiency is an optimisation and, in many ways, not so different to storing the output from a Rails action on the server, or a web browser caching an image or an HTML page.

Where the analogy breaks down of course, is that compilers often provide other benefits such as detecting poor or incorrect use of the langauge. That said, some of these benefits are lost -- or for that matter not even useful -- when using languages such as Ruby and yet other benefits might be found in static analysis tools.

So, what does this all have to do with anything?

That's a very good question to which the answer may well be "not very much at all" however, I did find it interesting to ponder, as one does ;-)

One thing that I have noticed though, speaking of source-code, is there exists a distinct difference in the way the Ruby versus Java communities treat the sharing of code.

In the Java community, sharing of code is largely done by means of pre-compiled libraries with accompanying documentation. Sure, we have open source projects which make the source-code freely available for (ab)use. But, and I admit to having been [guilty of this myself](/blog/2004/10/27/when-corporates-embrace-open-source), the general feeling is don't modify the source unless you really need to; treat the binary distribution as a black box. And perhaps for good reason?

In the Ruby (and apparently Smalltalk) community, the philosophy is more one of source-code sharing. Need a Rails plugin? download the source-code into your source-tree; Looking for some missing Ruby functionality? Native extensions aside, what you'll get is a bunch of source-code. In most cases the source-code also comes annotated with RDoc -- the Ruby equivalent of Javadoc-- but even still, being confident to look into the source-code is, IMHO, a bonus.

Of course, there are those who would argue that sharing of raw source-code isn't always a commercially viable option and perhaps they're right.

In any event, I'm not sure I have enough experience in either yet to have formed an opinion as to which (or even if) one approach is better than the other but I do wonder if the fact that one is primarily treated as a compiled language whereas the other is essentially treated as interpreted leads to at least some of the differences in culture that I see or if I have cause and effect around the wrong way.
