---
layout: post
title: "Help save the object"
alias: /2003/12/help-save-the-object.html
categories:
---
I have an innate dislike of the so called _static helper_ class.

If you've not seen one of these, you are very lucky indeed! It's a class that contains entirely static methods that usually perform convenience functions such as data conversions, formatting, resource management, you name it.

If you've come from a C or VB background, it's probably a natural way to bring back that warm fuzzy feeling to the otherwise scary world of OO.

The problem is, static classes are difficult to decorate (yes, again, I know we now have AOP frameworks) and importantly, they're not pluggable (as in Strategy) which means you can't mock them out for testing!

It's not so much of a problem if the behaviour is trivial such as performing a simple calculation. But when it involves reading from a database, putting messages on queues, etc. the situation becomes much worse. You end up setting up, configuraing and ultimately testing a lot more than you had anticipated.

Quite often, the behaviour would be more appropriately placed in the class on which the processing is being performed instead of exposing the internals of the class just to perform a caclulation. If you do need to expose some of the internals, trun it around and use a Visitor. At the very least, turn the class into a Singleton and possibly even use a factory method of some kind. The java libraries have very few static classes. When they do (such as `java.util.Collections`) they're almost always some kind of factory.

So next time you're building a system, spare a few lines of code and help save the object. Better still, start practicing test-driven development and see how few of these things you end up with :-)

P.S. I've been accused of using too many acronyms before. This time I'm sure to be accused of using too many patterns. That's what patterns are for: communication. It saves me having to write a whole lot of code to explain what I mean. So get over it! :-)
