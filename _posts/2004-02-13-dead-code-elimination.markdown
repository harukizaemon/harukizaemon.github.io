---
layout: post
title: "Dead code elimination"
alias: /2004/02/dead-code-elimination.html
categories:
---
I recently added <a href="http://www.joverage.org"'>jCoverage</a> to the build of a "mostly" TDD project. The bits that weren't TDD are classes that we either "hacked" together for a quick-and-dirty proto-type (and that will ultimately be removed) and some that we used for domain modelling.

I like test coverage results, mainly because it gives me a warm fuzzy feeling. I can see how well or how poorly the developers are going WRT test coverage. But, I've often wondered if it has much benefit over and above the warm fuzzies.

It has occured to me many times that in many cases (especially on a TDD project) the coverage analysis really shows me potential areas of dead code. That is, code that almost never (if at all) gets executed. Especially on large projects with lots of re-factoring going on, it can be hard to keep track of which methods are no longer used.

IDEs such as [IntelliJ IDEA](http://www.intellij.org) and [Eclipse](http://www.eclipse.org) and even [Checkstyle](http://checkstyle.sf.net) and I think [PMD](http://pmd.sf.net) can show you visually which private members aren't being used. The IDEs can also show you which public and protected methods aren't being used but you have to run the search manually. Granted, it is also possible to write some code that loads all your classes and builds dependency graphs to do the same. But why bother when I already have a tool that does it for me?

And so it came to pass that this morning I was looking over a coverage report and found a few areas where the coverage was a big fat zero. Intrigued, I opened the code in my IDE and did a search for all usages. What do you know. None. Zero. Nada. Bupkis. Zilch. You get the idea :-). I did this on the next few untested methods and I'd say that around 25% of the untested code was actually dead code!

Just for curiosity, I'd really like to put this into a live system and see which parts of the code aren't used. I'm sure some of it will turn out to be old code that handles scenarios that never eventuate anymore. Who knows? Worth a try at least?
