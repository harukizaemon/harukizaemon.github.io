---
layout: post
title: "When is a clock not a clock?"
alias: ["/2003/12/when-is-clock-not-clock.html", "/blog/2003/12/07/when-is-clock-not-clock"]
categories:
---
When it's a mock.

It's a common problem. You need to test behaviour of an application based on the system clock. Say some event that is triggered every n minutes, or at a certain time, etc.

Allow me to go nuts with some code examples for a change :-)

The most common solution to the problem is to do something like this:

{% highlight java %}
public class Task implements Runnable {
    private final long _alarmTimeMillis;

    public Task(long alarmTimeMillis) {
        _alarmTimeMillis = alarmTimeMillis;
    }

    public void run() {
        waitForAlarmTime();
        doSomething();
    }

    private void waitForAlarmTime() {
        while (System.currentTimeMillis() < _alarmTimeMillis) {
            try {
                Thread.sleep(_alarmTimeMillis - System.currentTimeMillis());
            } catch (InterruptedException e) {
                // Ignore it
            }
        }
    }

    private void doSomething() {...}
}
{% endhighlight %}

This raises two questions in my mind: Firstly, should the action taken by the task really depend this closely on the scheduling? And secondly, how are we going to test it?

Let's deal with the easy problem first. Let's create a class called say, `Alarm` that looks something like this:

{% highlight java %}
public class Alarm implements Runnable {
    private final Runnable _task;
    private final long _alarmTimeMillis;

    public Alarm(Runnable task, long alarmTimeMillis) {
        if (task == null) {
            throw new IllegalArgumentException("task can't be null");
        }
        _task = task;
        _alarmTimeMillis = alarmTimeMillis;
    }

    public void run() {
        waitForAlarmTime();
        _task.run();
    }

    private void waitForAlarmTime() {
        while (System.currentTimeMillis() < _alarmTimeMillis) {
            try {
                Thread.sleep(_alarmTimeMillis - System.currentTimeMillis());
            } catch (InterruptedException e) {
                // Ignore it
            }
        }
    }
}
{% endhighlight %}

Now this doesn't look much different really except for one very important thing: We've decouple the scheduling from the action. `Alarm`  now depends on a `Runnable` (an interface) to actually get the work done. That means `Alarm` is responsible for one thing only. Namely, the scheduling of a task. This allows us to independently test the schduling component and the task itself.

Because Runnable is an interface, we can easily code up a test that creates an implementation of `Runnable` purely for testing. Ie. a mock object. And because the tasks are now independent, we can test their behaviour irrespective of when they are likely to be scheduled.

So far so good. But how does this all relate to the original title? Well when we go to code up our `AlarmTest` we encounter some grief due to the fact that `Alarm` is coupled to `System.currentTimeMillis()`, a static method over which we have no control and also to `Thread.sleep()` another static method over which we have no control!

The solution however is relatively simple: Let's [help save the object](/blog/2003/12/05/help-save-the-object) by introducing our own abstraction of the system clock.

We'll make an interface and call it, strangely enough, `Clock`:

{% highlight java %}
public interface Clock {
    public long getCurrentTimeMillis();

    public void sleep(long millis) throws InterruptedException;
}
{% endhighlight %}

Now we can re-write our `Alarm` like this:

{% highlight java %}
public class Alarm implements Runnable {
    private final Runnable _task;
    private final long _alarmTimeMillis;
    private final Clock _clock;
    public Alarm(Runnable task, long alarmTimeMillis, Clock clock) {
        if (task == null) {
            throw new IllegalArgumentException("task can't be null");
        }
        _task = task;
        _alarmTimeMillis = alarmTimeMillis;
        if (clock == null) {
            throw new IllegalArgumentException("clock can't be null");
        }_clock = clock;
    }

    public void run() {
        waitForAlarmTime();
        _task.run();
    }

    private void waitForAlarmTime() {
        while (_clock.getCurrentTimeMillis() < _alarmTimeMillis) {
            try {
                _clock.sleep(_alarmTimeMillis - _clock.getCurrentTimeMillis());
            } catch (InterruptedException e) {
                // Ignore it
            }
        }
    }
}
{% endhighlight %}

This allows us to implement a `SystemClock` that simply delegates to the original methods we called. But importantly, we can also implement our own `MockClock` that returns whatever values we like for `getCurrentTimeMillis()` and does whatever we like (most likely nothing) for `sleep()`.

Not only does this make it easier to test, but it also means we don't actually have to have our test wait for 3 hours just to see what happens. We can simply have our `MockClock` pretend to sleep for 3 hours.

We haven't tried to make our own `Thread` or subvert the `System` class or clock in anyway. We've taken control and not allowed ourselves to be dictated to by someone elses API.

There's still obviously some room for improvement here. We could make the `Clock` API possibly a little higher level, maybe? We could provide a `ClockFactory` if you don't like the idea of passing it into the constructor (ala [IoC](/blog/2003/12/15/inversion-of-control)). But you hopefully get the gist.

Of course had we done this test-first, we would likely have arrived at this solution in the first place. But it does serve to illustrate the point that, IMHO, testability necessitates good OO design.
