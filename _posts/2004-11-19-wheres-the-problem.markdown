---
layout: post
title: "Where's The Problem?"
alias: /2004/11/wheres-the-problem.html
categories:
---
One of the biggest problems I see over and over again is the difficulty support and maintenance teams have diagnosing problems. It's unfortunate but developers have a knack of writing code for _Happy Days_ scenarios - when the software works, it works well; when it fails, it can be a disaster.

A [cow-orker](http://c2.com/cgi/wiki?CowOrker) and I were recently discussing the use of [assertions](/blog/2003/12/27/be-assertive) in production code. He had previously been discussing the topic with one of his colleagues who had suggested that their existence was a smell; that it indicated a lack of testing.

Now, enough has been said on the topic of unit testing so suffice it to say that the great thing about unit testing is that it's easy to ensure your components work as advertised. You can even, and in many cases should, test what the behaviour would be given invalid input such as `NULLs`, etc. Then of course we move into integration, functional, acceptance, regression, etc. testing to prove that the application hangs together as a whole.

The problem is that tests don't necessarily prove that the software does what it's supposed to *GASP!*. Rather, tests prove that software works for the given scenarios and the assumptions made and the plain fact is that these do not always match reality. We may have the greatest, most comprehensive test suite in the known universe, but if it's testing the wrong things, it matters little. Sure, in a system over which you have complete control, high levels of functional/integration test coverage can compensate but even then, even 100% code coverage doesn't equate to 100% accuracy. In particular, at best, it is difficult to test for and therefore prevent clients of your code from passing invalid parameters. In fact if you've ever written and published a public API you'll know that, by definition, it's impossible.

The further into a system a problem propogates, the more difficult it becomes to diagnose and the greater the likelyhood of "damage". One of the major benefits of production code (as opposed to test code) assertions is that at run-time we can detect and prevent unexpected scenarios as early as possible, thereby preventing them from propogating. Maintenance developers and those familiar with the [Fail Fast](http://c2.com/cgi/wiki?FailFast) axiom will appreciate how important this is in a production environment.
