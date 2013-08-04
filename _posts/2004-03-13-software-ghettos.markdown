---
layout: post
title: "Software Ghettos"
alias: /2004/03/software-ghettos.html
categories:
---
I was sticking my nose into a code review [James Ross](http://www.redhillconsulting.com.au/blogs/james) was performing the other day. Among other things, naturally, he was checking for code duplication. Now my informal analysis of open source projects and commercial projects I have been brought onto suggests that, a figure of anywhere between 10% and 20% code duplication is pretty standard. As horrifying as that may seem (to me at least), it was rather amusing to think that the final report might include words to the effect that, _"your software falls within industry norms with respect to duplication of source code."_

This lead me to question what is acceptable. Why is it that one developers clean code is anothers quagmire? James suggested that over and above the obvious differences in experience and "smarts", it may come down to how much "smell" we can tolerate. He likened it to cleaning the house - Some people (ok me!) can leave dishes in the kitchen for days while others (like James) apparently can't go to bed if there's even one dirty dish lying around.

James also recalled an article he had read on [Broken window syndrome](http://www.sjvgreens.org/broken.shtml) and we wondered if the same thing applied to software via accumulated [technical debt](http://www.martinfowler.com/bliki/TechnicalDebt.html)? That maybe as the density of code smell increases, there is some point beyond which there is no escaping. A point beyond which your team subconciously stops caring anymore because _"well the rest of the code is crap so I'll just add this hack in here."_

Maybe that's why Windows&trade; is so very broken? ;-)

I recall my mum being infuriated that as kids, we would simply walk past the rubbish or toys on the floor as if they weren't there. She couldn't understand why it never ocurred to us to pick up the Lego and put it away. Even now I find myself noticing a piece of paper on the floor and forcing myself not to just leave it.

So while your team might start to understand the value of design patterns, TDD, whatever, it's really important to get them to be more sensitive to identifying and eliminating rubbish as they walk through the code and not just leave it there for mum to pick up.
