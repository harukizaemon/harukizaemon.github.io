---
layout: post
title: "I've Finally Been Subversioned"
alias: /2005/03/i-finally-been-subversioned.html
categories:
---
Having been berated for [bashing](/blog/2004/11/08/cvs-saves-my-life-once-again) [subversion](http://subversion.tigris.org/) based on heresay and rumour, it was about time to make the switch and actually see for myself what life without CVS was like.

As luck would have it, the guy who does all my [hosting](http://www.geekisp.com) had already been considering providing SVN support so that when I asked him he was more than happy to make a guinea-pig out of me ;-)

As I didn't do the conversion from CVS to SVN myself, I can't comment on what is involved but I can say that it appears to have gone seamlessly and without fault. I installed the latest SVN client on my mac and was up and running within minutes.

Initial impressions are good: The command line application is very straight-foward and somehow seems more intuitive than the CVS equivalent; It certainly runs faster than CVS (or at least appears to) as it only sends diffs to the server (based on checksums); and; It will really come into its own in a week or so when I will need to perform a bulk rename of files for the book - the real reason I needed SVN in the first place.

The only mildly dissapointing thing is the rather less than wonderful support in IntelliJ: Even in the latest [EAP](http://www.intellij.net/eap) versions it seems to be much slower than the command line application and doesn't have an option for hiding the `.svn` directories. No doubt all of these will be "fixed" soon...right?

The best part for me is the repository versioning: every commit increases the repository version number so in effect you get a new tag with every commit. This will come in particularly handy for some code analysis tools I've been developing that track metrics over time. Previously I had to go through hoops to try and work out what changed and when in CVS. Now SVN gives me this information for free!

So all in all it's happy sailing on the SVN ship. That is of course until my repository becomes corrupted. JUST KIDDING!
