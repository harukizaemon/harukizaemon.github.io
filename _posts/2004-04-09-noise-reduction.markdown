---
layout: post
title: "Noise Reduction"
alias: /2004/04/noise-reduction.html
categories:
---
An amusing topic coming from one such as myself that can quite clearly never shut up!

I've been bitten a few times this week by a low signal/noise (or a high noise/signal which ever you prefer) ratio. The first was earlier in the week when I was supposed to give a talk (more noise) and demo at the monthly [MXPEG](http://groups.yahoo.com/group/melbourne_XP_enthusiasts/) meeting.

Like most geeks, I'm continually tweaking my system. And as most geeks know, tweaking is really a euphemism for breaking something. I'm constantly updating the patches on my system and installing the latest version of this and that. Each time I perform the awesome [Gentoo](http://www.gentoo.org) `emerge -uD world` it spews forth volumes of logging. So much logging that it's not only hard to see if there are any messages worth reading but they fly past so quickly I probably couldn't read them even if I tried. So after a very short time one starts to ignore pretty much all of them. Hey if it doesn't complain at the end then it's probably ok. Right?

So there I am at the meeting trying to get my machine to connect to the network and failing dismally. I gave up and vowed to track it down when I got home. Eventually I discovered that there were some config files that needed updating and the messages were there for all the world to see saying just that. But because I had just ignored them all I never noticed.

Similarly, we had a bunch of checks on our code base that were flagged as warnings not errors. At some point, that list of warnings got so big we all just started to ignore them. Then one-day I accidentally checked in a copy of the build file that stopped breaking the build on errors (over zelaous find and replace) . Nobody noticed. Why? Because everyone had just grown accustomed to ignoring the output because it contained so much noise. The answer was of course to either remove the warnings as we clearly didn't care about them, or turn them into errors so we train people to look at all messages.

[James](http://www.redhillconsulting.com.au/blogs/james) and I worked on a project where there was so much logging going on, nobody noticed all the stack traces and NPEs etc. being spewed into the server logs. In fact there were so many errors that they turned off the "page me if there is an error" handling. This of course just made the problem worse.

JavaDoc comments are another pet hate of mine. We did a refactor the other day that involved a rename of a class and the subsequent getters and setters on the domain object and process beans, etc. We ended up spending so much time updating the comments from "`Sets the value of the product details`" to "`Sets the value of the vehicle details`" that I wanted to murder someone. JavaDoc comments convey little or no more than the method signature _should_ convey. I emphasise SHOULD for good reason. Sure, there are some special cases such as "`will return the default if no explicit value has been set`" for example but if you feel you need to explain what `getCustomer` does, then throw a `GetAGripException` or find a new profession.

All this stuff is noise. The problem really is that you have been allowed or have chosen to ignore it. You can't afford to. Don't just ignore it or it will come back to bite you later on. Of course, just saying _"well we ignore it so maybe we can just stop doing it"_ isn't the answer either. Maybe it's there for good reason. Find out why it's there and stop needing to ignore it before it starts to take it's toll on yours and the rest of the teams poductivity.

P.S. Can anyone tell I'm back on a project again? LOL
