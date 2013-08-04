---
layout: post
title: "McAppy Time"
alias: /2004/11/mcappy-time.html
categories:
---
Something appeals to me about the way Mac OS X applications are distributed. Sometimes there is an installer (boo!) but more often than not there is only one "file" to drag into `/Applications`. I say "file" because although it looks like a single file, it's actually an entire directory structure with a special folder `Contents` containing meta-info that the Mac OS X GUI understands. Sort of like an unpacked Jar file but for native applications. Best of all, if I decide to remove the application (I've played with about 10 IM clients so far) just delete the application and whoosh, it's gone. In most cases about the only thing left lying around are preferences in `~/Library/Preferences`.

So, finally understanding all that, I thought I'd try my hand at building a deployable application - called a bundle. I found a [web site](http://developer.apple.com/documentation/Java/Conceptual/Java141Development/Deployment_Options/chapter_4_section_3.html) that documented most of the steps required. I also found an [Ant target](http://www.loomcom.com/jarbundler/) that allows you to generate a bundle from a build. The next step was to build an application.

Being the _Java Snob_ that I apparently am, I naturally decided to try deploying a Swing app. The fact that Mac OS X comes with Java out-of-othe-box is pretty cool. What's more the deployment mechanism supports Java apps directly meaning that besides some minor L&F issues here and there, Java applications slide right in amongst native applications.

So, I thought I'd put a "Launcher" GUI on Simian just for fun. Nothing special. Certainly nothing to replace the IDE plugins others have written for. And here's a [picture](/blog/archives/mac-app.gif). Makes me want to work on a Swing project even more. Though I believe [JavaScript](http://selenium.thoughtworks.com) is all the rage now ;-)

One thing I didn't do was specify an application icon but that's as simple as converting an image and dropping it into the Resource folder inside the bundle, something that can be achieved by using the `icon` parameter on the ant task.

The other thing I need to do is to find an ant task that can create disk images - the standard way Mac applications seem to be distributed.

And finally, just as I was about to post this, I stumbled across a [three-part series](http://today.java.net/pub/a/today/2003/12/08/swing.html) on making your swing applications play nicely on the Mac.

Not sure if any of this is any better, worse or really just the same as a Jar file. But it's kinda fun in that geeky kinda way.
