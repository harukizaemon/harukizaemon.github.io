---
layout: post
title: "Domain Driven Design"
alias: /2003/12/domain-driven-design.html
categories:
---
I spent many years writing software that was essentially procedural in nature with [transaction scripts](http://www.martinfowler.com/eaaCatalog/transactionScript.html) accessing a database through very simple [active records](http://www.martinfowler.com/eaaCatalog/activeRecord.html). Since I've come to appreciate the benefits of a [rich domain model](http://martinfowler.com/eaaCatalog/domainModel.html) I've wished I had the ability to communicate this to others.

So I was excited when I heard about the book [Domain Driven Design](http://www.amazon.com/exec/obidos/tg/detail/-/0321125215/qid=1071396235/sr=1-1/ref=sr_1_1/104-9671265-6859168?v=glance&s=books).

It's nicely written. A good mix of code, diagrams, anocdotes and I particularly like the transcripts of conversations from actual modelling and design sessions. Lots of examples, rules of thumb and hints and tips here and there. It reminds me of [Pragmatic Programmer](http://www.pragmaticprogrammer.com/ppbook/index.shtml) in the way it reads.

Points I've particularly liked:

* Model-driven design he says _"discards the dichotomy of analysis model and design to search out a single model that serves both purposes."_ Something I've always felt since reading [Analysis Patterns](/blog/2003/12/09/analysis-patterns). As the author points out, this is very hard to do but well worth the effort.
* The good old manufacturing metaphor where highly skilled engineers design and less skilled laborers assemble the products has _"messed up a lot of projects for one simple reason -- software development is all design."_ All right! Rock on! I'm printing that out in 70pt font and sticking it up on the wall of my favourite enterprise architects office!
* _"If an unsophisticated team with a simple project decides to try a MODEL-DRIVEN DESIGN with LAYERED ARCHITECTURE, it will face a difficult learning curve."_ I've recently pondered this as I've struggled with bringing teams of developers up to speed with these concepts. I've wondered whether I may be making matters worse before they, hopefully, get better.

Things I've disliked:

* As Doug points out in [his review]( http://www.creativekarma.com/comments/40_0_1_10_C/), I too was a bit surprised by the "Manager" classes. Seems to defeat the whole purpose of rich domain models but as I haven't finished yet and, the author promises to address this later in the book (fingers crossed) I've chosen to ignore it so far.
* The book is rather verbose I'm sad to say. I got into the first section and started to stall! It was very hard to get motivated to read the rest of the book.

All in all not a bade book. Someone definitely needed to write about it but as I mentioned, it was a little verbose for my liking. But I did finish it and I'd have to say that it was probably worth it.
