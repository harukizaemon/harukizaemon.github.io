---
layout: post
title: "Business Rules Goodness - Continued"
alias: /2004/05/business-rules-goodness-continued.html
categories:
---
Continuing with the business rules examples [thread]({% post_url 2004-04-01-more-business-rules-goodness %}), [James](http://www.redhillconsulting.com.au/blogs/james) looked at the examples of using `logical` to achieve a "compensating retraction" and asked me "so that's all very well and good but what happens if I'm not asserting a fact? What a happens if instead, I'm sending an email to my broker?"

My first reaction was that this is a separate problem. The fact that I was potentially sending an email based on the `SellOrder` seemed like an implementation detail that I didn't want cloduing the simple fact that, under certain conditions, I wanted to indicate my desire to sell.

After a little discussion, we came up with the following solution which, IMHO, elegantly maintains the atomicity of rules and the separation of concerns. I've taken some liberties with the syntax and I've not actually tried this in JRules but it does serve to demonstrate the concept:

```
rule BrokerInformedOnNewSellOrder {when {?order: SellOrder();not SellOrderActive(order == ?order);} then {sendMessage("Sell stock");assert SellOrderActive(?order);}}
```

As the name suggest, this simply informs the broker on any _new_ `SellOrder`. The key here is that we introduce a new _fact_ `SellOrderActive`. If we see an order that isn't active, we'll send a message and `assert` that it is now active.

(NB. `sendMessage()` isn't syntactically correct for JRules but you get the idea.)

Next we need to know what to do if the `SellOrder` is `retracted` (either explicitly or implicitly):

```
rule BrokerInformedOnRetractionOfSellOrder {when {?active: SellOrderActive();?order: SellOrder() from ?active;not SellOrder(?this == ?order);} then {sendMessage("No longer sell stock");retract ?active;}}
```

This rule says that anytime we _think_ we have an active order but the `SellOrder` itself no longer exists, `retract` it and send another message to the broker indicating that we no longer wish to sell.

As usual, I'll re-write this rule in [JESS](https://mail.geekisp.com/horde/util/go.php?url=http://herzberg.ca.sandia.gov/jess). Thanks go to the creator Ernest Friedman-Hill for clarification on the exact syntax:

```
(defrule broker-informed-on-new-sell-order?order <- (sell-order)**(not (sell-order-active ?order))**=>(sendMessage "Sell stock")(assert sell-order-active ?order))(defrule broker-informed-on-retraction-of-sell-order?active <- (sell-order-active ?order)**(not ?order <- (sell-order))**=>(sendMessage "No longer sell stock")(retract ?active))
```
