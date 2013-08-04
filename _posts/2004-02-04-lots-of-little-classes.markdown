---
layout: post
title: "Lots of little classes"
alias: /2004/02/lots-of-little-classes.html
categories:
---
I remember having a heated discussion many years ago over the use of hungarian notation. Their argument went something like:

> ...If I don't call it `pLAmount`, how do I know it's a long?

To which I replied:

> You don't have to know if you create a `Money` class!

Whenever I see variables with type information embedded in their name or logically grouped by some prefix or suffix I immediately think there must be a missing class. An abstraction or simple concept that we haven't expressed in code yet.

As an example, we're in the process of building a booking system and we obviously needed a way to represent date ranges for various things such as, you guessed it, `Bookings`.

The first thing that emerged was a `Booking` containing a `fromDate` and `toDate`. This had two aspects (no, not AOP-speak) that annoyed me. One was the fact that the variable names all had suffixes of `Date`. The other, anytime we need to pass these values around, we need two, count 'em, 2 parameters for every date range!

Oh one more thing. I loathe and detest the `java.util.Date` classes.  Many people have commented on the problems with them so I'll say no more.

Instead we created a `TimePeriod` holding the start and end of the period represented as milliseconds GMT.

First the tests:

{% highlight java %}
public class TimePeriodTest extends TestCase {
  public TimePeriodTest(String name) {
    super(name);
  }

  public void testSameRangeOverlaps() {
    assertTrue(overlaps(1, 2, 1, 2));
  }

  public void testRangeBeforeDoesntOverlap() {
    assertFalse(overlaps(1, 2, 10, 11));
  }

  public void testRangeAfterDoesntOverlap() {
    assertFalse(overlaps(10, 11, 1,2));
  }

  public void testRangeInsideOverlaps() {
    assertTrue(overlaps(5, 6, 1, 10));
  }

  public void testRangeOverlapsAll() {
    assertTrue(overlaps(1, 10, 5, 6));
  }

  public void testRangeOverlapsStartOnly() {
    assertTrue(overlaps(1, 6, 4, 10));
  }

  public void testRangeOverlapsEndOnly() {
    assertTrue(overlaps(4, 10, 1, 6));
  }

  private boolean overlaps(long f1, long t1, long f2, long t2) {
    return new TimePeriod(f1, t1).overlaps(new TimePeriod(f2, t2));
  }
}
{% endhighlight %}

And here's the class itself:

{% highlight java %}
public final class TimePeriod implements Serializable {
  public static final long FOREVER = Long.MAX_VALUE;

  private final long _from;
  private final long _to;

  public TimePeriod(long from, long to) {
    Assert.isTrue(from <= to, "from can't be > to");
    _from = from;
    _to = to;
  }

  public boolean overlaps(TimePeriod other) {
    Assert.isTrue(other != null, "other can't be null");
    return _to >= other._from && _from <= other._to;
  }

  public int hashCode() {
    return (int) (_from ^ _to);
  }

  public boolean equals(Object object) {
    if (object == this) {
      return true;
    } else if (object == null || !getClass().equals(object.getClass())) {
      return false;
    }

    TimePeriod other = (TimePeriod) object;
    return _from == other._from && _to == other._to;
  }

  public String toString() {
    return getClass().getName() + '[' + from + '=' + _from + ", to=" + _to + ']';
  }
}
{% endhighlight %}

You'll note, there is no `getFrom()` nor `getTo()` method. Not because we won't eventually need them but because so far we don't need them for the user stories we have finished. More importantly, not having getters and setters forces us to think about classes in terms of behaviour not data. A [topic I've ranted on previously](/blog/2003/12/03/arent-classes-supposed-to-have-both-data-and-behaviour).

So, we implement the class and now a `Booking` has a `TimePeriod` during which it is active.

Searching for available resources becomes pretty easy. We simply search for resources with no booking that `overlaps` a nominated period.

Instead of placing a `getPeriod()` on `Booking`, we choose instead to add a `overlaps(TimePeriod)` method. Again, it's not because we think we won't need to eventually get/set the period but instead we eel it's more meaningful to add this kind of behaviour.

Let me re-iterate: Getters and setters are EVIL!

The next thing we needed to do was retrieve bookings. Aside from using a `referenceId`, the users want to search by `name` and `booking date`. The trouble is, sometimes they (the customers) don't remember the exact date:

> Um...it was sometime in january when I made the booking

says Mr. Smith. So how do we do this?

At first we though we might need some specialised class that magically knows to ignore days when comparing itself to a `TimePeriod`. It then struck us that we already had everything we needed.

Given our example, the month of January can be represented by a `TimePeriod` where the `from` is assigned `2004/01/01 00:00:00.000` and the `to` is assigned `2004/01/31 23:59:59.999`. This necessarily `overlaps` any `TimePeriod` that falls within or around January 2004.

So now we can search for any booking that `overlaps` the specified period.

_"Nothing staggering here"_ you might say. _"I've seen all this before"_ mutters the crowd. Well true. But it serves as the basis to illustrate something I feel is very important in software design.

_"Which would be?"_

Continually simplify your code by introducing abstractions whenever and wherever possible. Abstractions, often at what appear to be ludicrously fine levels of granularity, almost always lead to better quality code. It's much easier to remove an abstraction than to introduce one later on!
