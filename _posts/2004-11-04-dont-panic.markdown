---
layout: post
title: "Don't Panic!"
alias: /2004/11/don-panic.html
categories:
---
Apparently, "after hours" batch jobs don't require load testing. Yes, you heard it. Supposedly jobs that run when no users are logged in are pretty much free to do whatever they like, all 57 of them! Is it some weird side effect of [Heisenberg's Uncertainty Principle](http://www.aip.org/history/heisenberg/p08.htm) that I've never heard of whereby it's possible to be either using the system interactively; or have limitless computing power; but not both at the same time? Who knows but excuse me for suggesting otherwise.

You'll also be relieved to learn that there is no need to load test your applications together on the same box even if they will be co-located in production because we can extrapolate from the results obtained by running a single application stand-alone. That'll be a good cost saving I'm sure.

Oh and as for including the generation and downloading of PDF documents, bah! That does nothing more than test all that pesky "network bandwidth stuff". There's nothing we can do about that anyway so why bother testing it right?

Phew! That's a load off my mind (no pun intended). I had thought that we might end up doubling the load on the production box but it seems I was somewhat misguided. Glad they've got it all sorted out. At less than 6 weeks 'till go live and with the application only just now limping into System Test, I was beginning to worry. Silly me. What was I thinking?

Now where did I put my double pair of Joo Janta 200 Super-Chromatic Peril Sensitive [Sunglasses](http://hhgproject.org/entries/perilsensitivesunglasses.html)? I'm sure they're around here somewhere...
