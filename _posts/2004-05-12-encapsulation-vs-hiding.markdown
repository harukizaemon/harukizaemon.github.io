---
layout: post
title: "Encapsulation vs Hiding"
alias: /2004/05/encapsulation-vs-hiding.html
categories:
---
It's becoming dull explaining the difference between _Encapsulation_ and _Hiding_. Maybe universities need to teach English as part of their computer science courses?

> **v. encapsulated, encapsulating, encapsulatesv. tr.**1. To encase in or as if in a capsule.2. To express in a brief summary; epitomize: headlines that encapsulate the news.

> **v. hide, hidden, hidesv. tr.**1. To put or keep out of sight; secrete.2. To prevent the disclosure or recognition of; conceal: tried to hide the facts.

Encapsulation **_does not_** mean that my classes need to have a high proportion of private methods to demonstrate good OO. It means the methods and data on a class should be cohesive - share and participate in common purpose and responsibility. Encapsulation **_has little_** to do with making all fields private and adding respective getters and setters.

A class has certain well defined and cohesive responsibilities - think normalisation (1BNF, 2BNF, etc.) but for classes - and those responsibilities are, by in large, public. Read _encapsulation_. The implementation of those responsibilities is hidden. The fact that some of the implementation lies is private methods is incidental and occurs for clarity and maintainability. Too many private methods is often an indication that a class has too much or wildly varying responsibility.
