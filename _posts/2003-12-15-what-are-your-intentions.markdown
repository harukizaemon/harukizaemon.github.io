---
layout: post
title: "What are your intentions?"
alias: /2003/12/what-are-your-intentions.html
categories:
---
Allow me to vent for just a minute. I promise it won't take long...

In an effort to improve the amount of Javadoc in the source base, the current project I'm working on has been set up (not by me I might add) to use [checkstyle](http://checkstyle.sf.net) to enforce Javadoc standards.

Following on from [why javabeans are evil](/blog/2003/12/03/arent-classes-supposed-to-have-both-data-and-behaviour) I have to say that enforcing Javadoc on getters and setters (that's accessors and mutators for all you Java Certified Developers out there :-P) is one of the most pointless things I've ever seen.

It takes me about 10 seconds to use IntelliJ (or eclipse for that matter) to generate all the getters and setters for the attributes on my DTOs. It then takes me ten minutes to either write the JavaDoc by hand. So I try using the auto generated javadoc only to find that it takes almost as long to change all the generated `Obtains the value of paymentDate` to `Obtains the value of the payment date`.

In fact it occurs to me that if my IDE can work out what to generate as javadoc, surely I can work out what the code is doing simply by looking at it?

I've always been a fan of commenting code. I guess it comes from my assembler programming days. I must admit I write fewer and fewer comments these days as my programming style has changed to be more "self-documenting". No matter what though, if you are going to write comments, it's important that you document the intent and not the implementation. How many times have I seen this:

{% highlight java %}
++i;                          // Increment the current index
collection.add(values[i]);    // Add the value at the current index to the collection
{% endhighlight %}

This serves no purpose other than to satisfy the good-old comment/source-code ratio checker :-). Even worse, it adds nothing to my understanding of the code (ie intent) and IMHO adds significantly to the signal/noise ratio.

Most javadoc I see follows along similar lines. When 50% of the code is comments, kindly informing me that the `getCustomer()` method `Gets the customer`, etc. I can't help but wonder what the hell we've been spending our time doing.

What's worse, as with most kinds of heavy documentation, the comments rapidly become out of date as we constantly refactor the code, rename classes, attributes, etc.

Hmmm...I'm no language expert (as is evidenced by my ramblings on this site) but I wonder if it's possible to analyse the comments and determine if they're  documenting intent or implementation detail?

In any event, as a start, do me a favour and don't enforce Javadoc on getters and setters. I'll meet you half way and try to get used to [putting opening braces on a new line](/blog/2003/12/14/dont-blame-the-brace) :-)
