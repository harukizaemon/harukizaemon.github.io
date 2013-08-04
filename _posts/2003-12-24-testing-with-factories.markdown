---
layout: post
title: "Testing with factories"
alias: /2003/12/testing-with-factories.html
categories:
---
A long time colleage of mine asked me yesterday about [IoC](/blog/2003/12/15/inversion-of-control). After explaining about constructors, etc. we then discussed factories. He understood the way you would pass a single instance of say a widget to an object via the constructor, but what about something more complex that doesn't know which widget it wants or needs multiple widgets. Enter the factory.

Factories are really no different from other java objects that require some kind of lookup mechanism to find such as, JDBC connections, JNDI contexts, etc. One of the most common ways to implement factories to facilitate runtime lookup is the so called abstract static factory "pattern". (I used the term pattern simply because others have. In general I find most people abuse the use of the word, but that's a rant for another time).

For those not familiar with abstract static factory, the general idea is you create an abstract base class with a static method, say `newInstance()` or `getInstance()` for example, that returns the concrete implementation of the factory and an abstract method, say `createWidget()` that the concrete class implements. Typically, the concrete class is determined at runtime by using something as simple as a system property or something a little more sophisticated (ala `META-INF/services`). If you've ever used JAXP, you've been using one without possibly realising it.

``` java
public abstract class WidgetFactory {
  public static final WidgetFactory newInstance() {
    try {
      return Class.forName(System.getProperty(WidgetFactory.class.getName()).newInstance();
    } catch (...) {
      throw new IllegalStateException("Can't instantiate concrete factory: " + e.getMessage());
    }
  }

  public abstract Widget createWidget();
}
```

``` java
public final class WidgetFactoryImpl extends WidgetFactory {
  public Widget createWidget() {
    return new WidgetImpl();
  }
}
```

``` java
public final class Window {
  public Window() {
    add(WidgetFactory.newInstance().createWidget());
    ...
  }
  ...
}
```

I will usually have to set a system property to tell the abstract factory which concrete implementation to use. Apart from the fact that this destroys my ability to run multiple tests in parallel (system properties are global!), you could also think of it as a kind of violation of encapsulation - my test class has to know how the abstract factory is implemented so that I can tell it to return my mock factory instead.

``` java
public void testSomething() {
  System.setProperty(WidgetFactory.class.getName(), MockWidgetFactory.class.getName());
  Window window = new Window();
  ...
}
```

So anyway, given my love of TDD which seems to lead me to pass implementations of interfaces into constructors (ala IoC), I have a [dislike of most things static](/blog/2003/12/05/help-save-the-object). Even the abstract static factory.

Instead, I prefer to have the factory defined by an interface. Then pass an instance of the interface to the class that depends on the factory (ie no more `newInstance()`). In my test this can be a mock implementation, and at runtime this can be configured in a number of ways to pass a real implementation.

``` java
public interface WidgetFactory {
  public Widget createWidget();
}
```

``` java
public final class WidgetFactoryImpl implements WidgetFactory {
  public Widget createWidget() {
    return new WidgetImpl();
  }
}
```

``` java
public final class Window {
  public Window(WidgetFactory factory) {
    add(factory.createWidget());
    ...
  }
  ...
}
```

``` java
public void testSomething() {
  Window window = new Window(new MockWidgetFactory());
  ...
}
```

Sometimes this is difficult to achieve, especially when you have to use a 3rd-party API that only has an abstract static factory (such as JAXP). In this case, you really have a few possibilities:

* Call `newInstance()` externally and pass the resulting concrete factory into the constructor. At least this way, you should be able to implement some kind of mock implementation, even if that means extending the abstract factory;
* In the case of sframeworks that force you to have a default constructor, consider a hybrid where your objects have both a constructor that accepts an instance of a factory for your own use, and a default constructor that falls back on some kind of lookup mechanism (such as abstract static factory) and simply passes the factory instance to the other constructor; or
* In the case of struts, as James Ross has pointed out, you can hang the factories out of the session or the application context and let struts set them for you; or lastly;
* Continue using the factory ASIS from within the class. In the case of JAXP I can't see a big problem with this (one hopes that it is already fully tested!);

Naturally, I have a preference for the first approach if I can get away with it as it makes my life much simpler when it comes time to test and debug. No more flipping system properties, tests that can be run independently and in parallel, etc.

I can think of reasons for using the last method that are mainly to do with resource issues like "but I don't want an instance of the factory lying around if I don't need it. If that really is going to cause you resource issues (which I very much doubt), then at least wrap the factory in a class that lazily creates a factory for you.
