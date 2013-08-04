---
layout: post
title: "Broken Tests"
alias: /2004/03/broken-tests.html
categories:
---
As I mentioned, I've been doing some code cleaning up as I add new functionality to the system I'm currently working on. I have to agree with Andy Trigg and say the delete refactor is one of my favourites. It's gratifying to replace great swathes of code with something much simpler and yet more functional. The best is when I refactor some code that allows me to delete a test. This is usually because responsibility for some behaviour has been smushed over a number of classes which leads to duplication among several test classes.

If it weren't for all those failing tests, I'm sure I would have screwed up pretty early on. As I was fixing tests here and there, I noticed something interesting. Some of the tests were breaking due to assertion failures. Just what you would hope for. However, rather more were failing due to exceptions. Usually a `NullPointerException`. What's worse is the NPEs were in the test case code, not code the test was calling.

Tests should `assert` expected behaviour. If you expect a return value not to be null, be explicit about it. If you expect an iterator to have more values, assert it. Don't wait for the underlying code to blow up in your face unless you're actually testing for that behaviour of course.

I also like to keep tests as small, simple and obvious as possible. If a test case checks for a limited set of conditions, its much easier to work out where to fix the code when it breaks. Heavily factored tests may appear small, but if you were to inline all the test code, you'd find that it's definitely not simple nor obvious.

What I've found most interesting is that the worst tests are generally the ones written after the fact. The _"oh yeah I'm gonna need to test this"_ kind. They stand out like the proverbial k9's gonads. They're usually long (the tests not the testes), difficult to follow, test more than just a few simple concepts and tend to be very brittle. That last point seems counter intuitive at first but is definitely backed up by experience.

So what's the point? Tests aren't just for keeping [jCoverage](http://www.jcoverage.org) happy! In fact crappy tests are a liability. They will end up being a maintence nightmare if left unchecked (pardon the pun).
