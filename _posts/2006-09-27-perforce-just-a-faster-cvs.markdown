---
layout: post
title: "Perforce: Just A Faster CVS?"
alias: ["/2006/09/perforce-just-a-faster-cvs.html", "/blog/2006/09/27/perforce-just-faster-cvs"]
categories:
---
So, it's 7am-ish and I've had 6 or so hours of sleep to ruminate on this but yup, from a developers perspective, I still think [Perforce](http://www.perforce.com/) sucks.

Can anyone tell me why they believe it seems like a good idea to:

* Require an ssh tunnel to have encyrpted communication;
* Keep a secondary workspace to enable offline revert;
* Have a command-line tool that uses environment variables -- or command-line arguments -- to specify connection details;
* Display a diff of which files changed as a tree -- I just want to see the individual files not my entire project;
* The list goes on...

I like to work offline, a lot, on planes, trains and in taxi-cabs; I like to be able to see immediately what's changed; and I like to be able to revert everything (or only somethings) several times while I'm prototyping.

With subversion I get a lot out-of-the-box and while there will always be nice to have features such as "add all unknown files" it does pretty much everything I need.

As I moved from C to C++ to Java and then to Ruby, I felt empowered each step of the way. I had a similar experience moving from CVS to SVN. Perforce seems like a step backwards.

Google may use and recommend Perforce but when the answer to "why can't I do ..." is "you can, just write a script to ..." I'm not sure I'm convinced.
