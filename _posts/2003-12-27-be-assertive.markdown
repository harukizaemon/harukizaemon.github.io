---
layout: post
title: "Be Assertive"
alias: /2003/12/be-assertive.html
categories:
---
I recently discovered a bug in an Ant task I had written a while back. A customer was running the code and it barfed with an `IllegalStateException` telling them proudly that the code"formatterType cannot be null"code. Nothing special about that I suppose. A NPE would have done much the same thing. The only difference is that the code is obfuscated. That means the stack trace is totally useless. Not only that, but they were running an older version of the software which would have meant I would have had to check out that particlar version from CVS, build it and see if I could work out what was going on.

As it turned out, There were only two places in the code that performed a null pointer check with this particular message and only one of them is called from the Ant task. A bit of detective work later (like 5 mins tops) and I had a failing unit test written and running. The fix was one line. Actually half a line as it required assigning a default value to an instance variable in the declaration :-).

I program defensively. I check parameters for null pointers, I assert that objects are in the correct state at the start of the method etc. I've found I catch strange bugs much, much earlier than if I simply wait for the NPE. I also have a preference for immutable objects, which substantially reduces the set of things I need to check for. I've had countless debates with, mostly junior, developers who believe that it's better just to wait for the NPE. But fundamentally, I put assertions into my code so that out in the field my software fails in predictable ways. Ways that will ultimately help me to identify the nature and cause of a problem as easily as possible.

I even have an IntelliJ macro setup so that it's as painless as possible to add a check to my code. And just to be sure, James Ross and I even wrote a custom [checkstyle](http://checkstyle.sf.net) check to ensure that an object reference is always checked for null before being used.

There are also a number of libraries around such as [iContract](http://www.reliable-systems.com/tools/iContract/iContract.htm) and [jContractor](http://jcontractor.sourceforge.net/), that go way beyond simple assertions and allow you to add Design by Contract conditions to your code via javadoc comments and byte-code modification, special methods, macros that a pre-processor inlines, etc. Though I've never used them, I'd be interested in comments from anyone that has.

The thing is, I've never used the `assert` statement in java. That doesn't mean I don't believe in assertions, clearly. I've just never used the built-in support. I've never liked the fact that you can turn them off. To me, this defeats the reason for having them there in the first place. In fact the JDK assertions (at least in 1.4) have to be explicitly turned on. I'm not so much interested in catching NPE's etc in development as preventing really bad things happing in production so to me, it seems rather pointless to turn then off.

Instead, I use a custom `Assert` class that works in any JDK and can't be turned off.

{% highlight java %}
public final class Assert {
    private Assert() {
        throw new UnsupportedOperationException("Constructor should not be called");
    }

    public static void isTrue(boolean condition, String message) throws IllegalStateException {
        if (!condition) {
            fail(message);
        }
    }

    public static void fail(String message) throws IllegalStateException {
        throw new IllegalStateException(message);
    }
}
{% endhighlight %}

(This is one of those times where I feel it's just fine to have a bunch of static methods. Imagine butchering your code to have an `Assert` object passed into the constructor of every class I ever created? YUK! ;-) All you AOP weenies get back in your box before things get ugly!)

I also noticed [commons-lang](http://jakarta.apache.org/commons/lang.html) has a [Validate](http://jakarta.apache.org/commons/lang/api/org/apache/commons/lang/Validate.html) class that provides the same functionality (and then some). Though, again, I've never used it myself.

I can think of one possible reason why I might want to turn off not only an `AssertionError` being thrown but the condition check itself. If you're using asserts for checks such as "when the method ends, there should be at most 2 address records for the customer". This may well be an expensive operation that you don't want performed everytime in production but I don't tend to check these sorts of things. Now I'm thinking maybe I should? Or is that what my unit tests are for? But then usng `asserts`, I _could_ turn them on in production if I needed to catch some bug I was having trouble replicating. Having said that of course, if I had a sufficient test suite, I bet I could find the bug just as easily without resorting to debugging in production. Hmmmm
