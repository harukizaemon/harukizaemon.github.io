---
layout: post
title: "Don't mock infrastructure"
alias: /2003/11/don-mock-infrastructure.html
categories:
---
While there are a few cases where I feel you may need to, for the most part I feel very strongly that mocking infrastructure is a fundamentally flawed approach to testing.

I have been down the path of mocking JNDI, JDBC, JMS, UserTransaction, etc. and whilst at the time I thought this was all very cool, I did wonder what the hell I was doing writing all this stuff.

Eventually I realised that the problem I was having was one of abstraction. Or more properly a lack thereof. I was tackling the wrong problem. Instead of thinking my class needs to lookup JNDI therefore I need to mock out JNDI, I started to think about the problem in general. That is, a mechanism for looking up services.

A perfect example is [Using mock naming contexts for testing](http://weblogs.java.net/pub/wlg/677). While I think the intention is great, I don't believe it goes far enough.

JNDIs Context has a multitude of methods I'll never use, not to mention the fact that they all throw NamingException which quite frankly my code has never known how to deal with and usually throws a ImGivingUpException.

What I really need is a ServiceLookup interface with a few very simple methods like, not surprisingly, lookup(Class). Then I can have two implementations, one a MockServiceLookup that I can create inside my JUnit test and return whatever I like and a JndiServiceLookup that actually understands how to go about performing all the nasty JNDI stuff.

Not convinced? Ok well now imagine I have more than one way to obtain a service. Sometimes it's through JNDI and sometimes I instantiate a local object. If all my code is tied to a JndiLookup what will I do? Instead, now that my code is tied to an interface, I can create an implementation that say first looks in a local configuration and then if that fails, delegates to the JNDI version. Or maybe a SuperServiceLookup that holds a list of other ServiceLookup implementations and just delegates to them as appropriate. Again referencing only the interface and not any concrete implementation.

Now think JMS. Instead of writing to/reading from queues, why not have MessageQueue interface with two simple methods: read(Message) write(Message). then I can have a MockMessageQueue that I instantiate in my test and a JmsMessageQueue that talks to the real thing.

Again, we've abstracted the problem. That is, sending/receving messages. Not talking to JMS.

So now, back to the original example, I can see that a MockInitialJndiContext may well be useful but probably only for one thing: testing my JndiServiceLookup. And then I can probably just have a constructor in JndiServiceLookup that accepts an Context and a default constructor that creates an InitialContext. Then use something like [Easy Mock](http://www.easymock.org) to fill in the rest for me.

When it comes down to it, InitialContext is really a convenience for calling InitialContextFactory anyway which in turn creates a Context which is after all an interface. So why be constrained by someone elses API?

My rule of thumb is that in general, I want to be mocking out my own interfaces not someone elses. I'll usually have many places where I need to mock out a given interface of my own and at most one place where I need to mock out someone elses.

Using Factory and AbstractFactory, etc. for creation will then allow you to use Strategy, Decorator, etc. for different behaviour ie. Mock vs JNDI, JMS, etc. Martin Fowlers [Patterns of Enterprise Application Architecture](http://martinfowler.com/eaaCatalog) also has some great ways for doing this for enterprise applications as well.

Using testability to drive my thinking forces me to abstract problems in a way I hadn't previously and good design emerges. This is why I'm such a huge TDD bigot. Because as Dan North point out, [TDD is not about testing](http://www.sys-con.com/story/?storyid=37795&DE=1).
