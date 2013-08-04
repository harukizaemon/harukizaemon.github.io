---
layout: post
title: "Lock Your Screen with LaunchBar"
alias: /2008/02/lock-your-screen-with-launchbar.html
categories:
---
Mostly just so I don't forget, here are some stupidly simple instructions for locking your screen using [LaunchBar](http://www.obdev.at/products/launchbar/). (Technically, this actually starts the screen saver but as that is password protected it has the same effect as locking the screen.)

* Open the the Script Editor, `/Applications/AppleScript/Script Editor`
* Enter the text `activate application "ScreenSaverEngine"`
* Save it to `~/Library/Scripts/Lock Screen`
* Open LaunchBar &gt; Configuration &gt; Scripts &gt; Options
* Enable "Home Library Scripts"
* Save and Rescan

To lock the screen, simply open LaunchBar and type `lock screen` (on my setup `loc` is enough) and hit enter. Voila!

Hotkey fans will probably want to consider using something like [MenuMaster](http://unsanity.com/haxies/menumaster) or [iKey](http://www.scriptsoftware.com/ikey/) to execute the AppleScript.
