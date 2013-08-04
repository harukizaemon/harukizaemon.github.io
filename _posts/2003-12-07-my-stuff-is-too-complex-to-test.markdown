---
layout: post
title: "My stuff is too complex to test"
alias: /2003/12/my-stuff-is-too-complex-to-test.html
categories:
---
Boulderdash! [Dealing with complexity is the essence of software engineering.](http://www.relisoft.com/book/proj/1software.html) I particularly liked the idea that software is fractal in nature as it is something I've always tried to communicate to others. The interactions between components in any layer (should) look the same as the interactions between layers. No matter what the level of abstraction. But I digress.

If you've managed to build a system that does the job but is too hard to test, then your system works by coincidence. Sure you may think you've deliberately implemented a solution in a particular way but the fact that it works is more dependent on probability than pure determinism.

If it seems too complex, first break the problem down into smaller, more manageable, chunks. Start solving the smaller problems. Then start re-factoring and abstracting based on common code, common solutions, etc. In this way, large systems become a complex arrangement of simple things.

Software that is designed well is testable. The corollary to this is that software that is not testable is designed poorly. A focus on testability results in software that is well designed.

Can you guess what the next three letter acronym is likely to be?

If you guessed TDD, you guessed right!
