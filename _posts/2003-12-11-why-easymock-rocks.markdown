---
layout: post
title: "Why EasyMock Rocks"
alias: /2003/12/why-easymock-rocks.html
categories:
---
My favourite [mock objects](http://c2.com/cgi/wiki?MockObject) library is [EasyMock](http://www.easymock.org). IMHO, if you need much else for mocking stuff on a new project at least, you're mocking (read testing) the wrong thing.

It's uses Javas built-in [dynamic proxies](http://java.sun.com/j2se/1.3/docs/guide/reflection/proxy.html), meaning you can mock out anything that is defined by an interface.

I was helping someone to write some tests recently and explining how to use the library when she noted that "it's like a macro recorder." I had not thought of it like this but I guess she's right.

The idea is you create the mock object and then call it in exactly the way you expect the class under test to call it. The cool thing about this is that you get to use the same interface as the class under test. You setup expected method parameters, return values and even exceptions. Then when you're done setting it up, you call the class under test and [EasyMock](http://www.easymock.org) handles the rest.

As an example, I thought I'd take the [clock code]({% post_url 2003-12-07-when-is-a-clock-not-a-clock %}) and write a test. (P.S. Anyone spot the "deliberate" mistake? [Perryn Fowler](http://www.jroller.com/page/perryn) did.)

{% highlight java %}
public void testThreadInterruptedWithTimeExpiredShouldNotSleep() {
  // Create the mock clockMockControl clockControl = MockControl.createStrictControl(Clock.class);
  Clock clock = (Clock) clockControl.getMock();

  // Create the mock taskMockControl taskControl = MockControl.createStrictControl(Runnable.class);
  Runnable task = (Runnable) taskControl.getMock();

  // Setup an alarm to go off at time=10
  Alarm alarm = new Alarm(task, 10, clock);

  // When run, the alarm should check the current time.
  // We'll tell it that the time=1clock.getCurrentTimeMillis();
  clockControl.setReturnValue(1);

  // This will cause the alarm to calculate the sleep time.
  // We pretend the thread was interrupted suffiently that the alarm
  // time has now expired, time=11, which should cause the task to be run
  // immediately.
  clock.getCurrentTimeMillis();
  clockControl.setReturnValue(11);

  // We want the task to be run once, and once only!
  task.run();

  // Tell the controls we're ready to rock'n'roll
  clockControl.replay();
  taskControl.replay();

  // And away we go...
  alarm.run();

  // Final check to ensure no expected methods are left outstanding
  clockControl.verify();
  taskControl.verify();}
{% endhighlight %}

Running this test should demonstrate the bug just nicely, resulting in the following exception (that'll teach you to write the code without even running it!):

{% highlight java %}
junit.framework.AssertionFailedError: Unexpected method call sleep(-1): sleep(-1): expected: 0, actual: 1
at org.easymock.internal.ObjectMethodsFilter.invoke(ObjectMethodsFilter.java:41)
at $Proxy0.sleep(Unknown Source)
at Alarm.waitForAlarmTime(Alarm.java:30)
...
{% endhighlight %}

How easy is that! You'll note that at least 50% of the "code" is actually comments I added for understanding, making it seem a lot longer than it really is.

Some traps for young players though:

* Don't forget to call `replay()` for each control prior to invoking the class under test. If you do forget, you'll end up `java.lang.IllegalStateException: missing behavior definition for last method call on the mock`;
* Don't forget to call `setReturnValue()` or `setThrowable()` on the control after calling a method on the mock object. Again you'll end up with `java.lang.IllegalStateException: missing behavior definition for last method call on the mock`; and;
* It's a good idea to call `verify()` on each control at the end of the test just to be sure.

About the only feature I can think of that I'd like added would be for a single control to handle multiple interfaces. That way I can have the ordering of calls across multiple interfaces checked as well. It _should_ be relatively simple as [dynamic proxies](http://java.sun.com/j2se/1.3/docs/guide/reflection/proxy.html) already support this. Without this feature, there's no real way to tell if the tasks `run()` method was called at the correct point in time. I draw the line at creating a dummy interface that extends both `Clock` and `Runnable` purely for testing ;-).
