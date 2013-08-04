---
layout: post
title: "TDD and Genetic Programming"
alias: /2004/02/tdd-and-genetic-programming.html
categories:
---
TADAIMA!

It's been sometime since my last entry (feels like the start of a confession) but I've been in Japan for the past 2 weeks. For various reasons I really needed to get away from computers for a bit (no pun intended) and what better way than to emerse myself in my [other passion](http://www.iwamaaikido.com.au).

As I was sitting in the plane on the 9 hour leg from Sydney to Tokyo, I was thinking about genetic programming (GP) and good old fashioned artificial intelligence (AI). I remember being fascinated by AI when I was a kid only to discover that in essence it was all about fitting a curve to a line. In more recent years, I stumbled on GP which sparked my interest once again especially when I read about people taking out patents on the basis of genetic algorithms (GA) they had developed specifically for the purpose of producing patentable designs.

So anyway, I started to think back to simple (and not so simple) neural nets that I had worked with. The kind where you create a back propogating net, feed it the expected inputs and outputs and watch it reconfigure the weights, etc. accordingly.

This got me to thinking about my old friend test driven development (TDD). Neural nets "evolve" by reconfiguring themselves to satisfy the old requirements AND the new requirements. TDD encourages a similar behaviour.

One of the criticisms of neural nets is they can be quite brittle. That is, they may very well solve the problems they have trained for but can often fail dismally on problems that to us seem almost exactly the same yet obviously differ enough to "confuse" the net. Usually it's just a matter of re-training the 'net with all the old data AND the new data (just like running your tests!). The trick is to understand the problem sufficiently to create useful training data.

One of the criticisims of TDD has been that it creates brittle applications. Now my personal experience is that is just rubbish. If anything, it encourages designs that are more flexible/extensible in the long run.  But, it is true to say that a purely TDD application may not cope with a scenario that hasn't been tested for. In fact I argue that anything that hasn't been tested for is by definition not a feature of the system. Not only is it not a feature of the system now, it is definitely not guranteed to work in any future release even if it does work by coincidence now.

But the real thing I liked about comparing TDD with AI is that it's evolutionary. The system is continually adapted to suit the changing needs. This naturally lead me to think about GP and wondered if it was possible or even practical (let alone useful) to write a GA that could take a suite of failing JUnit tests and generate an application that would make the tests pass? Then we would simply add some more tests and re-run the GA to produce a new system adapted to the new requirements.

As I've already stated, I'm not sure how practical this is let alone useful even if it is possible. But it was the last thought on computers I had until I arrived back in Melbourne.

Now to find a good Japanese restaurant...
