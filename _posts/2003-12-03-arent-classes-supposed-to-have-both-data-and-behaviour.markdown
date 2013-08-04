---
layout: post
title: "Aren't classes supposed to have both data and behaviour?"
alias: /2003/12/arent-classes-supposed-to-have-both-data.html
categories:
---
A bit late off the mark but I only just found an old article on [why getters and setters are evil](http://www.javaworld.com/javaworld/jw-09-2003/jw-0905-toolbox.html?). Combine this with my preference for immutable objects, and JavaBeans often become the devils work. It also fits in nicely with another article I read recently, Martin Fowlers [AnemicDomainModel](http://www.martinfowler.com/bliki/AnemicDomainModel.html), in that both talk about the value of spending the time to put behaviour back on classes rather than having dumb data objects with essentially procedural functions (in the way I usually see entity and session beans used).

It's a strikingly difficult concept to grasp. I know I struggled with it for years. And I've found it even harder to convince others of since I did get it.

Here's a typical example I see quite often. Now this is only my opinion of course but instead of:

``` java
account.setStatus(AccountStatus.CLOSED);
```

I'd prefer to see:

``` java
account.close();
```

Internally, the `Account` might simply look like:

``` java
public void close() {
    setStatus(AccountStatus.CLOSED);
}
```

But it might also do something more. In this trivial example, the benefits are not so obvious but, when the logic becomes more complex, the `setStatus()` method can become quite large and usually ends up with a `switch` statement that (hopefully) delegates to separate methods anyway. So why not just call the methods explicitly. After all, the `AccountStatus` class is really an implementation detail, albeit a common one.

Having said this, deciding where behaviour belongs is often quite difficult. I often find myself using Strategies and Composites instead of inheritence but I've also found this a difficult concept to communicate, with doubters usually exclaiming _"But aren't classes supposed to have both data and behaviour?"_ :-)
