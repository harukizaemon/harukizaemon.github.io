---
layout: post
title: "The Lost Art of Database Design"
alias: /2004/08/the-lost-art-of-database-design.html
categories:
---
Have you ever read any [C. J. Date](http://www.amazon.com/exec/obidos/ASIN/0321197844/qid=1091840202/sr=ka-1/ref=pd_ka_1/104-0267997-9716779)? No? Hardly suprising. In fact most of my colleages couldn't give two hoots when it comes to data modelling (aka ER modelling) as distinct from object modelling. I'll admit that for many it's not the sexiest of topics.

Some months ago, [Dave](http://www.redhillconsulting.com.au/blogs/david) and I were discussing object modelling and database design. Daves assertion was that you need to do a good data model before your object model. I countered that I believed a good object model necessarily produced a well formed and normalised data model. On reflection it seems we were both right.

You see I was under the illusion that a good understanding of relational theory and alegbra was ubiquitous. I naively believed that all software developers knew what a normal form was, what a primary key actually meant and why relational integrity enforced via foreign key constraints IS important! WRONG!

Relational database management systems (RDBMS) are based on mathematically provable theory. SQL for example is a bastardisation (though not Dates exact words) of the [relational algebra](http://www.fact-index.com/r/re/relational_algebra.html) which in turn is derived from [predicate calculus](http://www.math.psu.edu/simpson/papers/philmath/node6.html). Interestingly RDBMS and Business Rule Engines have much in common so it's not suprising that Date, an amazing logician,  seems to spend a bit of his time these days [writing](http://www.brcommunity.com/index.php) on business rules. But I digress.

Given this basis in rigourous theory and mathematics, I find it amazing that as an industry of self-proclaimed _engineers_ so few of us seem to have any clues on the matter, preferring instead to stay in the relative adhocery of object modelling where we can all become pioneers by inventing and re-inventing this months _rules of thumb_ or _best practices_ for good OO design.

Many of the available ORM tools force you to either screw your object model or screw your database model and being the anal retentive person that I am when it comes to both topics I find this rather discouraging. Mostly because I'd be willing to bet my left nut that most developers out there don't even realise that they've ended up with either an object model that violates some pretty basic principles of good OO design or ended up with a data model that is just plain wrong.

Granted, DBA's are _supposed_ to look at our data model and find the flaws but so far as I can tell, most DBAs these days are there to enforce `BLLSHT NMG STNDRDS THT N N CN NDRSTND` or to tell us what columns need indexing.

I'm all for [surrogate keys](http://www.bcarter.com/intsurr1.htm) over business keys, etc.  but the fact remains that until the world chooses object oriented database management systems (OODBMS) in favour of the good old RDBMS don't delude yourself into believing that relational theory isn't important.
