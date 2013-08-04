---
layout: post
title: "Tests and Refactoring"
alias: /2004/03/tests-and-refactoring.html
categories:
---
TDD means no writing production code unless there is a breaking test right? Most practitioners agree on this. However the topic of TDD in the context of refactoring came up in a conversation last night. One of my colleagues challenged that because refactoring wasn't changing the behaviour just the implementation, it can largely be done without a breaking test.

I'm not convinced that is the case. If I think about why we refactor there really is only one possibility - something is wrong with the code. Otherwise, why would we bother. It seems to me this "wrongness" then breaks down into three main causes:

* Missing or superfluous functionality;
* Deprecated implementation; and;
* Smelly code.

The first case is easy to justify writing tests for - clearly, any change in functionality necessitates a test.

The second is more subtle but again, clearly there was some reason for changing the implementation (such as persisting to a database versus in memory) in which case we would need to add some kind of test(s) to verify this.

The third case is a little harder. On what basis do I determine that the code smells and therefore needs a cleanup? In the specific case from last night, Dave had renamed an abstract class to `AbstractXXX` and extracted an interface. But he had written no extra tests. Admittedly, I usually write a test to ensure that my class implements the necessary interface(s) so he could have done that. However the reason he made the change in the first place was because a checkstyle check was barfing. Why was it barfing? Because it had detected a likely code smell. To my mind, this is a breaking test. It's a test for code quality as opposed to functionality.

Unfortunately, not all code smells can be picked up by machine (though I'm trying very hard lol). But I thought it was interesting to challenge my own notions of what constitutes a test and, getting back to a [previous entry]({% post_url 2004-03-27-more-broken-tests %}) (as is my habit), what kind of test is appropriate in various circumstances.

I also want to see if I can come up with many cases where it is justifiable to modify code without some kind of test to prove that the change is in fact necessary. If I can't prove that a change is required (and what better way than with a breaking test) why would I make it?
