---
layout: post
title: "Drools Schmokes!"
alias: /2004/10/drools-schmokes.html
categories:
---
We're about to open source a new [rule-based project](http://www.sourceforge.net/projects/joodi) and up until now, we'd been using various closed source rule engines to get us going. Of course this won't cut-it once we open source so we hoped that [Drools](http://drools.`haus.org) would come to our rescue.

And it did. With some caveats, I can safely say that Drools is incredibly fast. Not bad for a code base that by their own admission has, quite rightly, favoured stability over performance and as such has had ittle or no profiling done.

Luckily we had built joodi, short for Java-Based O-O Design Inferometer (just had to get the word Inferometer into a project somehow!), test-first and as such the guts of the app was based on interfaces so cutting over to Drools was prety easy. It took me about an hour I guess to convert the application, rules, tests and all, to run with Drools. We fired it up. All tests passed. Hooray!How happy were we!?

Next to run a "benchmark". We ran the application over the struts classes using the closed source engine first and it finished in around 9 seconds. COOL! Performance had been one of our unknowns and this was certainly well within tolerences.

Then we switched over to Drools and run the same test. 20 minutes later it still hadn't finished. Another ten minutes I'd say and I was fast asleep. So when morning came around I lept up and ran into the lounge to see if it had finished. It had. In 78 minutes!!!

Yikes we thought. This aint going to cut it. Elation turned to dismay. But no real profiling of Drools had been done so surely there was room for improvement?

After a bit of chatting with the peeps in [da haus](http://www.`haus.org), I decided to check-out the source and use [JMP](http://www.khelekore.org/jmp/) to do some profiling. Run it, we thought, find the lowest hanging fruit, fix it, then keep doing that until we've done all the obvious stuff.

So I cranked it up and it didn't take long to find a hot-spot. In fact it appeared that nearly 50% of the time was being spent in one small area - conflict resolution. A quick look at the source code was all that was needed to confirm my suspicions. Lots of unecessary iteration. But again, I'm not taking anyone to task over it. I'd rather it was stable and functional first.

Looking more closely at the code, I realised that the functionality provided by the classes under scrutiny were not actually necessary, yet, for me to get joodi running. Thankfully due to the thoughtful design it was pretty easy to stub out, without even touching the Drools source-code.

Time to run again...holy-cow! 5 seconds! That can't be right. Run it again. Nope 5 seconds again. Quick look at the output to verify it was actually working correctly. Yup. Run all the joodi unit tests just to be sure. Yup they run just fine. It had gone from being 300 times slower to almost twice as fast!

Damn I'll try running joodi against another, bigger, project - xerces. With Drools plugged in, joodi ran in around 9 seconds. With the closed source product I gave up after 5 minutes and stopped it.

So hats off to the Drools team. Damn fine job! I'll be submitting my patches ASAP and hope to see some of that other code re-factored soon :-)
