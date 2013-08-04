---
layout: post
title: "Beware The Cross-Product Join"
alias: /2004/10/beware-the-cross-product-join.html
categories:
---
An intersting discussion started on the [Drools](http://drools.`haus.org) user mailing list regarding some problems writing a rule. The particular problem is not unique to business rules though. RETE-based inferences engines share much in common with relational databases and in fact this particular problem can affect SQL queries in the same way as it affects business rules.

Let's say we wanted to find all pairs of people that were maternal siblings (ie that had the samemother). In SQL we could write a query like this*:

```
SELECT * FROM Child c1, Child c2WHERE c1.motherId = c2.motherId
```

If we imagine we have only two children in our database, Bob (childId = 1) and Mary (childId = 2),both having the same mother, this query would generate four rows:

* Bob, Mary
* Mary, Bob
* Bob, Bob
* Mary, Mary

This is called a _cross-product_; every row is joined to every other row. This results in rows we're not interested in: Bob, Bob and Mary, Mary. So the first thing we would do is try and ignore rows where the child was the same:

```
SELECT * FROM Child c1, Child c2WHERE c1.motherId = c2.motherId**AND c1.childId != c2.childId**
```

Which results in:

* Bob, Mary
* Mary, Bob

The next thing you'll notice is that we still have redundant rows - rows that mean the same thing. There are a few "tricks" to avoiding this and really come down to a knowledge of the underlying attributes of the tables involved. The simplest in our case is to change the condition:

```
SELECT * FROM Child c1, Child c2WHERE c1.motherId = c2.motherId**AND c1.childId &lt; c2.childId**
```

By imposing an arbitrary ordering, we prevent rows being joined to themselves and ensure that for any two siblings, we only get one row. Best of all, this technique translates directly into the implementation of business rules.

Not only do cross-products produce redundant and possibly incorrect results, the extra tuples (rows) generated as a consequence can cause your rule engine to grind to a halt.

* I realise that no one is going to model Children and Mothers in different tables but please cut me some creative slack ;-)
