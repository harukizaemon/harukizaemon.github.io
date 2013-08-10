---
layout: post
title: "Databases: A Necessary Evil?"
alias: /2006/01/databases-a-necessary-evil.html
categories:
---
It's an attitude that I see all too often in developers, especially those that label themselves Object-Oriented Purists who are supposedly capable of precise, rational thought, above and beyond that which we might expect from your mere mortal developer.

Ever since my [little rant on the subject]({% post_url 2004-08-07-the-lost-art-of-database-design %}) over a year ago now I have been tempted time and time again to write something more but each time I go to put finger to keyboard I'm overwhelmed with a sense of resignation to the fact that when it comes to relational databases, most people just don't get it. If I only had a dollar for every time a developer has adamantly told me that foreign keys are a waste of time and that somehow primary keys are a neccessary evil to satisfy the requirements imposed by the database so that we can look up a record. "Why would I need all that nonsense?" they chortle rhetorically, "my application handles all that stuff just fine." Sure it does, so prove it!

What I find even more frustrating is the FUD spread by those I expect to know better. Case in point. I was doing some reading of old Ruby On Rails -- yes, my shiny, not so new, toy -- newsgroup postings when I came upon [this](http://wrath.rubyonrails.org/pipermail/rails/2004-November/000451.html) from none other than Mr. Rails himself:

> And let it be no secret that I consider the database a necessary evil for the object-oriented domain model. Not the other way around. Hence, when given the choice between implementations that pit OO model against database purity, I will almost always side with the OO model. -- David Heinemeier Hansson

Now the statement just quoted was apparently a justification for Single Table Ineritence and the absence of Mutliple Table Inheritence in rails. Fine. I have no issue with this at all. In fact one of the things I like about Rails is the fact that it is unashamedly "Opinionated Software": stuff works the way it does in Rails because, well, because that's the way someone decided it should. So why shy away from this with some spurious argument about what is/isn't good database design? Why not just admit that it's a personal preference and indeed that's the real reason it's like that? Would you consider Active Record a necessary evil or simply a design choice? I guess it depends who made the choice ;-)

So anyway, let's clear a few things up. Firstly, I'm fairly sure that when he (DHH) says _database_ what he actually means to say is _**relational** database_ -- surely he doesn't mean Object-Oriented Database? Secondly, even though he means to say relational database, what he actually should be saying is _**SQL** database_ -- believe it or not there's a BIG difference!

Admittedly C. J. Date is not the most exciting read for many people but if you take the time to read some of his work on Relational Database theory, you might be compelled to re-consider your notion of the so-called "impedence mismatch" between Object-Oriented Design and Relational Databases:

> ... As far as I'm concerned, an object/relational system done right would simply be a relational system done right, nothing more and nothing less. -- [C. J. Date](http://www.oreillynet.com/pub/a/network/2005/07/29/cjdate.html)

I found it fascinating when I spent some time at a university many years ago that database theory was the least popular subject. Maybe it's just me? Maybe I really enjoy the subject because I see elegance and simplicity and yet enormous power in Relational Databases and it Just Made Sense&trade;.

Relatonal Databases surely aren't as sexy as Ruby or Rails but neither are they glorified record management systems. They are very precise and provably correct repositories for our data. So, do what you will with databases, treat them as evil if you wish. but I think it behooves us to try and understand what we actually think we've supposedly moved beyond.

**Update 18 January 2006**

[Primary Keyvil, Part I](http://blogs.ittoolbox.com/database/soup/archives/007327.asp)
