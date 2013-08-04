---
layout: post
title: "Exceptionally Challenged"
alias: /2004/01/exceptionally-challenged.html
categories:
---
I've finally finished my [foray into C#](/blog/2004/01/19/my-foray-into-c) and I suppose it would be obvious to all that I was rather less than impressed by some "decisions" that were made by the C#/.Net development team(s). Most noteably, the lack of checked exceptions.

So it was with much joy that I stubled upon [this article, on the Artima web site](http://www.artima.com/intv/handcuffs.html). An interview with Anders Hejlsberg, the lead C# architect and a distinguished engineer at Microsoft, on "The Trouble with Checked Exceptions".

Fantastic I thought! At last I'll get some sensible, logical, coherent and rational explanation for some of the stuff that feels so uncomfortable to a Java weenie such as myself.

If only.

Thinking that maybe it was just me, I forwarded the link on to a good friend of mine James Ross who knows quite a lot about the Microsoft world. But alas, he drew similar conclusions. (Portions of our conversion have been included here)

No, it is sad to say but Mr. Hejlsberg shows his true colours and in the process makes me even less impressed with .Net.

I'm usually not fond of taking an argument and disecting it line-by-line. It's often too easy to take stuff out of context and often leaves one open for a counter argument in a similar vein ultimately leading to a flame war. But on the basis that Mr. Hejlsberg wouldn't know me from a bar of soap let alone read my blog, why not.

So without further ado:

> ...I completely agree that checked exceptions are a wonderful feature. It's just that particular implementations can be problematic. ...I think you just take one set of problems and trade them for another set of problems.

So he likes them but thinks that it's the implementation of them that's wrong. Um...I must be really stupid but HOW ELSE DO YOU IMPLEMENT CHECKED EXCEPTIONS? I can't think of a simpler way than in Java. In fact I can't think of any other way really. Please enlighten me!

Skipping foward a bit, he reckons

> The concern I have about checked exceptions is the handcuffs they put on programmers ... It is sort of these dictatorial API designers telling you how to do your exception handling.

Well how about being dictatorial about what methods you can override? In C# a method cannot be overriden unless you declare it as virtual.

He then goes on to say:

> Let's start with versioning, because the issues are pretty easy to see there. Let's say I create a method foo that declares it throws exceptions A, B, and C. In version two of foo, I want to add a bunch of features, and now foo might throw exception D. It is a breaking change for me to add D to the throws clause of that method, because existing caller of that method will almost certainly not handle that exception.

Dude, try making an API that will be able to respond, like wrapping those FOUR DIFFERENT EXCEPTIONS into one abstract package-level exception, you clown! This argument doesn't stand up to scrutiny because the same thing can be said about method parameters and return types, so why not get rid of them too!?

In fact he says:

> C# is basically silent on the checked exceptions issue. Once a better solution is known - and trust me we continue to think about it - we can go back and actually put something in place...And so, when you take all of these issues, to me it just seems more thinking is needed before we put some kind of checked exceptions mechanism in place for C#.

Right! You're somehow going to go back and change the way exceptions work!? Get real my dear architect. Whatever happened to your argument about versioning issues?

> ...in a lot of cases, people don't care. They're not going to handle any of these exceptions. There's a bottom level exception handler around their message loop. That handler is just going to bring up a dialog that says what went wrong and continue. The programmers protect their code by writing try finally's everywhere, so they'll back out correctly if an exception occurs, but they're not actually interested in handling the exceptions.

AHA! Now we begin to see the truth. He doesn't actually like catching exceptions after all. I dont know what planet he is on but really.

I can just see my nuclear power station monitoring system catching a `CoreMeltDownException` in the "main message pump" and bringing up a dialog kindly informing the operator he might have a problem. Meanwhile, blissfully unaware, the rest of the system continues on removing the control rods from the core.

And what's with the `finally's` everywhere to "protect their code"? Holy cow. Don't let this man near any system I'm likely to work on. I don't know about you but I rarely need to write `finally` statements except maybe in some integration code. Who manages resources this way these days? I'm not saying I dont use them but my code surely isn't as littered with them as he implies it would.

Then he loses all credibility by delving into some spurious arguments on "scalability":

> The scalability issue is somewhat related to the versionability issue...Each subsystem throws four to ten exceptions. Now, each time you walk up the ladder of aggregation, you have this exponential hierarchy below you of exceptions you have to deal with. You end up having to declare 40 exceptions that you might throw. And once you aggregate that with another subsystem you've got 80 exceptions in your throws clause. It just balloons out of control.

Get out of my face! I've never EVER seen this, even in the worst code bases I've had the misfortune to work on. For a start, the numbers he quotes are just ludicrous. But again I say, how about wrapping those FOUR DIFFERENT EXCEPTIONS into one abstract package-level exception!? How about designing a language and libraries that allow smart people to do smart things instead of one that makes it even easier for stupid people to do stupid things?

What's probably even worse, is that now my `SQLException` propogates all the way from my database layer to my GUI layer. Some "clever" developer realises that he can catch the `SQLException`, check the `ErrorCode` for `9901` which he happens to know means a key violation (or whatever) and display some nice message to the user. Whatever happened to encapsulation and abstraction? I mean, I know [abstractions can leak](http://www.joelonsoftware.com/articles/LeakyAbstractions.html) but this is Niagra Falls baby!

> But that said, there's certainly tremendous value in knowing what exceptions can get thrown...

Excellent. Well at least we agree on something. Damn shame that because C# has no way of declaring what exceptions are thrown, I have no way of knowing if there are any to be caught, let alone what they might be. Unless of course it says so in the documentation. Documentation gets out of date VERY QUICKLY.

When I started my porting effort, I was using the [mono](http://www.go-mono.org) doco which is still incomplete. This meant in some cases I had no idea if nor what exceptions might need to be attended to. What's even worse, none of the exceptions seem to extend any sane base class so if I decide I need to catch more than one but treat them all the same way (say some kind of `IOException`) I have a 3 `catch` clauses!

> But I think we can certainly do a lot with analysis tools that detect suspicious code, including uncaught exceptions, and points out those potential holes to you.

Ahhh. Of course (slap myself on the head) why didn't I think of that? After all what we really need is yet another tool! In fact let's create an AOP Library for C#. Yeah. Then we can inject code into existing libraries to catch exceptions...the possibilities for this are endless ;-)

But seriously, exceptions form part of your API just like methods, interfaces, parameters, abstract data-types, etc. If you think about them in this way, they stop being scary and start being useful.

Some things to remember when using exceptions:

* Don't use exceptions for flow control;
* Create sensible exception heirarchies;
* Never throw more than one class of exception from a method unless you are forced to. That is, if you throw more than one type of exception, make sure they all extend a common base class. That way clients can catch them and/or re-thrown then easily;
* Avoid throwing someone elses exceptions. Eg. Don't throw SQLExceptions from your middle-tier. Wrap them. There is usually no reason for the GUI to know there was an SQLException.

