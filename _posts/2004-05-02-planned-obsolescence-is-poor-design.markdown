---
layout: post
title: "Planned Obsolescence Is Poor Design"
alias: /2004/05/planned-obsolescence-is-poor-design.html
categories:
---
Or, _interfaces are no excuse to build broken software._

I've ranted about this [previously]({% post_url 2004-04-02-sometimes-the-problem-does-just-go-away %}) and I see the problem recurring so often I feel compelled to have another go but this time from a slightly different perspective.

Lets start by agreeing that it's best not to be [constrained by someone elses API]({% post_url 2003-11-23-dont-mock-infrastructure %}). They introduce constructs and ways of "doing stuff" that don't conform to my ideal. Assuming we do agree, it then follows that mocking out someone elses API is not only a bad idea but usually plain pointless.

Enter Java's interfaces and C++ pure-virtual classes. They are truly wonderful things. You don't have to write any "real" code just to have one. No code means no tests required. And best of all, they provide functionality just the way you want it. They're the ideal black box. Not as ideal but potentially almost as good, are the methods themselves. Even concrete implementations can and should provide me the ideal functionality.

So what do I mean by "ideal"? Well, say I'm implementing a bit of code and all of a sudden I realise "doh! here I'm going to need to do X" where X is conceptually simple but implementation complex functionality. I have a few choices: I could go straight into implementing X; or; I could choose to "pretend" that X is actually implemented _exactly_ the way I want it to be.To achieve this I can either add a method to an existing class or I could introduce an `interface`. Either way, the important point is that I want this thing to do what I need in the ideal way. I don't care that it doesn't exist yet. But I don't want my code polluted with knowledge of how it's actually going to be implemented. I just want to call it and have it do it's magic.

So we "stub" it out and push on. Eventually we finish our lovely piece of code. We stand back and it looks good. It's readable. It's understandable. But there's still that X we haven't yet nutted out. No problem. We can still test the class using mock objects so we can still maintain the illusion that X actually exists.

At some point we will feel the pain. We need to actually implement X so it can be deployed as part of the application. So now let's say that X _really_ has the potential to be stupidly complex but for now we don't actually need to solve every possible case. We create a `SimpleX` or `DefaultX` or my much despised `XImpl` class. What we don't do is create a `MockX` class simply because it's "not the real thing". What does "not the real thing" mean anyway? It's there. It's working code. It does what I need. It may not be as flexible as I want it to be but it's NOT A MOCK!

And this is where the title of the entry hopefully becomes apparent.

As far as I'm concerned, every line of code I write IS PRODUCTION CODE. Period! If the customer was happy with the functionality of the system, they could disband the team and deploy the application right now and it _would be_ production code. Let me repeat. EVERYTHING IS PRODUCTION CODE.

Code I write is rarely written with the intention of one day throwing it way! It's a production implementation of the limited functionality that I need this minute. If I need more functionality at some point, I'll add it. This may require me to intoduce more abstractions, more interfaces, more methods that potentially do as much as required to get the job done. I'm pushing the unimplemented bits out as far as needed so that my overall design continues to adhere to my ideal. This i's not to say it won't or can't be binned at some point but that occurs as a consequence of an evolving application. It happens to any piece of code whether I actually decided upfront that it was likely to be binned or not.

There is always [The Law of Leaking Abstractions](http://www.joelonsoftware.com/articles/LeakyAbstractions.html) to consider. But. by way of the obligatory analogy, just because there is the chance someone might run into my car, shouldn't prevent me from actually driving. It just means I need to be vigilant and aware of what's going on around me when I do so.

And because I'm sure I won't have made any sense whatsoever up till now, I'll confuse you even more with a real example.

Take [pico container](http://www.picocontainer.org/). Hold for the moment your predjudices on IoC/Dependency Injection/etc. That's irrelevant to the point. The point is that pico container provides a very simple implementation of an interface. No bells, no whistles, no loading configuration from files. Everything is configured by code at run-time. But it does the job. It's just not as flexible as maybe I'll need down the track. But if it''s all I need right now, then it's, well, all I need.

Then I decide that what I need is something that can be configured from an XML file. Do I a) throw away all of pico container because it doesn't do what I need; b) cut and paste the pico container code into a new class, re-using as much as I can, or c) just simply add the new functionality (in this case by way of [nano container](http://nanocontainer.`haus.org/))?

I'll let you be the judge but anyone who chose to have written pico container as throw away code because they knew one day they'd actually need to load configuration from files and therefore it was throw-away code deserves to be forced to program in binary for the rest of their natural life.

(BTW, I didn't write pico or nano container I'm just using them as part of an example. That's why it's called an example)

Yes, it is true that things aren't always ideal. But this is very rare in my experience and will only occur at the hopefully thin boundaries between your code and someone elses API! If this isn't the case, then WTF are you doing? It's your software. You created it. How is it that it doesn't at least _pretend_ to do exactly what you want?

FWIW, This discussion is really a demonstration of how I choose to interpret XPs _do the simplest thing that could possibly work_.
