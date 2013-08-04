---
layout: post
title: "Two Tigers For Two Days In Two Minutes"
alias: /2005/04/two-tigers-for-two-days-in-two-minutes.html
categories:
---
I've been playing with Tiger (OS X) and Tiger (JDK 1.5) for almost 2 days now and I like it. Spotlight rocks. Dashboard widgets are neat and finally I get to do some stuff with Java 5! So here are some some observations and a few gotchas I've discovered so far.

First up I did an archive and install so I admit I didn't start with a totally clean slate.

Overall OS X 10.4 seems to be a little slower and a little buggier than 10.3. Safari hangs or crashes on me every now and then and the little colour wheel is becoming an all too frequent experience. Again this may be due to the fact that I did an "upgrade" rather than a fresh install. A friend of mine did suggest that perhaps spotlights indexing was chewing a bit of CPU from time-to-time. Not sure. I'll have to wait and see.

The RSS reader in Safari is beautiful though rather less than I had hoped for. Seriously, it would have made much more sense to build RSS support into the mail client than the web browser. There doesn't appear to be any way to see at a glance, which feeds have new articles. The only way seems to be to "show all" in which case my news feeds totally swamp any others that I might be interested in. Maybe it's just what I'm used to so, we'll see how it goes.

Speaking of mail clients, Mail 2 as it's now called has had a new face-lift and I can't say I'm that impressed. I kinda liked the old L&F. They've taken away my Outbox and status bar. I had found both of these particularly useful when accidentally pressing send before I had completed an email. Again, maybe I'm just a doofus but I can't see anyway to turn these back on. Guess I'm going to have to be more careful when I compose my emails from now on.

Spotlight (Apple+Space) is pretty damn cool even if it is probably little more than a Google Desktop but I like the fact that it's built in, let's me search inside files as well as through file names and it's fast enough (for me) that I've stopped using [Quicksilver](http://quicksilver.blacktree.com). Spotlight categorises files that it finds; one of those categories being applications. It has integration with mail, safari, address book, etc. so it's able to search all your emails, bookmarks, contacts, you name it. This is not to say that spotlight can really compete in any way with Quicksilver, it can't. But I'm no power user and it was really only an application launher for me so I can probably live without it, maybe.

No matter what your feelings on the OS are, if you want to play with JDK 1.5, then you have little choice but to switch over - Apple aren't going to ship a version for 10.3.9 or below. I [downloaded](http://www.apple.com/support/downloads/java2se50release1.html) and installed JDK 1.5 and followed the [instructions](http://docs.info.apple.com/article.html?artnum=301073) on how to make it the default runtime for applications. This meant IntelliJ now starts up using Java 5 runtime but running `java` from the command-line still brings up JDK 1.4.2. The trick it seems is to play with some symbolic links in the bowels of the totally non-standard distribution that apple concocted for reasons known only to a select few I'm sure. So, to get JDK 1.5 from the command-line, I found I needed to do the following:

``` console
> cd /System/Library/Frameworks/JavaVM.framework/Versions/sudo rm CurrentJDKsudo ln -s 1.5 CurrentJDK
```

On that note, the latest [EAP](http://www.intellij.net.eap) of IntelliJ seems to be a lot more stable. I'm not sure if it's the EAP, JDK 1.5, OS X 10.4 or a combination but whatever it is, I'm grateful as I was getting sick of the weirdness with dialogs losing focus and disappearing on me all the time.

The dashboard (F12 by default) is rather like [Konfabulator](http://www.konfabulator.com/) only it resides on a transluscent pane that appears on top of all other applications. I've been using the Weather, Calculator, Translator and Dictionary widgets quite a lot. Unfortunately the YellowPages widget is for US numbers only. Ooh ooh, the Flight Tracker is very cool! Not sure how often I'll _actually_ use it but it is pretty neat.

I also use the World Clock. Like all widgets, you can have as many open as you like on the dashboard. I currently have only three open: one each for London, Calgary and Bangalore :X. Two widgets I'd like are one for monitoring my email and another for tailing a file such as the system log. Anyone?

My next little hack is to do with the Weather widget. It seems that when creating the widget, the developers decided that evey user would of course be residing in the US; try putting in `Melbourne` for the city. I had no idea there was a Melbourne in Florida! But after a little sniffing around the [AccuWeather](http://www.accuweather.com) website I discovered that they have a "special" syntax for describing "foreign" cities: `CountryCode;StateCode;CityName` which in the case of Melbourne, Victoria, Australia, translates into `AU;VT;MELBOURNE`. So, I thought I'd try putting that into the weather widget and bingo! I now have weather for the "real" Melbourne ;-)

I use iSync with my [Motorola V3](http://au.motorola.com/pcs/v3/) Phone and my iPod and the new version definitely works better than the old, not to mention synchronising more of my contact details.

And that's about it I think. Oh, the only other observation I'd make is that I do get the impression that the battery-life is a tad longer. That seems strange to me and maybe it's just hope played out as reality but anyway, I'll be curious to see how long that impression lasts ;-)

So, was it worth the price of admission? I'm yet to be convinced. It's all fun geeky stuff but not much substance really when it comes down to it. Certainly most of the new features I could have had already simply by using more mature 3rd-party tools but hey, I'm a technophile so, as much as I'll berate people for wanting to play with cool toys at work, playing with them in the privacy of your own living room is an entirely different story ;-)
