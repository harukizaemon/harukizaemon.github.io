---
layout: post
title: "Naming Tests"
alias: /2004/03/naming-tests.html
categories:
---
Your business analysts have no doubt spent days coming up with test cases for an interest calculation. Writing it all out in nice, simple, plain english so you can understand what you need to implement. They've probably produced half-a-dozen scenarios for you to test against. Don't go and spoil it all by naming your method `testInterestCalculation`.  I want to see `testInterestCalculationWithFixedRateFor24Months` or something similar.

I know there will be those who say that it's redundant information to encode into the test name. I mean just look at the code right? Phooey! Ever written a method named `remove` that actually added something instead? If the answer is yes, go directly to jail. Do not pass go. Do not collection a hundred dollars! If, like most sane people, the answer is no, that's because we name methods to indicate intention. We then look at the code to see how it's been implemented.

Ever spent what seemed like hours trying to come up with a good name for your class? Why bother? Again, because it's so important to communicate not only to others but also to yourself the role of the class, it's purpose for being. Without an appropriate name, it's often hard to know what the classes responsibilities should be.

If I go onto a project and need to get an idea of what the system does, the first place I look is the test suite. It should tell me almost everything I need to know about the implementation.

So do me a favour and spend a bit of time naming your test cases something meaningful. Please! I beg you. It'll stop me wasting your time asking lots of stupid questions ;-)
