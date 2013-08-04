---
layout: post
title: "re: Spiking and TDD"
alias: /2004/03/re-spiking-and-tdd.html
categories:
---
Jon - Six Million Dollar Man - Eaves raises some interesting points on [spiking versus TDD](http://www.eaves.org/blog-archive/000053.html). A very good summary on a tried and true approach for learning new stuff. While I agree totally with the article (I do this myself quite often so it must be good hehehe), I've really started to try and use JUnit as a mechanism for doing spikes as well.

Just recently, as part of a new 3-month gig (many thanks to David Pattinson and [James Ross](http://www.redhillconsulting.com.ay/blogs/james) and much to the obvious displeasure of one who shall remain nameless) I started doing some investigation into [ILog JRules](http://www.ilog.com/products/jrules/), something I had never used before. My primary responsibility was to mitigate any technical risk associated with using JRules. Naturally, testability was one of the main concerns for the team.

Now usually in this kind of scenario, one would usually whip up a quick-and-dirty main method, and start writing some code to both learn about JRules and to prove what it can/can't do. Instead, I started off by writing a test. Of course I had no idea what the test would do but I wanted to force myself to do it anyway.

At first it was a very simple test to work out what jar files I would need to just get the engine up and running. Then as I had more and more dicsussions with various team members about what our overall requirements would be and we thrashed out the different ways we could achieve the same thing with JRules, I simply added more and more sophisticated tests to test out our assumptions.

Doing my spiking as fully-blown tests, forced me to think much more critically about what I was doing rather than just spew stuff to stdout and look at the results. I was forced to clearly state my assumptions and the tests proved conclusively if these assumptions were right or wrong.

The great thing now is that we have a full compliment of _integration_ tests that both prove our assumptions about, and demonstrate our chosen use of, JRules. This will be extremely valuable when the rest of the team start to write their own rules as part of implementing user stories. It also means we can happily upgrade to newer versions of JRules if/when they are made available with much less worry about breaking our code. This is of course why you all write integration tests isn't it ;-)

A few days ago, James and I did much the same thing when [doing some swing development](http://www.redhillconsulting.com.au/blogs/james/archives/000130.html). I can't say I've perfected it. I still do some debugging and println but in the context of writing a test. But, I'm forcing myself to be as "pure" about it as I can to see how far it's practical to push the technique.

As a complete aside, I found JRules pretty damn easy to use and test. The documentation is very good. If anyone is interested, I'll post a brief overview.
