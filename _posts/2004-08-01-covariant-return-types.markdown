---
layout: post
title: "Covariant Return Types"
alias: /2004/08/covariant-return-types.html
categories:
---
Now for something completely geeky so as to fly directly in the face of my [previous post]({% post_url 2004-08-01-dont-just-think-feel-it %}), I quite literally just received notification that a [Java feature request](http://bugs.sun.com:80/bugdatabase/view_bug.do?bug_id=4144488) I had voted for (along with 861 others lol) has finally been closed off.

It may not seem like much but I've quite often felt the desire to further restrict the return type of an overidding method and soon, according to the release notes at least, I'll be able to do just that in the up and coming 1.5 ahem 5.0 release of J2SE.

A clear candidate for this is `clone()` meaning I will finally be able to do:

{% highlight java %}
public ThisClass clone() {
    return (ThisClass) super.clone();
}
{% endhighlight %}

No longer requiring callers to perform the cast themselves.

Of course removing redundant casts is but the simplest and most obvious use. Anyone who's ever been forced to code on top of a "generic framework" implementing/overriding `Object doStuff(Object) throws Exception` methods will know what I'm talking about :-).

I guess it comes down to your preference for static versus dynamic typing in many cases and while I'm not averse to a degree of dynamic typing, I do like it when my APIs can be more explicit without the need for all that useful&trade; JavaDoc.
