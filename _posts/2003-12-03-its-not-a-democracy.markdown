---
layout: post
title: "It's not a democracy"
alias: /2003/12/it-not-democracy.html
categories:
---
_"Smart people don't always make smart decisions." -- anonymous_

Ahh yes. The code review...

Why do some developers feel that adhereing to (largely) well accepted design practices and coding standards (anyones just have them!) somehow takes away their freedom of expression when they're quite happy to unwittingly limit themselves by way of poor design choices and down right rotten coding practices?

Do they feel they work in a commune or democracy? It's not. Shared ownership I have no problem with. But shared responsibility? Yeah right.

Set your standards, enforce them and be done with it I say. You wanna debate about it, sure, now lets stay back and re-factor the code so that it now adheres to the new standards! I've done it before. It's a lot of work. But you gotta admit the code was easy to extend and maintain.

Use [Checkstyle](http://checkstyle.sf.net), [PMD](http://pmd.sf.net), or similar tool that is extensible enough to add your own. These tools dont just check line lenghts anymore. They can be used for enforcing design patterns, looking for poor ot at least suspect coding practices. Almost anything really.

And make sure you have a code cop with Balls!
