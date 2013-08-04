---
layout: post
title: "Prevent Mac Droppings on Network Drives"
alias: /2005/08/prevent-mac-droppings-on-network-drives.html
categories:
---
In my current gig, I'm using my PowerBook in an all M$ Windoze (with the exception of Linux servers) environment and it's working a treat. Except for one thing: all those `.DS_Store` files.

Finder in Mac OS X (and probably previous versions too I imagine) creates `.DS_Store` files whenever you browse a directory. The file is seems pretty harmless - apparently it contains little more than window preferences, etc. - and Finder hides them from view.

Unfortunately, it does get the back up of some of the other developers. Not really because the files exist, but more because, for some reason, the files get created with a timestamp in the future which causes all manner of problems for the guys writing their MFC applications - with asserts turned on the applications barf all over the place.

So, I did a quick hunt on google and found [this article](http://forevergeek.com/apple/preventing_creation_of_ds_store_files.php) that explains how to prevent the behaviour. It's a pretty simple fix and involves entering the following at the command-line (possibly followed by a re-boot?):

{% highlight console %}
> defaults write com.apple.desktopservices DSDontWriteNetworkStores true
{% endhighlight %}

Now if someone could only tell me why I seem to end up with all these `._XXX` files lying around that don't appear in Finder, nor when running `ls -la` but do end up in my zip and tar balls.
