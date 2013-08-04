---
layout: post
title: "Leaking Locks"
alias: /2007/03/leaking-locks.html
categories:
---
Spoiler: JDK 1.5 -- and near as I can tell 1.6 even though it's supposed to be fixed -- have a pretty fatal [memory leak](http://www.bright-green.com/blog/2006_10_24/beware_the_memory_leak_in.html) when using `java.util.concurrent` libraries.

How do I know?

Well, besides finding the earlier link via our good friends at Google, we recently switched from using the [backport](http://dcl.mathcs.emory.edu/util/backport-util-concurrent/) to the standard JDK libraries and the throughput of our application under load rapidly approached zero.

After spending some time creating a a quick-and-dirty load test on my own machine, I found I could easily replicate the problem with the added bonus of `java.lang.OutOfMemoryError`s in under a minute. Running it under a profiler confirmed as much with the culprit being `java.util.concurrent.locks.AbstractQueuedSynchronizer$Node`.

"Never" cries the guy we work for. "Doug Lee's stuff has been around for a long time and I'll bet his stuff works and you guys have screwed something up!" he continues.

We all agreed and decided to replace the locking code in the class I'd just created (and presumably screwed up) with some good old-fashioned synchronisation.

Nope. The problem persists but it's not as bad this time. We clearly "fixed" something but not everything. Hmmm.

On further investigation we realised there was still some code in the call chain using the concurrent stuff so we decided, just for laughs, to revert back to using the concurrent stuff but use the backport libraries instead.

Voila! Suddenly the load test exhibited exactly the expected behaviour: no out-of-memory errors; no grinding to a halt; nice see-saw memory usage; and using only 2MB of heap instead of, well, as much as it could get (>64M in our case).

Stunned, we went back to the default JDK version. Yup! Problem is back and not just in our own locking code either, we noticed the problem in much of the concurrent classes including the collection implementations.

The good news is that using backport we now have a build that works.

The scary part is that I can only presume some Sun engineer decided he knew better and screwed the pooch right royally on this one.
