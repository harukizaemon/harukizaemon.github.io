---
layout: post
title: "Sometimes the problem does just go away"
alias: /2004/04/sometimes-problem-does-just-go-away.html
categories:
---
I've often joked that procrastination is the mother of invention. If I put something off for long enough, it'll either cease to be a problem or a more elegant solution will come to me. Certainly this doesn't always hold true nor is it always that simple but it does work out that way often enough for me to at least continue believing my own rhetoric :-)

The idea is to continue pushing the implementation of a solution further and further away. By that I mean, instead of thinking about the nitty gritty detail, start off by thinking in terms of the simplest interface possible and code to that. Imagine that it actually worked even if you've hard `d some behaviour.

It's nothing new. It's what good software developers do all the time (one hopes). It used to be called top down design. These days it's what TDD exponents typically do.

The important thing for me is that it IS important to sit around and think through the problem as fully as possible. Think about all the possibilities. What could we do. What would happen if we did option a versus option b? What might the customer want to do?

 Yes that's right, I'm suggesting actually solving a possibly imaginary problem. *Shreaks from the crowd*. The BIG difference is I'm not actually going to implement any of this. All I'm trying to do is validate that my assumptions, my abstraction, my simplification of the problem, my procrastination on implementation, my (some would say blind) adherence to YAGNI isn't going to come back and bite me if at all possible. And more to the point, trick those 95% of unused brain cells into doing more than just suck nutrients from my blood stream.

Again I stress I'm not going to do anything about it. I'm going to defer implementing anything until as late as possible. Hopefully by then my brain has been feverishly working away in the background mulling over all the things we came up with in the brain storming session (as distinct from design session) and magically come up with an elegant solution.

Sometimes it may be necessary to break the problem into smaller chunks. Flip the question around. Rephrase it. Try and push it into a different mould. See if all your assumptions still hold. Playing simple semantic games can help make a problem seem more familiar. Try calling it a `Displayable` instead of a `Page`. Maybe (and this is where the domain modelling dudes possibly get upset) `Finance` really is a product just like `MotorVehicleInsurance` even though the customer doesn't see it that way because it's not core business.

And you know what? More often than not, the problem just dissapears. We've abstracted the problem such that it no longer seems like one of many disparate, unrelated problems. It starts to conform to some other problems we've seen or implemented. Maybe we can tweak an existing implementation to support the extra behaviour.

To me, that's what problem solving is all about. It's about thinking through the problem sufficiently enough that it no longer seems like an isolated case. Some of the worst code I've ever seen was obviously written as a mass of solutions to completely (perceived) isolated problems.
