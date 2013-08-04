---
layout: post
title: "More Business Rules Goodness"
alias: /2004/04/more-business-rules-goodness.html
categories:
---
I've always loved the idea of rule-based applications but never really had the opportunity to build one. And I have to say I'm having a lot of fun [using a rules engine]({% post_url 2004-04-28-bal-cancer %}) on this project. Since we dumped the BAL in favour of the IRL in JRules, productivity has sky-rockected. FWIW, TDD and rules-engines are a perfect match (a fact I'll blog more about in the coming days). I'm in geek heaven! About the only thing I wish I had now was an IntelliJ plugin (like the AspectJ one).

So anyway, my intention is that step-by-step, I'll try and document my progress starting with a little of what I discovered today by way of a rather contrived example:

```
rule MessageIsGeneratedOnSignificantStockMarketIndex {when {?index: Index();evaluate(index.value > 3000);} then {assert Message("Today the stock market rose above the psychological 3000 barrier");}}
```

This example says that whenever the stockmarket index rises above 3000, we assert that it was a significant event. (Actually JRules has some other nifty stuff to do with associating timestamps with events but I'll blog about that another time.) The important thing to notice is the `assert` keyword. This `asserts` a new fact into the "knowledge base". This new fact will remain "forever" or at least until another rule `retracts` it.

Simple assertions such as this are great when you know that the asserted fact will always be true independent of the triggering condition. In the example, it will always be true that at some point in time, the stock market index rose to a significant level, even if the index drops again.

But what if we have a fact that only holds while the condition holds? In such a case, we'd need a "compensating" rule to `retract` the fact when the condition changed. This could get quite ugly. Thankfully, JRules provides a neat solution:

```
rule SellOrderRaisedWhenStockValueReachesMinimum {when {?stock: Stock();evaluate(?stock.value >= 30);} then {assert **logical** SellOrder(?stock);}}
```

This rule says that we will place a sell order for any stock that rises to 30 dollars. The key difference here is the use of the `logical` keyword. This tells JRules that the assertion onlyholds while the triggering condition holds. That is, while the stock value is at least 30 dollars, the sell order remains. However, if the stock value drops below 30 dollars, JRules will automatically retract the fact for us. What's even better is that if the assertion of the SellOrder causes other rules to fire and therefore assert more facts, all those that were declared as `logical` will all be `retracted` as well. How cool is that?!

In our application, probably >99% of all rules will use the `logical` form of `assert`. This allows quite complex interactions between essentially independent rules.

If you find yourself having to structure your rules with priorities and worrying too much about the interactions between rules, it's likely your individual rules are doing too much. Ensure your rules should be as atomic as possible. Seperate "inference" rules from "do stuff" rules". And don't be tempted to simply change the state (ie property values) of existing facts. Instead, always assert new facts (as we have done in the examples above).

I thought I'd also show you the same rules using [JESS](http://herzberg.ca.sandia.gov/jess/index.shtml).

```
(defrule message-is-generated-on-significant-stock-market-index(index (value ?value))(test (> ?value 3000))=>(assert (message (text "Today the stock market rose above the psychological 3000 barrier"))))(defrule sell-order-raised-when-stock-value-reaches-minimum?stock <- (**logical** (stock (value ?value)))(test (>= ?value 30))=>(assert (sell-order (stock ?stock))))
```

You'll note that in JESS, the `logical` keyword is associated with the _condition_ (or _LHS_) rather than the _action_ (or _RHS_).

In many ways JESS provides a richer environment than JRules but I admit the syntax is less obvious to novice users.
