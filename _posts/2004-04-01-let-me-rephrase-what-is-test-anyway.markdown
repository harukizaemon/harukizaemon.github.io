---
layout: post
title: "Let me rephrase - what is a test anyway?"
alias: /2004/04/let-me-rephrase-what-is-test-anyway.html
categories:
---
> A procedure for critical evaluation; a means of determining the presence, quality, or truth of something.

My [original post](/blog/2004/03/31/tests-and-refactoring) was (supposed to be) asking the question, when is it justifiable NOT to have a test (either new or modified) AND in particular what consititutes a test in the first place. It seems to me that many developers see refactoring as an excercise that somehow doesn't need tests. Also, the term `test` has become synonymous with `JUnit` and this seems to me somewhat short-sighted.

Unfortunately though, I think I failed to communicate this clearly. At no time did I suggest that tools would take over the world and do everything for me. In fact quite the opposite. It's especially the non-obvious forms of anything for which I want someone to write a test precicely BECAUSE I can't get a tool to check for it automatically.

Andy Marks suggested that maybe `rename` is one refactoring that doesn't require a test. I'm sure there are many others - `introduce constant`, `inline local variable`, etc. to name but a few - and though I probably agree that these are special cases on a simple gut feel level, I'm not convinced there has been a rigorous argument put forward.

Because, fundamentally, I don't believe you when you say it's an innocuous change :-) Hell I don't trust myself. And I think you'll find that the more experienced a developer becomes, the more sceptical he/she is about saying something like _"oh yeah that's trivial."_

It's like speeding in a car. You wouldn't speed if you thought you were going to kill someone right? You drive fast because you think you can handle it. And the speed _you_ think you can safely handle is probably lower than my 18 year old cousin even though you can probably safely drive faster because you have more exprience.

And therein lies the problem. Letting people arbitrarily decide when it's appropriate is dangerous especially on a team of inexperienced developers who by definition don't know what they don't know. But maybe just as much on a team of experienced developers because they believe they know everything ;-P

In light of that I think it's fair to say I want you (and myself) to have to "justify" the code change with a breaking test of some kind (preferably automated) if at all possible.

Take the example of double-checked-locking. If I see it, I want to remove it because it's a broken pattern. But I don't want to have to write a test for it cause that's pretty involved (like writing thread-safety checks!) So instead we have a machine identify them and on the basis that we all agree it's a bad thing AND the test for it (ie checkstyle) barfs, we are all happy to change the code.

So, can we consider this to be a test? If so, then I'm satisfied that I have a broken test and on that basis I am justified in making a modification to the code. After the change, the test no longer breaks and as long as no other tests broke as a consequence, we're satisfied.

On the other hand, if we don't consider it a test, where is your breaking test to change the code? And are we satisfied that we don't need one. Again, I don't believe you when you say _"it just needs to be changed and it's only a comment so it won't break a thing."_

From my perspective, the code base is a picasso - if you think you need to change something, you'd better have a damn good reason to do so. And just saying you don't like impressionist paintings isn't one of them :-)

Being forced to justify a change forces you to think rigourously about the underlying problem. Irrespective of delivering functionality, at it's core, that's why we believe TDD produces good quality code. And a bonus is that when you pair with a less senior person, you'll be forced to explain to them what you're doing. In fact as a junior developer, I won't let the senior partner in the pair get away with just telling me it'll be ok. I force them to stop and explain what it is they're doing AND make them write a test (first)!

And lastly, adding a test helps to ensure the issue will never return to the checked-in code base. Without the failing test, someone may decide to just change it back when they see a seemingly unecessary change or maybe even because they don't understand the change. At least with a test, it will force them to (re)consider.
