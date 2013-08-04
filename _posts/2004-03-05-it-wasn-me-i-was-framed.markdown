---
layout: post
title: "It wasn't me, I was framed!"
alias: /2004/03/it-wasn-me-i-was-framed.html
categories:
---
People who know me well, know well (and it's not even christmas) my disdain for frameworks in general. I must have missed it but at some point the ideal of component based development was hijaked by _the framework_. Was this because it didn't actually work or because someone decided to play a cruel joke on us to see how long we, as an industry, could keep going around in circles re-inventing the proverbial wheel?

Almost every large-scale project I've ever seen starts of with lofty goals of re-using the framework dejour and soon realises that it just wont cut it. Then comes the process of deciding if it is better to stick with it and forego the application you really want/need, roll-your own for maximum flexibilty or bastardise it so as to render your application largely incompatible with any future releases of said framework.

[Sourceforge](http://www.sf.net) is littered with every-man-and-his-dogs attempt to build a better mousetrap. And they're just the open sourced ones. I dread to think how many companies have their own flavours!

I've had much success building applications from scratch with no framework, just some user stories and test cases. Then letting the infrastructure requirements emerge and hunting around for libraries that will satisfy my needs. This seems to work much better than the times I've tried to decide up front which frameworks and tools I'd like to use, invariably leading to more work than it was worth. The key point here is identifying real requirements and addressing them. _"We will need struts"_ is not a requirement!

Off the top of my head, I can think of maybe half-a-dozen libraries that I've been using regularly that don't lock me into someone elses idea of how my application should be structured. Some for persistence, some for rendering, others for business rules, etc.

Sometimes there are so-called "strategic" reasons for using a particular tool. Unfortunately, the average whiteboard architect has usually been drinking way too much Gartner Koolaide.  To be re-usable, software components should solve a particular problem without trying to dictate my overall design. And more importantly, they should solve the problem well.

Yikes! Is that [bile](http://www.jroller.com/page/fate) I can smell? Hmmmm
