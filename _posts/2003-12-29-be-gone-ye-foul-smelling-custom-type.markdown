---
layout: post
title: "Be gone ye foul smelling custom type-safe collections!"
alias: /2003/12/be-gone-ye-foul-smelling-custom-type.html
categories:
---
I used to use STL type-safe collections in C++ all the time and when I moved to using generic collections in Java something just felt wrong. How was I to ensure my collections contained the correct types?

But you know what? Much to my surprise over the years, I've never really had a problem. I can't even remember the last time (if ever) I had a class-cast exception from accidentally adding the wrong type of object to a collection.

Which one of these would you rather?

The generic collections approach:

{% highlight java %}
Collection customers = ...;
for (Iterator i = customers.iterator(); i.hasNext(); ) {
  doSomething((Customer) i.next());
}
{% endhighlight %}

Versus the custom type-safe collection:

{% highlight java %}
public class CustomerCollection extends AbstractCollection {
  private final Collection _customers = new HashSet();
  public boolean add(Object object) {
    return addCustomer((Customer) object);
  }

  public boolean addCustomer(Customer customer) {
    return _customers.add(customer);
  }

  public int size() {
    return _customers.size();
  }

  public Iterator iterator() {
    return customerIterator();
  }

  public CustomerIterator customerIterator() {
    return new CustomerIterator(_customers.iterator());
  }

  public final class CustomerIterator implements Iterator {
    private final Iterator _iterator;
    private CustomerIterator(Iterator iterator) {
      _iterator = iterator;
    }

    public void remove() {
      _iterator.remove();
    }

    public boolean hasNext() {
      return _iterator.hasNext();
    }

    public Object next() {
      return nextCustomer();
    }

    public Customer nextCustomer() {
      return (Customer) _iterator.next();
    }
  }
}
{% endhighlight %}

{% highlight java %}
CustomerCollection customers = ...;
for (CustomerIterator i = customers.customerIterator(); i.hasNext(); ) {
  doSomething(i.nextCustomer());
}
{% endhighlight %}

Not so bad you say? Well try doing this for every type for which you need to hold collections. Then try implement `List` or `Set` instead of the basic `Collection`. Don't even think about what it takes to implement `Map`. I admit, you might end up re-factoring some of this into a generic base class. You could even write one without implementing any of the collection interfaces. But then, why bother?

Sure, they might have save me a few keystrokes here and there but because most people (as I have done in my example) create their type-safe collections with names like `getCustomer()`, etc. it saves me 2 key-strokes over using a simple cast like `(Customer)`. In fact, in the example given, I actually ended up with more key-strokes!

_"But it's not about keystrokes, it's about type-safety"_ I hear you exclaim. Well yes, you may be right. But as I mentioned earlier, I don't recall this ever being a problem on any projects I've worked on. Oh, how that I think about it, there may have been one project but I'm pretty sure we simply shot the offending developer for being such an imbecile ;-)

I'm currently enduring the pain of working on a project where the consensus was that we should have type-safe collections. I seem to spend half my day implementing these instead of letting IntelliJ and Eclipse do a fine job of adding the casts in for me.

And so it is that I eagerly await the release of JDK 1.5 and language supported type-safe collections in the hope that it'll once and for all stop people writing these useless custom implementations. Egads!
