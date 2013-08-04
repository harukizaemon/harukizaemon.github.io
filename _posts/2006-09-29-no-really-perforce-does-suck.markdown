---
layout: post
title: "No Really, Perforce Does Suck"
alias: /2006/09/no-really-perforce-does-suck.html
categories:
---
Ok, so after [my rant]({% post_url 2006-09-27-perforce-just-a-faster-cvs %}) yesterday I was feeling a bit better. So many people rushed to the defence of Perforce and on the authority of people I know, respect and work for -- not mutually exclusive roles -- I thought I'd get stuck into it and read the manuals, read news groups and even rushed out to buy a copy of [Practical Perforce](http://www.oreilly.com/catalog/practicalperforce/).

The documentation is plentiful and very informative and the support groups are very helpful. As for the book, well, the book is most excellent, a very easy read indeed and full of tonnes of really great tips -- recipes, idioms, patterns, hacks, call them what you will -- which just about sums up my experience thus far: Lots and lots of rather involved processes to do what I consider to be normal everyday activities. (At this point I feel compelled to direct you to an excellent article on [why patterns are indicative of unsophisticated systems](http://newbabe.pobox.com/~mjd{% post_url 2006-09-11- %}).)

To give you a 100% practical example, just today I committed 1600 files which I had to back-out almost immediately because I realised I had broken something. Now, ignoring the why's and how's I managed to get myself into such a pickle, the fact is I needed to rollback a commit. Here's what I did:

{% highlight console %}
> svn merge -c -27289 svn+ssh://me@therepositoryurl
> svn commit
{% endhighlight %}

Tricky stuff that!

So then on my way home I picked up the book mentioned earlier and went straight to the index to find "Backing out a recent change". Whoot! Just what I wanted to know. So here's the deal:

{% highlight console %}
> p4 files @=27289         # This lists all the files that have changed
> p4 sync @27288p4 add ... # For each deleted file
> p4 edit ...              # For each changed file
> p4 syncp4 delete ...     # For each added file
> p4 resolve -ayp4 submit
{% endhighlight %}

Yes! Pretty impressive! And, straight from the book, re-printed without any permission whatsoever (emphasis added by yours-truly):

> When a change involves a lot of files, you can filter the output of the `files` command to produce a list of files to open. **Unfortunately, `files` can't be piped directly to other p4 commands because its format isn't acceptible to them.** This can be easily fixed by using a filter; namely `sed`.

Wow. Cool! Just what I wanted to have to do. Ok, so let's try that:

{% highlight console %}
> p4 sync @27288p4 files @=27289 | sed -n -e "s/#.* - delete .*//p" | p4 -x- add
> p4 files @=27289 | sed -n -e "s/#.* - edit .*//p" | p4 -x- edit
> p4 sync
> p4 files @=27289 | sed -n -e "s/#.* - add .*//p" | p4 -x- delete
> p4 resolve -ay
> p4 submit
{% endhighlight %}

Awesome! That's sooooo much better. Sheesh, I might _even_ be able to script it, fan-bloody-tastic. Thankfully, Perforce is touted as being lightning fast because unless I'm very much mistaken, that's seven, count 'em, seven calls to the server!

So, what have we learned so far? We've learned that precisely the scenario I've been told Perforce is great at handling, it really, really, really, ok once more, really, sucks!

Oh, but there's more. I forgot to mention that I was also working offline before I committed the original sin. When I eventually connected this is what I did:

{% highlight console %}
> svn commit
{% endhighlight %}

Ok, so technically I did:

{% highlight console %}
> svn up
> svn commit
{% endhighlight %}

So, what would have been the equivalent if I had been using Perforce you might ask?

{% highlight console %}
> p4 sync
> p4 diff -se | p4 -x- edit
> p4 diff -sd | p4 -x- delete
> p4 submit
{% endhighlight %}

(As a side note, adding new files in both systems is about the same amount of work. That said, at least with subversion a simple `svn sta` will show me which files are not yet under version control. For the life of me I can't seem to find an easy way to do this with Perforce.)

Not too bad but technically, three times as many commands. And yes, again, I could script it but why should I need to? This is something I, as a developer, do every day. Am I mistaken for thinking that developers are by far the largest users of a tool such as this? Perhaps.

It's no wonder Google want people to know how to use Perforce; it pretty much proves the candidate has a brain large enough to even feel like working out how to use it.
