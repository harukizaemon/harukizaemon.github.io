---
layout: post
title: "Ahh crufty code"
alias: /2003/12/ahh-crufty-code.html
categories:
---
Inspired by [this blog entry](http://www.talios.com/read/373492.htm) I thought it was time to start blogging some of the crufty code that I come across from time to time and find highly amusing. Now having said that, let me say this: I've written a bunch of crufty code in my time (if we lived by my good friend [Jon Eaves](http://www.eaves.org/jon) _rules of life_, I'd have been removed from the gene pool long ago). I'd say probably every 12 months or so I look back on stuff I thought was great at the time and whince. But I have to admit that some of these are just way too funny not to air. Feel free to email me or comment with your own. So here goes...

These are just plain broken and wrong:

``` java
String s = new String();
```

``` java
String s = new String("");
```

``` java
String s = new String().valueOf(someInt);
```

``` java
try {...} catch (RuntimeException e) {
  e.printStackTrace();
}
```

``` java
try {...} catch (Exception e) {
  if (e instanceof SomeException) {
    ...
  } else if (e instanceof AnotherException) {
    ...
  } else {
    throw e;
  }
}
```

``` java
if (aBoolean == true) {
  return true;
} else {
  return false;
}
```

Next, I can only assume the developers thought that `finally` was only run after a `catch`?

``` java
try {
  return someMethod();
} catch (...) {
  ...
} finally {
  return null;
}
```

Some lesser evils...

Instead of this:

``` java
Object[] objects = someCollection.toArray();
for (int i = 0; i < objects.length; i++) {
  ...
}
```

try:

``` java
for (Iterator iter = someCollection.iterator(); iter.hasNext(); ) {
  Object o = iter.next();
  ...
}
```

This is pretty anal but instead of this:

``` java
return new Boolean(aBoolean);
```

use either of these two:

``` java
return Boolean.valueOf(aBoolean);
return aBoolean ? Boolean.TRUE : Boolean.FALSE;
```
