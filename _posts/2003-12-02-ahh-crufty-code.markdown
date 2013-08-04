---
layout: post
title: "Ahh crufty code"
alias: /2003/12/ahh-crufty-code.html
categories:
---
Inspired by [this blog entry](http://www.talios.com/read/373492.htm) I thought it was time to start blogging some of the crufty code that I come across from time to time and find highly amusing. Now having said that, let me say this: I've written a bunch of crufty code in my time (if we lived by my good friend [Jon Eaves](http://www.eaves.org/jon) _rules of life_, I'd have been removed from the gene pool long ago). I'd say probably every 12 months or so I look back on stuff I thought was great at the time and whince. But I have to admit that some of these are just way too funny not to air. Feel free to email me or comment with your own. So here goes...

These are just plain broken and wrong:

{% highlight java %}
String s = new String();
{% endhighlight %}

{% highlight java %}
String s = new String("");
{% endhighlight %}

{% highlight java %}
String s = new String().valueOf(someInt);
{% endhighlight %}

{% highlight java %}
try {...} catch (RuntimeException e) {
  e.printStackTrace();
}
{% endhighlight %}

{% highlight java %}
try {...} catch (Exception e) {
  if (e instanceof SomeException) {
    ...
  } else if (e instanceof AnotherException) {
    ...
  } else {
    throw e;
  }
}
{% endhighlight %}

{% highlight java %}
if (aBoolean == true) {
  return true;
} else {
  return false;
}
{% endhighlight %}

Next, I can only assume the developers thought that `finally` was only run after a `catch`?

{% highlight java %}
try {
  return someMethod();
} catch (...) {
  ...
} finally {
  return null;
}
{% endhighlight %}

Some lesser evils...

Instead of this:

{% highlight java %}
Object[] objects = someCollection.toArray();
for (int i = 0; i < objects.length; i++) {
  ...
}
{% endhighlight %}

try:

{% highlight java %}
for (Iterator iter = someCollection.iterator(); iter.hasNext(); ) {
  Object o = iter.next();
  ...
}
{% endhighlight %}

This is pretty anal but instead of this:

{% highlight java %}
return new Boolean(aBoolean);
{% endhighlight %}

use either of these two:

{% highlight java %}
return Boolean.valueOf(aBoolean);
return aBoolean ? Boolean.TRUE : Boolean.FALSE;
{% endhighlight %}
