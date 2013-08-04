---
layout: post
title: "Analysis patterns"
alias: /2003/12/analysis-patterns.html
categories:
---
When I first moved over to the cool new world of OO, I just loved the fact that, especially in C++, I could use inheritence to do so many things. I could just create yet another sublcass, override some method/s and bingo problem solved. What I didn't realise was that I was creating an enormouse mess of brittle, crufty, hard to maintain code. Personally, C++ multiple inheritence didn't help! But let's not go there just now shall we :)

So, for a simple problem with some very basic requirements:

* We have bank accounts
* We can transfer money from (debit) at most one account
* We have 2 types of transfers
* Single funds transfers allow crediting to one account
* Multi-funds transfers allow, you guessed it, multiple credit accounts

Now, how would you have modelled this? There are of course no right or wrong answers but I'm curious to hear your thoughts.

You see, the most common solution I come across has two sub-classes, one for each type of transfer. One allowing a single credit and the other allowing a collection of credits.

As you've probably guessed however, this isn't my preferred solution. I'd rather see a single class for a transfer with a collection of credits and some kind of constraint class that holds "rules" regarding minimum and maximum numbers of credit accounts.

I'd say the most influential books I've ever read to do with software developement, in the order I read them, were:

* [The Art of Computer Programming](http://www.amazon.com/exec/obidos/ASIN/0201485419/qid=1070969495/sr=2-1/ref=sr_2_1/104-9671265-6859168);
* [Design Patterns](http://www.amazon.com/exec/obidos/tg/detail/-/0201633612/104-9671265-6859168?v=glance); and;
* [Analysis Patterns](http://www.amazon.com/exec/obidos/tg/detail/-/0201895420/104-9671265-6859168?v=glance)

The last is probably not as well known as the first two but, quite aside from the patterns themselves, it fundamentally changed the way I model problems. For one, I stopped creating ridiculously deep class heirarchies to solve every problem I came across.

It's been many years now since I read [Analysis Patterns](http://www.amazon.com/exec/obidos/tg/detail/-/0201895420/104-9671265-6859168?v=glance) but I've had cause to refer back to it recently in group modelling sessions. If you've not read it I highly recommend that you do.
