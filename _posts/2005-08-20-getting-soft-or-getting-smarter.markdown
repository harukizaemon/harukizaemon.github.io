---
layout: post
title: "Getting Soft or Getting Smarter?"
alias: /2005/08/getting-soft-or-getting-smarter.html
categories:
---
Sixteen or so years have passed since I started my first job -- a programming gig writing language interpreters and compilers -- even though I didn't really know how to program. On day one I was given an intel 80386 machine code instruction manual, a [System/370](http://en.wikipedia.org/wiki/System/370) machine code instruction manual, a desktop PC, a copy of Microsoft assmbler, access to a mainframe via a 3270 terminal emulator and told to pretty-much work it out myself. And that's pretty-much what I did.

Sixteen years later, I'm doing some work for the same company. And while a lot of the industry has move on (or was that gone 'round in circles?) they're still developing the same software all written in assembler and their proprietary development languages. They do great business selling software for 7-digit figures whilst competing with the likes of IBM, CA, etc. The proprietary language they use -- all written in assembler remember -- runs on Windows, Linux, [OS/2](http://en.wikipedia.org/wiki/Os2) and zOS and has many of the features that "modern" languages have: dynamic dispatch, loose typing, etc. Not bad for a company with three people, doing it all The Wrong Way&trade;.

My job has been two-fold: write some DHTML and JavaScript communicating back and forth with a web server written in the proprietary language; and add some new features to said language. And, all-in-all it's been pretty good fun. The DHTML and JavaScript stuff is easy peasy (like we need an [acronym](http://en.wikipedia.org/wiki/AJAX) for this stuff, sheesh!) and getting back into assembler programming after all these years of Java has been downright good geeky fun. That is of course until something goes wrong.

JavaScript debugging is still a bit lame, even with the [Mozilla plugin](http://www.mozilla.org/projects/venkman/) but that's easy enough to fix with a few carefully placed calls to `alert()`. The biggest issue with JavaScript unfortunately is the difference in browser behaviour, especially with respect to keyboard events and `XMLHttpRequest`. No matter though, we overcame those issues pretty easily and moved on to other things: adding new features to the proprietary language.

It's probably been 10+ years since I did any assembler programming and I feel it; I've become soft. I make my changes and run the application. It touches a bit of memory it shouldn't and BOOM, memory violation. Right. Register dump. Ick! I remember those. Urgh. Start up the debugger -- [gdb](http://www.gnu.org/software/gdb/gdb.html) -- and let's try that again. BOOM. This time though we're inside gdb so I can start poking around. Right. Where are we? Hmm...let's look at where `eip` points -- linux on pentium hardware. Ok, where is that relative to the load-point for the module. Ok, &lt;snip&gt;

My forensic skills have become soft. I've become too accustomed to exception handling, stack traces, automatic buffer overrun detection, garbage collection, no pointer arithmetic; unlimited numbers of variables, no [little- vs. big-endian](http://en.wikipedia.org/wiki/Endianness) issues, etc. No peeking into memory to see what the processor stack might look like anymore. Instead, just look at the line numbers and there's most of what you need to know already in front you.

I'm not complaining mind you-- like not needing to think so hard about debugging -- but it is interesting to see how my skills have changed over the years and how my current development ideas (and ideology?) have been shaped (for better or for worse) by having a knowledge of the underlying execution architecture. Forget 80x86 or System/370 processors, these days Java, .Net (and no doubt countless others) are built on _[virtual machines_](http://en.wikipedia.org/wiki/Virtual_machine) with their own instruction sets, stacks, etc. How many developers _actually_ understand the workings of the underlying VM? How much does a developer gain (or lose) by having this understanding?

**Update (23rd August 2005)**

Just to show you _how_ soft I'm getting, I changed the design to one that didn't involve me needing to use gdb LOL.
