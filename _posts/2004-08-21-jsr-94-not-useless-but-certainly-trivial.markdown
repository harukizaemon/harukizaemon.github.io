---
layout: post
title: "JSR-94 Not Useless But Certainly Trivial"
alias: /2004/08/jsr-94-not-useless-but-certainly-trivial.html
categories:
---
I watch with interest as the [Rule Engine chatter](http://www.theserverside.com/news/thread.tss?thread_id=28211) begins to increase. I truly believe it's an area much ignored by the great majority of developers.

If you're not aware, there is a [JSR in the works](http://www.jcp.org/aboutJava/communityprocess/review/jsr094/) to provide a common interface for integrating rule engines. In its current form, JSR-94 provides little more than a common interface for creating a context/rule engine and marshalling objects in and out. It is trivial to implement this yourself.

The fact is (pun intended) that the JSR provides little more than would result in developing a system that makes use of a business rules engine keeping in mind requirements for testability of rules and loose coupling (a.k.a. good design?). Only the JSR is considerably more verbose.

To illustrate, we have a large number of rules in our current system and a very small but useful set of interfaces:

```
public interface RuleEngineFactory {public RuleEngine createRuleEngine();}public interface RuleEngine {public void reset();public void addFact(Object fact);public void addFacts(Set facts);public void execute();public Set getFacts(Class type);}public interface RuleEnginePool {public RuleEngine getRuleEngine();public void returnRuleEngine(RuleEngine ruleEngine);}
```

Add to these a few very light-weight implemenation classes (and some [Dependency Injection](http://www.martinfowler.com/articles/injection.html) for good measure) and you have pretty much everything you could need from an integration standpoint.

```
public class JRulesRuleEngineFactory implements RuleEngineFactory {private final IlrRuleset _ilrRuleset = new IlrRuleset();public JRulesRuleEngineFactory(Reader reader) {if (!_ilrRuleset.parseReader(reader)) {throw new RuntimeException("Error parsing rules");}}public RuleEngine createRuleEngine() {return new JRulesRuleEngine(new IlrContext(_ilrRuleset));}}public class ThreadLocalRuleEnginePool implements RuleEnginePool {private final ThreadLocal _engines = new ThreadLocal() {protected Object initialValue() {return _ruleEngineFactory.createRuleEngine();}};private final RuleEngineFactory _ruleEngineFactory;public ThreadLocalRuleEnginePool(RuleEngineFactory ruleEngineFactory) {assert ruleEngineFactory != null : "ruleEngineFactory can't be null";_ruleEngineFactory = ruleEngineFactory;}public RuleEngine getRuleEngine() {return (RuleEngine) _engines.get();}public void returnRuleEngine(RuleEngine ruleEngine) {assert ruleEngine == getRuleEngine() : "ruleEngine not allocated from the current thread";ruleEngine.reset();}}
```

We theoretically have plugability of rule engines but to believe that this might be useful or even practical is naive at best.

Unfortunately, (or fortunately depending on your perspective) the biggest part of using a rule engine is in analysing and writing the rules themselves. Granted, many engines use a [Rete Algorithm](http://web.njit.edu/all_topics/Prog_Lang_Docs/html/jess51/rete.html) but to suggest that all Rete-based rule engines are the same is akin to saying that two Java applications are the same. [JRules](http://www.ilog.com/products/jrules/) and [JESS](http://herzberg.ca.sandia.gov/jess/) both use a Rete network and both are implemented in Java but the language, tools and behaviour (not to mention performance characteristics) of each differs sufficiently to render the conversion process rather less than trivial.

Surely, few of you would be imagine that a switch from using JSPs to say [Velocity](http://jakarta.apache.org/velocity/) in a system of any significant size would be an overnight job. Similary, a switch from [Struts](http://struts.apache.org/) to say [Tapestry](http://jakarta.apache.org/tapestry/) or [JSF](http://java.sun.com/j2ee/javaserverfaces/index.jsp) would be non-trivial. All of these technologies attempt to solve essentially the same problem but all come with a slightly different design philosphy. No matter how standardised the interface, if the behaviour of the system on the other side is different, the illusion breaks down.

Anything that lowers the barrier to entry for those wishing to explore the use of declarative rules is to be applauded however there are far more important problems for an organisation to solve than transparency of the underlying rule engine implementation. Atomicity, testing, managability, education, analysis, configuration, understandability, to list but a few. It is no coincidence that these are largely non-technical.
