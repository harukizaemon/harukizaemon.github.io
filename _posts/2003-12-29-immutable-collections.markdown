---
layout: post
title: "Immutable Collections"
alias: /2003/12/immutable-collections.html
categories:
---
Oh what joy. Back on the last week of my project from a brief break for xmas and straight to the inevitable `cvs update`. Yikes!

This is probably old hat to a lot of you but for all others, `Collections.unmodifiableCollection()` and its siblings don't actually make a collection immutable!

By way of example, the following code is broken:

``` java
public class Component  {
  private final Collection _subComponents;

  public Component(Collection subComponents) {
    Assert.isTrue(subComponents != null, "subComponents can't be null!");
    _subComponents = Collections.unmodifiableCollection(subComponents);
  }

  public Collection getSubComponents() {
    return _subComponents;
  }
}
```

Looks pretty good but unfortunately, the following test breaks:

``` java
public void testImmutability() {
  Collection originalSubComponents = new HashSet();
  Component component = new Component(originalSubComponents);
  Collection returnedSubComponents = component.getSubComponents();
  assertNotNull(returnedSubComponents);

  // First, we test to see if the returned collection is immutable
  try {
    returnedSubComponents.add(new Object());
    fail("sub components should be unmodifiable");
  } catch (UnsupportedOperationException e) {
    // Pass
  }

  // Next, we test to see if we can subvert the immutability
  // by addding to the original collection
  assertTrue(returnedSubComponents.isEmpty());
  originalSubComponents.add(new Object());
  assertTrue("sub components should be a defensive copy", returnedSubComponents.isEmpty());
}
```

We haven't made a defensive copy! So let's add a little more code to get the test to pass:

``` java
public class Component  {
  private final Collection _subComponents;

  public Component(Collection subComponents) {
    Assert.isTrue(subComponents != null, "subComponents can't be null!");
    _subComponents = Collections.unmodifiableCollection(Arrays.asList(subComponents.toArray()));
  }

  public Collection getSubComponents() {
    return _subComponents;
  }
}
```

Remember, anytime a caller passes you a mutable object (such as `Collection`, `Date`, `Calendar`, etc.), you need to make a defensive copy if you're going to hold on to it. This will ensure no one accidentally subverts your own immutability.

Why did I use `Arrays.asList()` to create the copy? Well I'm glad you asked:

* Because I know the collection will never be modified (that's what immutable means after all), I don't need to maintain the semantics of the original collection (which may have been a `Set` for example);
* The resulting `List` will be have been created with exactly the right size to accomodate the contents of the collection, meaning no re-allocating buffers. As a reader points out the standard `ArrayList` constructor that takes a collection as an argument will allocate an additional 10%;
* `Arrays` `List` implementation performs well when iterating; and;
* The iteration order is preserved (if that was important).

It is also possible to use `new ArrayList(int)` followed by `ArrayList.addAll(Collection)`, which is probably easier to read?:

``` java
public class Component  {
  private final Collection _subComponents;

  public Component(Collection subComponents) {
    Assert.isTrue(subComponents != null, "subComponents can't be null!");
    List temp = new ArrayList(subComponents.size());
    temp.addAll(subComponents);
    _subComponents = Collections.unmodifiableCollection(temp);
  }

  public Collection getSubComponents() {
    return _subComponents;
  }
}
```

As one reader has demonstrated, on his version of the JDK it uses an iterator which is the way I had assumed it would be implemented but I was thrown off when I looked at the source code for the version of the JDK I have and discovered that it creates a temporary array. Either way this post wasn't really about performance so I guess I'm getting a little off track now.

P.S. Anyone see the funny side of this test given my [previous post](/blog/2003/12/29/be-gone-ye-foul-smelling-custom-type)?
