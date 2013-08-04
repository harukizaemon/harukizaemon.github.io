---
layout: post
title: "Sometimes crappy tests are better than none at all"
alias: /2004/04/sometimes-crappy-tests-are-better-than.html
categories:
---
Some of the arguments against the tsting barrow I push are that you end up with essentially "legacy" tests. Tests that you have to continually maintain. And often tests that are brittle.

I was having a discussion with a colleage the other day and he asked me if I thought it was really necessary to test everything given developers often write crappy and brittle tests anyway. Whats worse is that inexperienced developers often go through unecessary hoops to build the tests and/or production code and possibly end up writing software that is just as bad (or maybe worse?) than if they hadn't tried to do test first (or at all).

I couldn't believe it but you know I'd never really thought about it too much. I had just believed my own BS and assumed that, yes, you need tests for as much as you can possibly test.

And you know what?

I still believe it!

Crappy tests almost always (I can't think of an exception but there invariably is one) imply crappy code. That means that the brittle tests are likely testing brittle code. Now, brittle tests are bad because when I make my seemingly innocuous change, I invariably end up breaking some of these brittle tests when really it shouldn't have. But then again, if I assume brittle tests means brittle code, the chances are I really have broken the underlying code! And how would I have know that if the tests hadn't been there in the first place?

So I still stick by the mantra. Test everything. Even if (especially if) it's crappy and brittle 'cause you'll be doing yourself and everyone else on the team a favour by ensuring they know when they've broken your code.

I leave developer re-education/career re-assignment as an excercise for the reader. Though I hear a 2x4 comes in real handy. I have the scars to prove it ;-)
