---
layout: post
title: "Code Normalisation"
alias: /2004/03/code-normalisation.html
categories:
---
I've been doing some incidental code cleaning as I implement functionaility for the system I'm currently working on. It's something I really enjoy. Code quality is somewhat of an obsession. I often wonder if it's actually worth it. But I dislike leaving "ugly" code lying around so I fix it anyway. I'm sure some of the developers wished I'd just shut up. Some of them have even taken to not letting me actually look at the code when answering a problem for fear that I'll get distracted :-)

Last year, [James Ross](http://www.redhillconsulting.com.au/blogs/james) and I wrote some 20+ custom [checkstyle](http://checkstyle.sf.net) checks (most have made it into the core distribution). They were brought about by a project we were both working on at the time. We would manually identify code smells and then set about developing some heuristics for automagically finding all of them.

I've been applying most of those same checks to this project and, I'm happy to say, although they turned up some "violations", for the most part the code base is pretty clean. I still come up with more checks I'd like to implement when I decide to make the time and last night I made the time and wrote a very simple check for ["missing abstractions"](/blog/2004/02/04/lots-of-little-classes) or more properly, opportunities to normalise.

There are clearly many ways we determine that there are missing abstractions but there is one thing I've noticed on many projects: code containing common prefixes and/or suffixes is often a good candidate for extracting a class. Some very simple yet recurring examples might be date ranges (`dateFrom` and `dateTo`) and addresses (`addressStreet`, `addressCity`, `addressPost``), etc. I'm sure you can think of other less obvious examples.

The check simply identifies clusters of instance variables or parameters that share a common prefix and/or suffix. When I ran it against the code base this morning I was amazed to find hundreds of examples! Probably 1/3rd were in constants: `MINIMUM_AGE`, `MAXIMUM_AGE`, etc. with the remainder largely instance variables and a small number of parameters.

After trawling through the results, I'd say I had probably found a dozen or more absolute, no-bones-about-it cases for refactoring which I thought was a pretty good result. What concerned me was there seemed to be a lot of noise. But then again, was it really noise? Would it not be better to have minimum and maximum values actually represented as a `Range`? If so, how far do you perform this kind of normalisation? As James pointed out, humans really do have a left and a right arm. Do we really need to make an `Arms` class with `getLeft()` and `getRight()`? Probably not. It could be argued though that constants sharing the same prefix/suffix such as `_ACTION_NAME` or `DISPATCH_` are really just introducing an implicit namespace that might be better moved to a separate, explicit, enumerated type?

I'd be interested to know what you think and if you'd be interested in running the check against your own code to generate some feedback. To do so, [download the jar file](/blog/archives/jamaica-tools.jar) and add it to your build class path. Then, to run the check, simply add the following code fragment to your checkstyle configuration as a child of `TreeWalker`:

``` xml
<module name="au.com.redhillconsulting.jamaica.tools.checkstyle.MissingAbstractionCheck"/>
```

You'll also find some more checks (all in the same package) that have yet to make it into the checkstyle distribution:

* ClassDataAbstractionCouplingCheck
* ClassFanOutComplexityCheck
* DuplicateLiteralCheck
* FinalFieldCheck
* GuardingIfCheck
* NPathComplexityCheck
* NullPointerCheck
* ReturnFromCatchCheck
* ReturnFromFinallyCheck
