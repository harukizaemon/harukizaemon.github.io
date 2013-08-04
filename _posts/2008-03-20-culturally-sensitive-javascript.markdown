---
layout: post
title: "Culturally Sensitive JavaScript"
alias: /2008/03/culturally-sensitive-javascript.html
categories:
---
JavaScript is a fantastic little language and with the likes of [Prototype](http://www.prototypejs.org/), [Scriptaculous](http://script.aculo.us/), and my newest favourite, [lowpro](http://lowpro.stikipad.com/), you can build some quite frankly, remarkable web applications.

One area where most web browsers fall down however is in their error-reporting, or lack thereof. A fact that has caused me to waste seemingly countless hours trying to find the source of some problem or other only to realise that a typo that had been staring me in the face the entire time was to blame!

Now, like just about any programming library I use these days, most JavaScript libraries use American english. `initiali**z**e`, `capitali**z**e`, you know what I'm talking about.

For the most part the use of 'z' instead of 's' isn't too much of a problem but just recently I consistently tried to use lowpro's `addBehaviour` method, only there isn't one. It's called `addBehavior ` (sans 'u').

So today after about 20 minutes cursing and swearing at the spelling [Steve](http://steve.cogentconsulting.com.au/) asked "is there anyway you could create an alias?" Being JavaScript the answer is of course "abso-bloody-lutely!":

``` javascript
Event.addBehaviour = Event.addBehavior;
```

You can alias just about anything this way.

No more will my code silently fail due to differences in spelling :)
