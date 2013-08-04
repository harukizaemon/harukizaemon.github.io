---
layout: post
title: "Suite Memories"
alias: /2004/07/suite-memories.html
categories:
---
One of the managers at the client I'm currently working for observed that I hadn't blogged quite so much recently as in the past and wondered if I was, perhaps, suffering _blogstipation_. But today I received a good dose of editorial cod-liver oil.

I mentioned some time ago our success in [speeding the build]({% post_url 2004-03-27-build-speed %}) with respect to JUnit execution times. That has worked sensationally well for us for some time now. Unfortunately, as more and more tests were added we started getting `OutOfMemoryError`s forcing us to generate multiple suites which in turn slowed the build.

 Our short term solution was to stop [instrumenting the build](http://www.jcoverage.com). This bought us some time but that was in no way considered a viable long-term solution. So finally today, after much distruption to the [cruise build](http://cruisecontrol.sf.net), we decided to get to the bottom of it. We already knew that [JRules](http://www.jrules.com) classes have some quirky behaviour that meant special care was needed to ensure it can free all working memory before being garbage collected. So we took a punt and looked at the tests involving rules.

On inspection, it did indeed look as if we had forgotten to clean up. Voila! No more memory errors. But then something struck me. I had just recently implemeted a solution to this for our mainline code for this very problem that ensured we were cleaning up in the `finalize` method of the same class we were using in our tests. This implied there was something else going on. Something most peculiar.

Unconvinced we had found the real cause of the problem, we decided to back-out the change and try something else. This time we made sure that the objects being used were candidates for garbage collection by explicitly setting the instance variables to `null` in the `tearDown()` method. We ran the tests again and bingo! Problem solved. But hang on a second. Surely the test classes themselves are being garbage collected? Surely clearing the instance variable was redundant?

A bit of digging around and we were soon scruitinising the JUnit `TestSuite` source code. And most fascinating reading it was too. Right there in plain Java for all the world to see: A `Vector` (how Java 1.1); a constructor that creates and stores (in said `Vector`) an instance of the test class for each test method it finds; and; a `run` method that simply iterates over an `Enumeration` of the instances, running each one in turn.

No wonder we were running out of memory! The bloody test class instances, all 2000+ of them, and the data they were holding on to were never released until the suite had finished.

A bit of decoration and a custom test suite class later and it's bye bye to our `OutOfMemoryError`s. I wonder what the next _gotcha_ will be?
