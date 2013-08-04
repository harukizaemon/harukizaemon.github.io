---
layout: post
title: "Simian bake-off"
alias: /2004/01/simian-bake-off.html
categories:
---
Well you asked for it and here it is. Results from running the native, C#, flavour of Simian versus the Java flavour.

As I mentioned earlier, I had originally run the comparison on my linux machine using mono. As many people had pointed out, this was far from a "fair" comparison. Some people even suggesting that purely porting the code would result in poor performance. To this I reply fooey.

The test (and I use the term loosely) was performed on a DELL 2.0 GHz Inspiron 4150 with 512MB RAM running Microsoft Windows XP Pro against the JDK 1.4.1_01 source.

And the winner is...I'll let you be the judge:

The java version using the Sun JDK 1.4.2_03 ran in 64MB:

```
> java -jar simian.jar -recurse=*.java > java.txt
Similarity Analyser 2.1.0 - http://www.harukizaemon.com/simian
Copyright (c) 2003-11 Simon Harris.  All rights reserved.
Simian is not free unless used solely for non-commercial or evaluation purposes.
Loading (recursively) *.java from C:\jdk1.4.1_01\src
{ignoreCurlyBraces=true, ignoreModifiers=true, ignoreStringCase=true, threshold=9}
...
Found 40880 duplicate lines in 2339 blocks in 872 files
Processed a total of 369957 significant (1187603 raw source) lines in 3889 files
Processing time: 18.337sec
```

The C# version ran natively in 61MB:

```
> simian.exe -recurse=*.java > csharp.txt
Similarity Analyser 2.1.0 - http://www.harukizaemon.com/simian
Copyright (c) 2003-11 Simon Harris.  All rights reserved.
Simian is not free unless used solely for non-commercial or evaluation purposes.
Loading (recursively) *.java from C:\jdk1.4.1_01\src
{ignoreCurlyBraces=True, ignoreModifiers=True, ignoreStringCase=True, threshold=9}
...
Found 40880 duplicate lines in 2339 blocks in 872 files
Processed a total of 369957 significant (1187603 raw source) lines in 3889 files
Processing time: 12.628sec
```

Running with `-server` gains us about an 8% improvement in performance for the Java version but certainly still nothing like the nearly 30% needed to catch up to the native .Net

Surprisingly, running under BEA JRockit 1.4.2_03 used 235MB and tookaround 35 seconds using all default settings. The disk seemed to be thrashing but we made no attempt to tune the performance using JRockit options.

The C# version running under mono on the same hardware ran in 250MB and took around 78 seconds. Unfortunatele\y we couldn't get any of the optimize features to work on the windows version of mono. Besides, we figure this comparison is rather moot. Rather it is better to compare Java versus Mono on linux.

So here are the results on a DELL 1.8GHz Inspiron 8200 with 1GB RAM running Gentoo Linux (2.4 kernel) against the JDK 1.4.2_03 source.

The java version using the Sun JDK 1.4.2_03 ran in around 60MB and took 25 seconds.

The C# version under mono (with `-O=all`) ran in around 90MB and took 34 seconds.

Amusingly, nay astoundingly, the .Net version runs natively faster under windows+VMWare+linux than the mono on straight windows or straight linux!! Go figure?

Interesting to say the least. I wait with baited breath for the ensuing storm of abuse from the Java community this entry generates Hehehe. Though it'll make a change from receiving a serve from the .Net community.

Now the task is to see if we can pin-point what accounts for the difference. Unfortunately I fear, that because it's a direct port (ie line by line), any improvements I make to the Java version will likely carry forward into the .Net version.

Performance aside .Net is still _not my bag baby_. It still feels a little clumsy. But then I've years of practise getting my Java up to scratch.
