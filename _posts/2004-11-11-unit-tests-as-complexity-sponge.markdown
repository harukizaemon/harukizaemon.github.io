---
layout: post
title: "Unit Tests As Complexity Sponge"
alias: /2004/11/unit-tests-as-complexity-sponge.html
categories:
---
A number of people have variously [commented](http://www.sys-con.com/story/?storyid=37795&DE=1) that unit tests may in fact be more about design than actual testing. Many others (the links elude me at present) have also complained about the undue burdon imposed by a large number of unit tests and that because of this, and other very sound reasons, they prefer acceptance tests. If I was ever in any doubt about the importance of acceptance tests, I was certainly convinced after the last project where acceptance tests would fail where no unit test had, due no doubt in large part to the fact that the acceptance tests also acted as integration tests.

One thing I did notice however was that over and above their usefuleness as a design tool, unit tests seemed to act as yet another positive constraint helping reduce the overall complexity of the code. Because developers were forced to write unit tests, they were forced to produce relatively simple, testable code. Much simpler, I believe, than would have been the case otherwise. The down side to this testability was that in many cases, the corresponding unit tests were rather more complex than we would have liked. And, as noted previously, complex tests tend to be brittle and this has a knock on effect with respect to maintenance. But does this really matter?

Ultimately what is important is working software (for which you have acceptance tests) and clean, [easy to understand](http://www.eaves.org/blog-archive/000071.html) code that is hopefully cheaper to maintain. You could choose to throw away all those "dirty" unit tests once you reach production and rely solely on your acceptance tests; or you might choose to buy new ones through refactoring/re-writing; or you may decide that the unit tests are worth the extra effort to maintain.

Whatever the course of action, it seems to me that, yet again, unit tests have benefits beyond simply (or not as the case may be) producing "correct" code.
