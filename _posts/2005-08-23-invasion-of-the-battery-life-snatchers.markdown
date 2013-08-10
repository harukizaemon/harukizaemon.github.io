---
layout: post
title: "Invasion of the Battery-Life Snatchers"
alias: /2005/08/invasion-of-the-battery-life-snatchers.html
categories:
---
Over the past 6-9 months, I've been doing a lot of development using my PowerBook running on battery. For the most part it works really well: The performance is just fine (with some tweaking of the power-saving settings), giving me around 2 hours of editing, browsing, emailing, etc. That is unless I'm developing code using IntelliJ.

When running in the foreground, IntelliJ seems to use anywhere between 3.0% and 6.0% of the CPU. Not too bad you might think but that is when I'm not doing anything with it. Even when IntelliJ is in the background -- either hidden or minimized -- it still uses around 3.0% of the CPU as this screen-shot from `top` clearly shows:

```
     PID COMMAND      %CPU   TIME   #TH #PRTS #MREGS RPRVT  RSHRD  RSIZE  VSIZE1259 top         14.5%  0:29.70   1    18    22   776K   372K  2.62M  26.9M -- &gt;  670 idea         2.7% 11:11.22  26   &gt;&gt;&gt;   485   270M  28.3M   231M   831M &lt; -- 1248 Safari       0.2%  0:06.22   6   127   238  8.89M  27.9M  18.8M   243M1247 SyncServer   0.0%  0:03.65   2    53    48  12.2M  3.36M  15.1M  47.3M1225 mdimport     0.0%  0:01.11   4    66    67  1.39M  3.27M  5.16M  39.7M668 bash         0.0%  0:00.02   1    14    17   220K   820K   904K  27.1M
```

A quick check of the Java version indicates that I am running JDK 1.5 by default:

```
simon$ java -versionjava version "1.5.0_02"Java(TM) 2 Runtime Environment, Standard Edition (build 1.5.0_02-56)Java HotSpot(TM) Client VM (build 1.5.0_02-36, mixed mode, sharing)
```

And a quick check of the About Box in IntelliJ confirms this.

Now admittedly I haven't tried any other Java applications so I'm not too sure if it's a Java issue, a Java on OS X issue, an IntelliJ issue or what but it is driving me nuts because it pretty much turns my 2+ hours of battery life into 30-45 minutes.

So if anyone has any idea how I might get IntelliJ to stop using CPU when it's idle, please, please, please let me know.

Oh and [Jeaves](http://www.eaves.org/blog), _"use Eclipse"_ and _"get a real computer"_ will not be considered helpful answers ;-)

**Update (30 August 2005)**

[Vote](http://www.jetbrains.net/jira/secure/ViewVoters!default.jspa?id=48645) for the [bug](http://www.jetbrains.net/jira/browse/IDEA-4729).
