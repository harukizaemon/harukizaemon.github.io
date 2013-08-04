---
layout: post
title: "Subversion Gotcha #17"
alias: /2006/01/subversion-gotcha-17.html
categories:
---
Ok, so actually it's the first that has bitten me but calling it #1 implies it's the worst one there is and who am I to bestow that honour ;-)

I refer you to [this](http://www.contactor.se/~dast/svn/archive-2004-07/0636.shtml) positing in an [SVN](http://subversion.tigris.org/) news group.

It seems that by default, when using [Berkely DB](http://www.sleepycat.com/) with SVN, the default setting is to auto remove log files. Now log files serve a purpose: they allow you to re-play transactions. If those log files no longer exists...no guesses needed.

The rationale for the decision to have this behaviour by default (and I'm paraphrasing): We (the SVN community) don't want people to complain to us with silly stuff like my disk is full. Rather we'd prefer that in the unlikely event of a repository crash, we'd rather that poor sod looses his days work. We made a trade-off between convenience and security.

Hmmm...let me think about that. How often does my hard disk crash? Almost never. And how often do I need to restore from a previous version of a file? Almost never. And how often do I back up my hard disc? Every day. Gee...maybe I don't need a versioning repository?

If I said that at work, I'd be laughed off the premisis. Why? Because a repository is **more than just a network filesystem**. Moreover, it holds one of my companies major assets: the source-code.

I think I can live with my disk filling temporarily every once in a while versus losing a days work multiplied by 50 developers. In the worst case no one can check in. At least we know there's a problem.

So anyway, this bit me last night when the server on which my SVN repository resides had an NFS failure and corrupted some files. No problem, we can just replay the log...oh they're not there!

One could argue that I (as the maintainer of the repository) should have known about this option and turned it off. However that argument simply doesn't wash as the main argument for turning it on by default in the first place was that most people coming over to SVN won't know enough about how to configure SVN to do the right thing so a choice was made in favour of convenience.

Thankfully, we have daily backups and (in this instance) we only lost two commit sets. So what am I whining about? I'm whining in the hope that this doesn't happen to you under more critical circumstances.

So how about turning that option off by default and putting a big notice in big bold red lettering on the front cover saying **"If your disk fills up, it's most likely the logs. Go delete the log files for any days for which you have backups. Leave the ones for which you presently have no backups -- ie todays."** Not so hard now is it?
