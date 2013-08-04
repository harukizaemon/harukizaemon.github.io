---
layout: post
title: "Turning Japanese"
alias: /2007/09/turning-japanese.html
categories:
---
You've moved to another [country](http://web.mac.com/jessecaah/Site/Blog/Entries/2007/7/17_Arriving_in_Iwama.html). Used the opportunity to take some time off work and find a [place to live](http://web.mac.com/jessecaah/Site/Blog/Entries/2007/7/19_New_House.html). Settle in and get to know your [surrounds](http://web.mac.com/jessecaah/Site/Blog/Entries/2007/8/2_a_day_at_the_beach_..._well_..umm%2C_the_lake_%3B).html). Indulge a little in the local food, [culture](http://web.mac.com/jessecaah/Site/Blog/Entries/2007/8/28_Iwama_Matsuri.html) and just a few of the [local beverages](http://web.mac.com/jessecaah/Site/Blog/Entries/2007/9/2_Farewell_Party.html). Your wife get's bored so you get connected to the internet but resist the temptation to do anything really useful.

Finally, after much um'ing and ahh'ing and hollow excuses as to why it's still not the right time, you decide you really had better start earning a living again. So, you make sure the VPN still allows you to connect to your customer's servers, switch to the directory of the project you were working all those weeks (ok months) ago and type `svn up`. A dozen or so filenames whizz by and then the screen appears to freeze. 5 minutes go by. Then 10. Then an hour. Then two. What the...?!

After a few emails back and forth between the customer's technical people it becomes apparent that the freeze is a result of downloading a 33M jar file from the repository coupled with the client's internet connection speed being at least 1/10th if not 1/100th that of my 50M/s ADSL. So what do you do? There's no studio audenice; it doesn't make sense to go 50/50; so, I guess I'll have to phone-a-friend.

A cunning plan emerges. James would update his checked-out version onto his laptop over the LAN at work -- resulting in a whopping 1.14G of source material:

```
james> svn up project
```

Even zipped this would still be enormous but remembering that good-old SVN keeps two copies (one as a backup) we then deleted all files _except_ those in the `.svn` (ie backup) directories:

```
james> find project -type f -not -path '*.svn*' -delete
```

This was then zipped up resulting in a modest 150M:

```
james> tar -cvzf project.tgz project
```

And sent to a server in the US -- being a whole lot easier than tunnelling through firewalls and NAT'd routers -- from his home connection -- being a whole lot faster than the one in the office:

```
james> scp project.tgz server:
```

I then downloaded it:

```
simon> scp server:project.tgz .
```

Unzipped it:

```
simon> tar -xvzf project.tgz
```

Switched the user:

```
simon> cd projectsimon> svn switch --relocate svn+ssh://james@repos svn+ssh://simon@repos
```

Restored all files from their backup copies:

```
simon> svn revert -R .
```

And finally did an update to be sure:

```
simon> svn up
```

Which all makes me wonder if were it not for all these nifty command-line tools with which to play, might there have been an easier solution? Was this perhaps just a little too much technical masturbation? M'eh who cares. It was fun and it worked!
