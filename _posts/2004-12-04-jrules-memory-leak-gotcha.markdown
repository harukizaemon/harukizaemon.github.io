---
layout: post
title: "JRules Memory Leak Gotcha"
alias: /2004/12/jrules-memory-leak-gotcha.html
categories:
---
About 6 months ago we were profiling our application to ensure we had no memory leaks, etc. We did find some and we were able to fix them pretty much immediately. However, today I happened to be chatting with a colleague who is investigating a memory leak in another application and it sounded scarily similar. So in the interests of all you [JRules](http://www.ilog.com/products/jrules) developers, here's a little gotcha.

JRules maintains a binary, 1-to-many, association between the rule set (`IlrRuleSet`) and the, possibly, many instances of the working memory (`IlrContext`) - Also referred to as a "rule engine" by the JRules documentation. I'll spare you my diatribe on binary associations for now, suffice to say that if you weren't aware of this little "feature" (or if you were and simply hadn't given it much thought) you're in for a nasty surprise.

When you're done with an `IlrContext` the natural thing to do would be to simply remove all application references to it and let it be garbage collected. Unfortunately, due to the two-way nature of the relationship, this doesn't have the expected effect. Instead, because the rule set still holds a reference to the context, it will NEVER be garbage collected.

To combat this problem, ILOG thankfully provided a somewhat innocuous looking method `IlrContext.end()`. To quote from the documentation:

> Prepares this rule engine instance for garbage collection. After this call, the engine will not keep any reference to this rule engine instance. The rule engine instance will be detached from the ruleset and will no longer be notified of modifications on the rules. The rule engine instance will also disconnect all its tools and all the related resources will be released. If the application does not keep this object, it is then subject to garbage collection.

In other words, anytime you've finished with a context and wish it to become a candidate for garbage collection, be sure to call `end()` or be prepared for a slow and painful application death as the heap runs out.

One final tip, if you make use of context pooling, be sure to also call `IlrContext.reset()` _before_ returning it to the pool. This will remove all references to your application objects within the context.

&lt;blatant-plug&gt;If you're in the market for a cheaper alternative, you might like to try out the [latest version](http://blogs.`haus.org/archives/000901_drools_20beta18_released.html) of [Drools](http://drools.`haus.org).&lt;/blatant-plug&gt;

P.S. If anyone from ILOG is listening, this is exactly the kind of problem [`WeakReference`](http://java.sun.com/j2se/1.4.2/docs/api/java/lang/ref/WeakReference.html)s (and [`WeakHashMap`](http://java.sun.com/j2se/1.4.2/docs/api/java/util/WeakHashMap.html)s in particular) are designed to prevent :)
