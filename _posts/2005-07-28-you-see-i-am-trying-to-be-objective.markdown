---
layout: post
title: "You See I Am Trying To Be Objective About It."
alias: /2005/07/you-see-i-am-trying-to-be-objective.html
categories:
---
It was an unsettling feeling, one I was sure I had experienced before.

Seven days ago today, I boarded the good-ship X` bound for the land of Cocoa. I was promised a bumpy but nevertheless enjoyable ride over well charted sea, enjoyable smalltalk and no bugs. Life would be wonderful and applications would spew forth from the finger tips of those who chose to take the journey.

Seven days later, I'm in pain. I'm hurting. I'm surrounded by square brackets. I have a hard time keeping track of my memory (maybe it's alzheimer's?). I wish I new what was wrong with me but even the good Dr. GCC seems to have a heck of a time diagnosing my condition. I have a sneaking suspicion it's a form of jet-lag - I feel as though I've travelled back in time (about 15 years to be precise) but I'm not sure.

Humour (such that it is) aside, I've been getting into Objective-C the past week as there's a project I'm about to start working on that is targetted at Mac OS X and will be written in said programming language. So, I thought I'd go head first and use X` as well. Ouch, ouch, hurty, hurty. Why do I feel like someone just cut off my arms and stuck red hot pokers in my eyes? To be honest, I think I should have started off with TextMate and GCC. But nevertheless I'm percervering.

I do like the Smalltalk like features such as dynamic binding and the calling syntax is quite simialr too. I don't care much for the use of square brackets - though I'm not sure I could come up with anything much better for a language that is (or at least was) essentially a pre-processor for a C compiler. The fact that I have to use pointers all over the place is particularly irritating too.

Ahh yes, I was once a member of the K&R club: First C; then C++. All those calls to `malloc` & `free`, `new` and `delete`. Endless hours of trying to remember what happens when I use array indexing on something declared as `int **`. In the end though, I guess it comes down to what you're used to and for the last 6 years (I think?) that's been Java; a language that for all it's short-comings (of which there are plenty), is a pretty simple language to learn and understand - at least it was for me anyway. I've become acustomed to garbage collection, packages and object references (the last being syntactic sugar mostly) and I like them. And of course then there's IntelliJ and Eclipse to help me out, suggest class names, think for me.

So it came as rather a shock to the system to go back to an IDE that seems to require soooo much configuration for _apparently_ soooo little benefit, a language that forced me to think about when to call `release` to cleanup unused objects, and an overall environment that let me compile and run something that, on subsequent inspection, didn't even seem to be valid code - more likely a "feature" of X` than any underlying problem with GCC or the language itself.

Of course many of you will argue that I've forgotten what it's like to be a Real Programmer&trade;; that it's character building. To this I say phooey. I was once an assembler programmer and I loved it. Then I became a C/C++ developer and I loved it. Then I moved to Java and never looked back (well maybe once but nobody saw me do it so you can't prove a thing). Moving to Objective-C is thus a painful yet disturbingly rewarding experience; like pulling out the old Commodore-64 and playing Galaga: The graphics suck but they're cute and the game is nevertheless fun to play.

So, the things I like about Objective-C so far:
* Dynamic dispatch - ie. run-time method binding;
* Categories - adding methods to objects at run-time;
* Smalltalk-like calling syntax; and
* `self` - I don't know why but it's somehow more appealing (or just different) than `this`;

Things I dislike about Objective-C so far:
* Manual (yes as far as I'm concerned even reference counting is manual) memory management;
* Square brackets;
* `+` and `-` for marking static and instance methods respectively;
* `@whatever` - smacks of pre-processor hackedy hack hack hack; and
* Needing to manually call the super "constructor" - it's not really a constructor so, yes, I know why it's necessary but it still sucks.

But in the end, who can resist the sexy look-and-feel of Mac OS X apps? Not I. And I want to try my hand at creating some so what better way to do so than Objective-C/Cocoa. I'm assured that once I get into using the Cocoa stuff, life get's a lot more fun and interesting, fingers crossed. Or perhaps I'm just too old and grumpy ;-)

If anyone has tips, links, experience they'd like to share on how to make this a more pleasant journey (no, shut-up and take it like a man doesn't count), please please please let me know.
