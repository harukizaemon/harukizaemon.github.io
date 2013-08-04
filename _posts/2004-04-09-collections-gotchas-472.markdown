---
layout: post
title: "Collections gotchas #472"
alias: /2004/04/collections-gotchas-472.html
categories:
---
I found an interesting (to me anyway) implementation detail in Suns JDK `LinkedList` and `HashSet` code.

Compare these two. The first is deep in the bowels of `LinkedList.indexOf(Object)` which is used by the implementation of `contains(Object)`:

``` java
if (o.equals(e.element))
    return index;
```

The next is from `HashMap.containsKey(Object)` which is used by `HashSet` to implement `contains(Object)`:

``` java
return x == y || x.equals(y);
```

Notice anything interesting? No?

Well I know it's not much but `HashSet` which one would _think_ probably only honours the `equals(Object)code+`hashCode()` contract, also checks for object identity. On the other hand, `LinkedList`, which I would have thought probably only checks for object identity, actually only honours the `equals(Object)code+`hashCode()` contract.

Why is this a problem? Well I found a class (in a 3rd-party library) that never returned `true` for `equals(Object)`. I only found it because I was writing a test and wondered why when I added instances of the class to a `LinkedList`, `contains(Object)` always returned `false` even though when adding them to a `HashSet`, `contains(object)` always returned `true`.

I had expected `List` implementations to at least check object identity. Unfortunately it behaves exactly as the documented, says so I'll just have to deal with the real problem and shoot the developer who wrote the broken code instead. But saying so upfront would have spoiled a good rant :-)

And the moral of the story is? Tests are good; Assumptions are bad; Test your assumptions; and; Sometimes the documentation _is_ worth reading but usually only after the fact when all the subtleties hidden therein become apparent.
