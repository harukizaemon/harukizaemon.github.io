---
layout: post
title: "It's an obsession"
alias: /2003/12/it-obsession.html
categories:
---
As I noted earlier, [Perryn Fowler](http://www.jroller.com/page/perryn/20031211#tail_wags_dog) spotted the "deliberate" bug in the [clock code]({% post_url 2003-12-07-when-is-a-clock-not-a-clock %}). These two entries prompted some interesting commentary suggesting (possibly quite rightly) that I've been obsessing about unit tests.

While I think the points raised are valid, I'm compelled to note that my obsession lies with improving code quality and Test Driven Development (TDD) in general and not specifically with unit testing.

My focus recently has been on unit testing because I disagree with the idea that _developers focus too much on unit tests_. My experience is that developers are more than capable of writing great swags of integration and functional tests over poorly constructed, hard to understand and hard to maintain code. Not to mention that the tests are written as an after thought, are often difficult to automate and take countless hours to run even when they are automated.

I would never claim that one should only write unit tests. Rather, I would simply ask that you start with a test. Some have noted that TDD takes us back to the "good old days" of top down development :-)

FWIW, I write significant numbers of integration tests, often (though not always) before I've written the corresponding unit tests. Why? Because I need to test my assumptions about the behaviour of various layers, libraries etc. on which my code will depend. Then when I've verified my assumptions (probably learning much more than I had anticipated along the way) I am happy to ignore those layers for as long as possible.

And there is no doubting that there is an absolute, cannot do without, requirement for a comprehensive functional test suite. Frameworks such as [Fit<a>, jWebFit (part of the <a href="http://jwebunit.sf.net/">jWebUnit](http://fit.c2.com/) package) and [Fitnesse](http://www.fitnesse.org/) are interesting in that they hold the promise of writing functional tests before writing the code! But as yet, I've no practical experience worthy of note using them. I assure you that when I do, I'll let you know :-)

Although I would [never recommend mocking infrastructure]({% post_url 2003-11-23-dont-mock-infrastructure %}), people have used [service stubs](http://www.martinfowler.com/eaaCatalog/serviceStub.html) since the dawn of software development. There is no denying that these are vital when access to back-end systems is not always guranteed. In keeping with the [fractal nature of software](http://www.relisoft.com/book/proj/1software.html), I don't see there is a difference between stubbing a mainframe application for testing business logic, and stubbing and application server for testing presentation logic.

None of this, however, in anwyay explains my interest in (obsession with?) TDD.

My strongly held belief is that [TDD isn't actually about testing](http://www.sys-con.com/story/?storyid=37795&DE=1). For one, it's about documenting requirements and letting those requirements drive the design of the application. And importantly, the observation of many that easily testable code is designed well, is easier to understand and easier to maintain.
