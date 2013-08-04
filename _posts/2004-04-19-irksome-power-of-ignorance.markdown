---
layout: post
title: "The Irksome Power of Ignorance"
alias: /2004/04/irksome-power-of-ignorance.html
categories:
---
> Scarcely any degree of judgment is sufficient to restrain the imagination from magnifying that on which it is long detained. -- <cite>Samuel Johnson</cite>

So "why then do I need all these unit tests for my code?" I [read](http://jroller.com/page/pyrasun/20040408#the_power_of_obsession) with amusement. Why can't I just write functional/acceptance/integration tests and be done with it? The arguments are ludicrous. Saying that there are people we know who obsess about unit testing yet seem to write crap software and/or go overboard with seemingly useless tests in no way proves that it's a flawed concept. Holy crap! if we applied that kind of logic to the entire industry we'd have stopped doing software development years ago.

We have to be very careful what we think we're arguing about. I'd probably get upset if a Windoze weenie told me that Java was slow when what they really mean is that Java seems slow on Windoze! It may well be the case that Java is slow. But the fact that it's slow on Windoze hardly justifies saying that Java as a whole is slow.

The fact remains that what most people do IS integration and functional testing and therefore most people know very well how to do this. Just because most people don't know how to do proper unit-testing doesn't mean unit-testing is wrong. It just means we work in a very immature field. Hell most people (myself included) don't know how to do good software design. But we're getting better at it!

Unit testing has proved itself time and time again in other fields. Do you think Ferrari needs an alternator to prove its engine design? Does the alternator manufacturer need an engine to prove its design? No. Likewise, every component (unit) of your computer has been individually tested before it's placed into the PC your using to read this. Every single one. And then some! The components that make up the hard disk were individually tested before being used to assemble the hard disk. But does the PC manufacturer care about the components inside the hard disk? No. Of course not. Why? Because to them the hard disk is the smallest unit.

And so it is for software. Each "layer" in a system can be thought of as both a higher level of abstraction than the layer below it, yet a lower level of abstraction than the layer above. Sounds obvious right? Because software is and should be fractal in nature, it looks essentially the same no matter the magnification or level of granularity.

Do you test whether the JVM can actually increment a variable using the pre-increment operation? No! Do you test that the `String` class behaves correctly? Do you test to see if "your favourite framework" does its job? Probably not. At best you can safely believe it works as advertised. At worst, you write some integration tests to prove (or not) that your understanding of the tool matches reality.

Go have a quick search on [Google](http://www.google.com) and see what types of testing go on in electronic, automative, aerospace and just about every other field of real engineering I can think of. Not this trumped up Research and Development we like to call Software _Engineering_. Here's a completely [random start](http://www.ball.com/aerospace/wrdms5.html).

So when it comes down to it, the same people that tell me that "the network will never be transparent" because if I'm building a nuclear power plant, I can't afford to just "ignore" network failures, also want me to believe that I should overlook a development and testing regimen that has proven itself time and time again in other, more rigorous, disciplines!

The project I'm on has 1200+ unit-tests that take around 11 seconds to run. Then we have a smaller yet substantial number of end-to-end regression tests, an even smaller set of end-to-end acceptance tests for each iteration and a tiny number of integration tests with external systems. So maybe we're not your beloved "Thought Leaders" but we definitely have plenty of experience doing other than unit-testing.

We have zero bugs that we know of. We have things the users don't like because they changed there mind or the developer didn't understand the requirement. But we have rarely, if ever, encounter unexpected behaviour in the system. When new code is added, it quite clearly breaks the build if the developer has made a mistake. I can't tell you how many times it's saved my ass. We've definitely not encountered the types of problems usually associated with complex systems whereby strange interactions between objects causes software failure. I put this down to well defined and tested interfaces, whether they be "artifical" or not.

We perform around 20 or so automated integration builds a day running the full suite of tests and each developer runs the unit tests countless times each day.

The best part is, our system is nicely de-coupled. We can quite happily replace bits of our system with little or no impact because the unit-tests defined our interfaces.

Oh and as for writing bazillions of lines of code, actually my experience is that we end up with less code. In fact I find myself deleting more code everyday as we refine our abstractions, usually driven by testability. But that's just my experience.

I know the software I produce today is, at best, average but the average has gone up since I started working - at least in the product development (as oppsoed to consulting) circles I keep. And it will continue to go up as we mature as an industry.
