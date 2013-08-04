---
layout: post
title: "Trading Design Pain for Runtime Pain"
alias: /2010/06/trading-design-pain-for-runtime-pain.html
categories:
---
So[^1], since my post on [functional programming in object-oriented languages]({% post_url 2010-03-01-functional-programming-in-object-oriented-languages %}) I've continued to tread the path with a mixture of gratification and despair. This morning the latter became overwhelming, I'd just had enough. My brain hurt and I just wanted to pump out some code, run it. I threw out the concepts I had been using as my guide and fell back on years of "old-school" object-oriented code.

Unfortunately, I was no more productive. In fact, I'd argue I was less productive. Things began to fail in weird and unexpected ways. The number of tests I needed to write to catch errors at least doubled. I soon returned to the comfort of my hybrid world.

<!--more-->
In hindsight, I had traded design pain for runtime pain. All the mental gymnastics that went into working out how to build classes that are inter-related and at the same time immutable, etc. was replaced with time spent writing tests for anticipated edge cases as well as debugging the unexpected ones.

I concluded that the "pain" I had been experiencing was largely the result of being forced to deal with the complexity of the underlying problem. Once solved however, the code fell out with few or no bugs. By contrast, when I reverted to my previous approach, the code flowed far more freely but I spent a lot more time working out how to ensure the code didn't do nasty things to itself.

What's perhaps as interesting to me is that my designs are resulting in smaller and smaller classes. The more I think about problems in a functional way, the more I'm am able to design solutions that are essentially pipelines. The irony being that even though we think of imperative code as being step-by-step, it more often than not turns out to be a big, intertwined blob. Functional code on the other hand is almost by definition a series of steps, or transformations applied one after the other on some input.

These two observations are drawing me ever closer to just "getting over it" and using a functional language. The issue for me is the only FP language I know and actually like is Haskell and the only FP language I'd be likely to get into production is Clojure. Which is all I'll say in public as I have no desire to start a flame war :)

[^1]: [A Connective Word Takes the Lead](http://www.nytimes.com/2010/05/22/us/22iht-currents.html)
