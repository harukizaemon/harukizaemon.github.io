---
layout: post
title: "Writing Readable Code"
alias: /2005/03/writing-readable-code.html
categories:
---
Sometime ago I wrote about my experiences pairing with [James](http://www.redhillconsulting.com.au/blogs/james) and the effect that [_listening_ to your code]({% post_url 2004-09-01-if-code-could-speak %}) can have on the naming of variables, methods, classes, etc. Then last night, I had a very interesting discussion with [Owen Rogers](http://dotnetjunkies.com/WebLog/exortech/Rss.aspx) about, among other things, the effect that TDD has on your code.

One of the observations was that, in order to write a test (and by inference mainline code) that is understandable, you need to name your methods very carefully. The specific example that Owen gave was a factory method for creating an `XmlWriter`. The usually accepted convention is to name the method `createXXX` or `newXXX` as in:

```
public class XmlWriterFactory {public XmlWriter createWriter() {...}}
```

Which would then be used something like:

```
order.write(factory.createWriter());
```

The problem with this is that it doesn't really flow particularly nicely. To illustrate, let's re-write this last line of code into English:

> order (dot) write, factory (dot) create writer

Reading the this aloud seems a little _unpleasant_ and requires you to think a little too much about what is going on, even with such a simple line of code.

The suggested improvement was to buck the "trend" and rename some of the methods so that the code would look something like:

```
order.**writeTo**(factory.**xmlWriter**());
```

This can then be converted plain English that reads something like:

> order (dot) write to , factory (dot) xml writer

Much nicer! Written this way, it's much easier to get a feel for what is going on and for that matter to ignore the occasional syntactic noise such as "dot" (and for that matter "factory"). Interestingly however the necessary changes to the factory class make the class itself seem a litte odd:

```
public class XmlWriterFactory {public XmlWriter **xmlWriter**() {...}}
```

If you looked at the modified factory class in isolation it might not be obvious what the method `xmlWriter` was doing. Because there is no verb attached to the name, we have lost some of the meaning of the method when viewed on its own. And, to me, this is one major difference between a traditional _design-your-classes-up-front_ approach, versus a more _design-your-classes-as-you-need-stuff_ strategy.

It seems there is a trade-off between understanding a class in isolation versus understand its use in context and the nice thing about simple, readable tests, is that they give you that context as well as all the other benefits associated with good quality unit tests.

Of course this doesn't help much if you're building an API that needs to be used by someone else.  Chances are you probably don't distribute your unit tests and even if you did, people still feel less comfortable reading test code than mainline code or JavaDoc. Having said this, it has always been my experience that no matter how good the documentation is it's usually almost impossible to work-out how a library should be used (especially one that is composed of [many small objects](http://www.robotwisdom.com/ai/thicketfaq.html)) unless you have concrete examples - code presented in context.

There are obviously many other factors that affect the readability of code and naming is but one however I do find all this particularly interesting as it challenges many of the assumptions, habits and conventions that I, and plenty of others, have formed over the years.

Even more interesting to me is that some approaches favour understanding by novices while others seem to benefit more experienced developers - what seems like the _simplest thing_&trade; to one person is completely unobvious to another.
