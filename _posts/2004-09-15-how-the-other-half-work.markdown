---
layout: post
title: "How The Other Half Work"
alias: /2004/09/how-the-other-half-work.html
categories:
---
It's amazing how after just 3 days on a motorbike, out on the open road, breathing in clean ocean air, all the worries of the world seem to dissapear; how the old "care-factor" rapdily falls away to hover tantelisingly above zero; only to be brutally re-awakened by my brain hitting the ground with a thud as I begin day one of a new, 3-month long, project. And what a project it is!

The [last project]({% post_url 2004-09-07-sunshine-and-party-pies %}) I was on kept my brain working overtime. I had to push myself to stay afloat, to keep up with the brains on the team. It was fantastic. We all pushed each other, striving to get as close to the "ideal" solution, continually honing and refining the design to remove all, seemingly, unneccessary fluff. The Simplest Thing That Could Possibly Work While Still Being Good Design &trade; was King.

But now, as if by way of some bizzare super-natural ying-yang thing struggling to balance the forces of nature, I find myself on a project where the Powers That Be have skillfully managed to find complexity where there rightfully should be none.

And yet I'm baffled that, at some level, it's a very simplistic design. A design that even with it's strange technology choices and inexplicable coding "standards", requires very little brain power to comprehend. I suppose that by adhering to all those wonderful&trade; J2EE Core Patterns, it can't help but be simplistic. It really is the epitomy of a Simple Arrangement of Complex Things. Need a service? JNDI is your friend. And for your troubles you'll get back an EJB. Need to extract a piece of data from some XML? Here's a static helper class we prepared earlier.

The other developers on the team (I've come in 3/4 of the way through) have no trouble wading through the hundred+ line methods that make up the half dozen or so EJBs mixed in amongst the various classes that together comprise the 20 or so WSAD "modules". In fact I found myself in a meeting honestly feeling stupid that I just didn't "get" the seemingly overly complex application architecture. I figured I must be missing something, something significant. And maybe I have.

With the exception of my good buddy [Phil](http://www.mikemelia.com), it made perfect sense to all the rest to store XML as a CLOB in a relational database only to unpack it, extract a subset of the fields and use [Lucene](http://jakarta.apache.org/lucene/) to index the records. No one else seemed to think that spawning threads inside the app server was a bad thing. I mean "we don't want to re-invent MQ". Of course not. Silly me. Are you sure 15 days isn't too fine-grained an estimate for you? We still have 6 weeks left, surely your gant chart would look a lot simpler if we just made everything finish then. :-P

I'm not sure if I simply suffer from a particularly severe form of geek snobbery (in the same way that Linux users look down on Windows users) or perhaps I expect too much from my work. Whatever it is, it's interesting to see another perspective on J2EE development. I will try to suspend for a while my preconceptions and at least give it a go. But I'm not giving up IntelliJ...yet!

My only consolation is that it will surely be an intellectual walk in the park and give me more than enough food for blog, not to mention oodles of free cerebral cycles to work on my various out-of-hours pet projects - the ones I had been neglecting for the past 6 or so months.
