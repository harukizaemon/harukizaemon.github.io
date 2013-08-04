---
layout: post
title: "CVS Saves My Life Once Again"
alias: /2004/11/cvs-saves-my-life-once-again.html
categories:
---
Some time ago I trashed my linux machine by running `rm -rf` on a logical mount that, for reasons too mundane to discuss, was pointed at the root partition. Yes yes, snigger snigger hehe but tell me you've never done something similar :P. Anyway, a day or three later, I had resurrected my machine and restored all my files from those non-existant backups that we all vow to make...one day.

I deploy stuff to my various websites using ant scripts. Each time I deploy a new version of a product or project or even just make a change to some static HTML, it's automatically shipped using [JSCH](http://www.jcraft.com/jsch) then some shell commands are rune using SSH to move stuff around/configure things as necessary.

Up until about an hour ago, I had been using a semi-colon (`;`) as the command delimeter with no issues at all. There is problem with this that I had never considered - If any of the commands fails, the shell keeps on executing the remaining commands! Now in my case, one of those commands changes directories and is followed immedately by, you guessed it, `rm -rf *code. Not a problem if everything goes to plan but I had just recently renamed said directory! Needless to say, it wasn't pretty.

It then occurred to me that what I should have been using were double-ampersands (`&amp;&amp;`) which terminate execution at the first failure. Even better would be to simply rename (move) the existing directory and create a new one; My newly adopted strategy.

Thankfully, all my projects and web sites are in CVS so restoring them is never a problem which got me thinking about all the bits of code I've seen commented out or the unused classes left lying around because, like all those off-cuts of building material you're keeping in the shed, _"who knows when I might need that."_

More often than not CVS is used purely as a central repository that all developers can access but it can and should be more than that. Having everything in CVS allows greater fluidity in development. It allows developers to try something out and if it ends up completely [borked](http://www.urbandictionary.com/define.php?term=bork), well, we can always just roll back. [James](http://www.redhillconsulting.com.au/blogs/james) even suggested (now that he's a CVS guru along with [Jon](http://www.eaves.org/blog) :P) using CVS branching to do some speculative changes without disturbing the guys on the trunk whilst still allowing us to check-in the code. This is certainly something I would have been extremely reluctant to do even 3 months ago but having seen it work out (so far) for one of the projects on which we depend, I'm rather more inclined to give it a go.

As developers, we need to feel comfortable with and trust that all of this is possible. I doubt that many people I've met _actually_ understand all the subtleties and features of CVS, myself included. One thing I know for sure is that resurrecting dead files in CVS isn't nearly as simple as it sounds. I'm hoping [Subversion](http://subversion.tigris.org/) will address this but right now, something tells me that after everything I've just said, using beta-software for SCM on my critical projects is, perhaps, not the smartest thing one could do.
