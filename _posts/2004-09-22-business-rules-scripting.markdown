---
layout: post
title: "Business Rules != Scripting"
alias: /2004/09/business-rules-scripting.html
categories:
---
As Business Rules come into vogue (again?) and the tools proliferate, there will be the usual fumbling about as many come to terms with what it all really means. How do we use these things? What should I look out for, the pitfalls, the traps? Are there any "patterns"? But above all, the greatest difficulty it seems, is coming to terms with the idea that Rule Engines ARE NOT procedural scripting languages.

The [Rete Algorithm](http://herzberg.ca.sandia.gov/jess/docs/70/rete.html) (pronounced REE-tee and Latin for net) was developed by Charles L. Forgy at Carnegie-Mellon University in the 1970's and is used in most modern high-performance rule engines. Rete is able to efficiently handle very large numbers of rules.

One of the most important features of the Rete algorithm lies in its ability to identify and subsume rules with similar predicates. Because of this, predicates need only be evaluated once. This differs from procedural (java `d) rules where every predicate in every rule must be independently evaluated, regardless of whether the same predicate might already have been evaluated in another rule. It can also locate conflicting rules. Something that's almost impossible in traditional, procedural, languages.

When it comes to codifying business rules, well factored Java code can be rather difficult to understand. After a couple of weeks away, it can often take the original developer some time to get back up to speed with their own code, let alone someone elses. On the other hand, Rules are declarative statements of fact. That means no trudging through tens or even hundreds of lines of procedural code to understand what will happen under various conditions. Weeks, months or even years later you can go back to the rule definitions and immediately understand their meaning and intent.

Rule engines share much in common with Relational Databases. They are based on tuples and predicate calculus. You don't navigate Relations (Tables), you join them. Similarly you don't navigate facts, you join them. Both suffer (or at least have suffered) similar problems in terms of performance and optimisation.

Business Rules should be simple and atomic. They should make inferences. They should not be calling out to databases nor making countless remote calls. That's what application code is for. Much like the difference between queries and stored procedures.

Analogies aside, the fact remains (no pun intended) that rules are not procedural, they are declarative statements of fact! Writing business rules requires very clear, concise and logical thought, as much if not more so than procedural code.

Rule-flow, priority, salience, etc. are mechanisms that allow some degree of procedural control and should therefore be considered a last resort, not the basis for a rule engine framework. While sometimes useful, all are frowned upon by rule advocates in much the same way as OO design frowns upon public variables.

If you can't or won't make the necessary shift from a procedural to a declarative mind set then I suggest you try [BeanShell](http://www.beanshell.org/), [Rhino](http://www.mozilla.org/rhino/), [Groovy](http://groovy.`haus.org/) or any of the myriad scripting languages available. There is nothing to be ashamed of with this approach but it is most certainly NOT the same thing.
