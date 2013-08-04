---
layout: post
title: "Adapter classes"
alias: /2003/12/adapter-classes.html
categories:
---
Since I ranted about [abstract classes]({% post_url 2003-11-01-abstract-classes-are-not-types %}) a couple of days ago I've read quite a number of other blogs that discuss interfaces versus abstract classes. Most of the discussion seems to be around the fact that adding a method to an interface is a pain because you need to add that method to any class that implements the interface. The solution in some cases (but definitely not all) is to have an "adapter" class that provides empty implementations of all methods. You can then extend this and override only the methods you need. For example:

{% highlight java %}
public interface Foo {
    public void somethingHappened();
}

public class FooAdapter implements Foo {
    public void somethingHappened() { ... }
}
{% endhighlight %}

This can make a lot of sense if you're implementing some kind of listener/observer. But if the interface is required to return something as in:

{% highlight java %}
public interface Foo {
    public int getSomething();
}
{% endhighlight %}

then what do we do? Return `0` for integers, `null` or `""` for strings or `throw UnsupportedOperationException`?

Interestingly, if you don't recompile the implementing class and something attempts to call a method on the interface that you haven't implemented, the JVM will throw a `NoSuchMethodError`.

The one place where it might bite significantly is if/when you change a public interface in a public API. Once that API is published and used by third-parties, it becomes much harder to change for fear of breaking backwards-compatibility.

At any rate, these "adapter" classes aren't really abstract classes nor are they types. They are once again a convenience of the programming language. Ruby and smalltalk for example will, like the JVM, throw an exception if you attempt to call an unimplemented method on a class. The difference being that Java's strong typing forces you to deal with it up front.

So, putting pandora firmly back in her box, I don't see that any of this really goes against my original proposition that [abstract classes are not in fact types]({% post_url 2003-11-01-abstract-classes-are-not-types %}).
