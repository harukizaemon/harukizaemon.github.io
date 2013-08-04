---
layout: post
title: "Well Behaved Rules"
alias: /2004/06/well-behaved-rules.html
categories:
---
I have [previously]({% post_url 2004-05-26-business-rules-fallacy-1 %}) made a comparison between rule engines (and the [RETE](http://herzberg.ca.sandia.gov/jess/docs/61/rete.html) algorithm in particular) and SQL databases. Business rule languages are declarative as is SQL, both being based on predicate calculus. Both suffer (or at least have suffered) similar problems in terms of performance and optimisation.

I recall many years ago, tuning my queries within an in inch of their (or my more likely) life. Re-ordering the `WHERE` clause, changing `JOIN` conditions, even changing the order in which columns were returned.

Thankfully these days, even the simplest of SQL database engines have some form of optimisation built-in. High-end systems such as Oracle have very sophisticated optimisation techniques. I can pretty much write any old SQL (with caveats) and know that I'll get at least acceptable performance in most cases.

The RETE algorithm (and it's successor [RETE-II](http://www.pst.com/rete2.htm)) is amazingly good and rule engines have also come a long way but certainly not as much as ye-olde RDBMS. So there are still some things you need to consider when writing rules.

Without going into too much detail, the RETE algorithm builds a network of nodes representing the conditions of your rules and the matching facts. In general, the smaller the network, the better the performance.

The first thing to note is that any rules sharing common conditons are optimised into a single node. However, with many rule engines, this is sometimes only possible if the conditions are listed in the same order. So for any N rules having M conditions in common, order the conditions so that the first M are the same.

Now that your are conditions are in the same order, you'll be interested to know that the exact order is in fact important. Because each condition is like an SQL `JOIN`, you need to place the MOST restrictive conditions first. That is, place the condition that is LEAST likely to be matched FIRST. This is no doubt familiar to anyone who has ever tuned SQL.

Iimagine we're trying to find two people with the same parent. We could do this (JRules code examples, just ask me if you want to see JESS as well):

```
?a: Person()Person(getParent() == ?a.getParent())
```

This has one glaring problem: It's essentially a _cross product_! So we need to fix it:

```
?a: Person()?b: Person(getParent() == ?a.getParent())evaluate(?a != ?b)
```

Now, as we've shown above, the number of conditions evaluated is also important. Anytime we can short-curcuit the conditions, we save ourselves another join. So once again, we can re-write our conditions:

```
?a: Person()Person(?this != ?a; getParent() == ?a.getParent())
```

I've found these simple techniques can result in the difference between rules running in seconds versus `OutOfMemoryError`s!

To be continued...
