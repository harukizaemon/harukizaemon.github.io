---
layout: post
title: "MovableType Weirdness Again"
alias: /2005/09/movabletype-weirdness-again.html
categories:
---
Some of you may have noticed some decidedly odd behaviour with the blog the past couple of days: you couldn't post comments; and I couldn't post new entries. Somehow -- I't's still a mystery -- things just stopped working. The suspicion I have is that there was some problem with the Berkeley DB files after a server upgrade but I can't be sure.

For the most part, MT behaves itself pretty well -- to-date I've hardly had any problems -- but when I do have a problem it usually requires quite a bit of effort to work out what went wrong and fix it (if possible).

So anyways, after unloading, reloading, repairing, upgrading MovableType and performing all manner of other unix witchcraft -- most of it performed at 3am -- things are finally back up and running again. The next step will be to migrate from Berkeley DB to Postgres in the next day or two so there may be a few more minor interruptions a long the way.

The upshot of all this is of course that I'm now running on the latest (and hopefully greatest) version of Movable Type. Having said that, blogging ain't my profession -- god save you all -- so I'm not sure that an upgrade of MT actually buys me much except some more long nights attempting to work out why things stopped working.

Everything seems to be in or-der but if you notice any more weirdness please let me know, otherwise I'll get back to eating my orange sher-bert

**Update (1 September 2005):** Ahh yes, I knew it was too good to be true. [Robert](http://twasink.net/) kindly informs me that comments are still busted: you can read them just not post them. After a little investigation, it looks like there is some problem with the new version of [SCode](http://www.movalog.com/plugins/wiki/SCode). It worked for me because I'm authenticated on my own blog but it doesn't seem to work for anyone who isn't authenticated. Sheesh! Maybe I need to write a suite of tests for my own blog!?

Thankfully, the solution is pretty simple. Thanks to the author of the plugin who responded to my [forum posting](http://forums.movalog.com/viewtopic.php?id=63): The file `SCode.pm` contains the "`$app->{requires_login} = 1;`". This should probably read: "`$app->{requires_login} = 0;`".

**Update (2 September 2005):** All converted to PGSQL now thanks to the truly awesome support people at MovableType. They ironed out the last little issues with upgrading and now I'm off Berkely DB. Hopefully this now means no more file corruptions.
