---
layout: post
title: "You Are What You Measure"
alias: /2004/09/you-are-what-you-measure.html
categories:
---
[James](http://www.redhillconsulting.com.au/blogs/james) mentioned that one of his [colleagues](http://www.wrytradesman.com/) had been compiling stats on various projects and how they rated in terms of [Cyclomatic Complexity](http://www.onjava.com/pub/a/onjava/2004/06/16/ccunittest.html) (A.K.A. McCabe's Complexity). Quite rightly, everyone on the team was ecstatic to find that the project ranked top of the list (ie. lowest average complexity) in a field of various open and closed source projects.

Tools such as [Checkstyle](http://checkstyle.sourceforge.net) and [PMD](http://pmd.sourceforge.net) (to name but a few) make collecting these kinds of statistics very easy. What's more, incorporating these tools into your developer and [continuous integeration](http://www.martinfowler.com/articles/continuousIntegration.html) builds makes enforcing limits on whatever metrics you like almost trivial. And that is exactly what we had done.

Comprehension and testability of code vary with complexity. Overly complex code is difficult to maintain, difficult to test and more likely to contain bugs (hidden flaws). Experience on previous projects (rather than just theory) had taught us that a low value for cyclomatic and it's cousin NPath complexity was indeed a good predicter of problematic code. On these projects, we had always cranked the threshold right the way down and had found this to have a positive influence on the code base.

In true [Pavlovian](http://employees.csbsju.edu/tcreed/pb/pavcon.html) style, and with nothing but ultruistic intentions, we decided to make cyclmatic complexity (along with dozens of others) a constraint on this project as well. It obviously follows then that our project would place at or near the top of the list. We had made it a constraint, enforced it in our build and therefore the only possible outcome was a low average.

But as always, the devil's in the detail. It soon became apparent that like so many other checks, it is possible to subvert the process. The worst part is that few developers do so out of malice. More often than not a check will fail and the developer will (often quite ingeniously I might add) code their way around it. Without knowing any better, it is quite easy to inadvertently increae actual complexity whilst adhering to simple metrics.

I have a love-hate relationship with development tools. In the right hands they are invaluable but you can't just give inexperienced developers tools and expect them to do as well. Tools + Monkeys != success. In fact as we discovered, left unchecked the opposite is much more likely to occur. Luckily for us we had a much better ratio of experienced/inexperienced developers so we caught most of the problems early and entered into intense education (ala Clockwork Orange - just kidding). I think it's safe to say that in the end, the benefits certainly outweighed the potential dangers.

While certainly interesting, relying on a single metric can be very dangerous. Blind adherence to any number of metrics can disatrous. In general it's often far safer to let tools do the grunt work to find the smells and even make recommendations, then employ the expertise of more experienced developers to deoderise. It's also a good idea to use several metrics in combination that address different, sometimes competing, concerns.
