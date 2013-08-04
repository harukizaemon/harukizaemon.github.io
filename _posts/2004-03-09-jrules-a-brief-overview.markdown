---
layout: post
title: "JRules - A brief overview"
alias: ["/2004/03/jrules-brief-overview.html", "/blog/2004/03/09/jrules-brief-overview"]
categories:
---
As requested, my brief overview of JRules. I'm sure I will have failed to answer many questions so feel free to ask.If I know the answer, I'll add it to the entry, othwerwise I'll either cop out and say I don't know or go and find out.As always, if I've made a mistake or three, feel free to point them out.

First off, I found JRules very easy to use. It comes with very good documentation. As I mentioned in a[previous entry](/blog/2004/03/09/re-spiking-and-tdd), foremost in my mind was to prove testability. I can safely say that rules can be fullyunit tested. So there are no excuses! ;-)

Like many rule engines, JRules usess a Rete network. The cool thing about the Rete algorithm is that rules with similar conditions (or subsets of conditions) are identified and evaluated together meaning that theconditions need only be tested the minimum number of times. This differs from java `d rules where typically everycondition in every rule must be evaluated regardless of whether we might already have tested for it in previous rule.

The centre (no, this is not a spelling mistake) of the programmatic world in JRules is the rule engine (`IlrContext`) also known as a context. Thecontext holds a set (`IlrRuleset`) of rules (`IlrRule`). The context also holds facts. Facts are simplejava objects and, as the name implies, are the things we know about the state of the processing "world". As you add(`assert`) facts about the state of your "world", the rule engine updates the `agenda`. At any given point,the agenda holds the rules that have conditions matching the current state and are therefore ready for firing.

Once a rule has fired, it is removed from the agenda and will not be made available for firing unless it, or anotherrule, modify the current state such that the rule conditions would once again be satisfied. This is pretty neat as itmeans you can have rules assert facts that cause other rules to fire.

Rules can be assigned a priority to assist in determining the firing order. The specific ordering rules are clearlydocumented so I wont repeat them here.

Rules can be grouped into "packets". Packets can then be referenced (and executed) by name. This can be very usefulif you have certain classes of rules that you know you will/wont need during processing and therefore wish toprogrammaticaly enable/disable at runtime. JRules 4.5 also provides a `RuleFlow` mechanism which is supposedlythe "preferred" method of achieving the same thing. I must admit that in our case, ruleflows didn't seem appropriate so we have so far stuck with packets.

There is one more facility for filtering rules from the agenda. These firing filters are applied to rules whosconditions have already been evaluated and are in the agenda. This differs from packets and ruleflows which selectivelyadd/remove rules prior to evaluation.

Rulesets support `in`, `out` and `inout` variables which are then available to all rules withinthe ruleset. This is one mechanism you could use to parameterise your rules.

JRules out-of-the-box provides three languages in which rules can be written:

* IRL - ILog Rule Language;
* TRL - Technical Rule Language; and;
* BAL - Business Action (Natural) Language.

I started off using IRL as it was the language given in the documentation examples. It's a fairly low-level language.I think from a geeks perspective it's the one to start with as it gives you a good understanding of what actually goeson. IRL can be written using the text editor of your choice. A simple example of IRL might look like:

```
when {?order: Order(product == Product.CHAMPAGNE);Customer(state == State.VIC; status == CustomerStatus.GOLD) from ?order;} then {?order.applyDiscountPercentage(15.00);System.out.println("Discount applied!");}
```

This essentially says, whenever an order for champagne is raised by gold customers in victoria, apply a 15% discountand print a message to that effect.

Each condition in the `when` clause is evaluated in the order declared. The conditions are AND'ed together.If the condition(s) are met, any statements in the `then` clause are executed when the rule is fired. Forcompleteness, there is also an `else` clause.

As you can probably see, the IRL is a Java-like language supporting, as far as I can tell, almost all usual javasyntax (including imports) plus the additional syntax required for describing rule conditions.

TRL is pretty much the same as IRL but is more verbose. For example:

```
when there exists an Order(product == Product.CHAMPAGNE)...
```

Strictly speaking, the BAL is actually an example language that demonstrates how you can buildyour own language on top of the underlying languages. Having said that, the BAL is almost feature complete and manybusinesses will choose it over the TRL and IRL. The intention is that business people can write in BAL. In actualality, I see this being about as likely to happen asbusiness people using Crystal Reports to write custom reports. If you can make it happen, great but I very much doubtit. However, BAL is much easier to read especially for business people and even for developers. So when, as a developer,you need to validate that the rule you've just written is correct, BAL is a god send. Even mildly complex rules arequite hard to follow in raw IRL and, for that matter, TRL.

BAL also requires a mapping layer between your Java objects and natural language. This isso that instead of writing `order.getTotal()` we can instead use `the total amount of the order`. On anagile project that is ramping up, I can see this as being an small but certainly not insurmountable issue due to theever changing domain model.

JRules comes with a GUI Editor that allows you to maintain rules in any of the supported languages. While it ispossible to create rules by hand in IRL and stored as plain text files, TRL and BAL require the GUI. The rules arestored in a repository (directory structures) that can then be exported to an IRL file for use by an application. Again,Not so much of a problem really but being forced to point and click when I really just want to type is very frustrating.If I could ask for one thing, it would be the ability to edit BAL rules in my editor of choice and then import them intothe repository.

BAL and TRL are both converted to IRL. IRL can be converted back into TRL. Unfortunately theconversion from BAL to IRL is a one-way street. Taking a look at the generated IRL was quite an eye-opener. The ruleeditor converts BAL into IRL that is almost impossible to understand by a human.

From a purely technical perspective, it matters little what language the rules are written in as they end up as IRLwhich is then parsed and executed by your application. Loading and running a test requires only one jar file(`jrules-engine.jar`) and can be as simple as:

```
public void testEngineIntegration() {// Define a simple rulefinal String rule = "rule TestRule\n" +"{\n" +"    when\n" +"    {\n" +"        not String();\n" +"    }\n" +"    then\n" +"    {\n" +"        assert Integer(57);\n" +"    }\n" +"};";// Parse the ruleIlrRuleset ruleset = new IlrRuleset();assertTrue("Rule parsing failed", ruleset.parseReader(new StringReader(rule)));// Create a context (rule engine)IlrContext context = new IlrContext(ruleset);// Execute the rule and ensure it was actually runassertEquals(1, context.fireAllRules());// Check the final state matches the expected stateEnumeration enum = context.enumerateObjects();assertNotNull(enum);assertTrue(enum.hasMoreElements());assertEquals(57, ((Integer) enum.nextElement()).intValue());assertFalse(enum.hasMoreElements());}
```

You can also load rules directly from the repository if you wish. So far we haven't attempted this and for the timebeing at least it's not a high priority as we will be loading the ruleset from the exported script.

The repository, apart from being a pain to use, has some other mildly annoying issues when working in an agile team.These are mainly to do with locking rules for editing. The rule editor allows you to create rule packages (not to beconfused with packets) and it is only at these levels that locking is possible. Again, not too much of an issue.Besides, you will quickly find your rules unmanageable if you don't group them at all. Packages are also useful if/whenwriting ruleflows. Remember however that these packages are purely logical and affect only the names of the generatedrules. Something you will most likely rarely have to worry about anyway.

Generally, I would recommend keeping your rules as simple as possible. If your find your rules blowing out to four pages, stop and think again. Rules can and should be decomposed into smaller rules. [James Ross](http://www.redhillconsulting.com.au/blogs/james) has been reading a great book on the subject, [Principles of the Business Rule Approach](http://www.amazon.com/exec/obidos/tg/detail/-/0201788934/104-3588424-8567908?v=glance), which he says is a must read for anyone embarking on business rules analysis.
