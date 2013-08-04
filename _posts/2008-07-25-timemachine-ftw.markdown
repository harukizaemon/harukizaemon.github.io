---
layout: post
title: "TimeMachine FTW!"
alias: /2008/07/timemachine-ftw.html
categories:
---
Not withstanding the fact that I needed to restore my operating system in the first place -- due to an inexplicable and catastrophic failure of the Java installation resulting in segfaults -- I was able to restore my entire 100GB system in around 4 hours. For posterity:

* Boot off the OS X System Install DVD -- hold down option while the system starts
* Connect the external drive with the TimeMachine backup -- in my case a TimeCapsule attached via ethernet
* Select "Restore from TimeMachine backup" in the Utilities menu
* Select the specific backup (by timestamp) from which to restore
* And away you go!

The disk is then automatically erased and a fully bootable system is restored sans temp directories and cache files. It even managed to restore my PostgreSQL databases that were running at the time -- which probably says more about PostgreSQL than anything.

The one grumble I do have is that the timestamps in the name of the backups were some non-obvious period relative to the actual date the backup was made. The difference wouldn't have been much of an issue had I simply needed to restore the most recent backup but as it turned out I needed to go back a couple of days in order to get a clean system. Thankfully I got lucky on the second attempt :)

Once I had restored the system I took a look at the backup folders and sure enough there are two timestamps: the one in the folder name, and the created date. The created timestamp was spot on but the one in the folder name -- the one presented to you when restoring -- was whacky. I honestly didn't spend long enough to calculate if the difference was consistent.

What is really interesting is that I had SuperDuper! on my list of software to start using but it would appear there is little need -- at least in my case.
