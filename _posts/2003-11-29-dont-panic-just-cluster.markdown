---
layout: post
title: "Don't panic, just cluster"
alias: /2003/11/don-panic-just-cluster.html
categories:
---
I encountered a rather (un)amusing situation just recently. A project I was casually checking in on (just because I'm a nosey parker) had a unit test suite take 70 mins to run? "How many tests are there?" I ask. A couple of thousand I thought to myself? What if I told you 200!

Ok, so now that you've picked yourself up off the floor. You're first thought would most likely be similar to mine: Aha! They're hitting the database in their unit tests. Well how about this, they were hitting the database not just in every test, but setUp() and tearDown() are rebuilding the database by loading a schema and data from XML and re-populating it.

But not to fear, they're on to it! They're doing some work to speed up the tests. They've figured that if they create some extra database users they can run the tests in parallel!

Reminded me of another project I saw some time ago. The (web) application services around 2000 users concurrently. The application craps out so often that they decided to have a dozen or more servers all regularly saving session state to a database (read SLOOOOOW) so that the users would experience no perceptable loss of service.

So what do these have in common? In both cases, instead of looking at the design of the application. Instead of cleaning up the code and making it more reliable. Instead of reducing dependencies between layers. The answer was to cluster. I mean, why build better software when you can just throw more hardware and software licenses at the problem....KACHING$$$$$.

In the case of the testing, had it not ocurred to anyone not to hit the database at all? How can a unit test hit a database? Wouldn't that classify, by definition, as an integration test?

As for the web app, it seems to me that had the code base been even remotely stable, the claims made by the application server even remotely accurate, add in a UPS and some RAID and the application would have been more stable than the average users internet connection.

Now don't get me wrong, like all things there are times when you want to service 1000s of customer concurrently and you may well require lots of hardware. But the overriding factor must be to get the application right first. Then think about clustering. The old saying goes "A chain is only as stong as its weakest link." If the application is broken in the first place, you will get ever diminishing returns my simply adding more and more servers.

It seems to me that in most cases, it's rare to see a business application that warrants true (real-time session failover) clustering rather than just a simple server farm. I agree that a cluster and/or a server farm gives you the ability to to take a machine out for servicing, handle hardware failures, and to scale an application to handle more and more concurrent users. But it should not be used as a quick fix for poor software development.

Endulge me as I make a tenuous analogy with my martial arts training. Most martial arts teach that it's not about strength. It's about technique. Get your technique right first without resorting to brute force. Then when you can make that work, add in your natural strength and you will seriously kick butt.
