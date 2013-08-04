---
layout: post
title: "Hear Me Sort"
alias: /2007/02/hear-me-sort.html
categories:
---
[James](http://www.redhillconsulting.com.au/blogs/james/) has a home-grown [GTD](http://en.wikipedia.org/wiki/Getting_Things_Done) system which he'll hopefully write about if not release to the world one day. It's totally command-line driven using a combination of bash, perl and ruby to manipulate plain text files and, from what I can tell, it's not only blindingly fast, it seems to work seriously well.

One of the daily tasks that James performs is to prioritise the things he wants to (attempt) to get done and what better time to do that than on the 45 minute train trip to work. As you can well imagine, prioritising 40 items takes a fair amount of time not only due to the sheer number but also, being vision-impaired, reading (and re-reading) takes a bit of concentration. So, we came up with a nifty solution in two parts.

The first part was quite simple: use binary insertion. That way, instead of O(n^2) comparisons, we'd get O(n log n). So far so good. But the that was pretty obvious. The next bit was the real doozy!

Rather than have each pair printed out so that James could read them and make a decision, we instead chose to use some Text-To-Speech (TTS) to literally speak each pair; something along the lines of:

> Is calling your boss to discuss staff pay reviews more important than send wife a bunch of flowers for her birthday?

In addition, we actually run the TTS in another thread so that we can easily interrupt it, primarily because James: has already chosen a suitable answer; needs to repeat the question; or wants to cancel the whole thing.

About the only problem we encountered was that, on his Powerbook, the `say` command (built-in to Mac OS X) ran a little too slowly for our liking so we simply aliased it to use the much faster `swift` command that comes as part of the commercial [Cepstral William](http://www.cepstral.com/demos/) voice he uses for all his Text-To-Speech.

Now James can listen to the minimal set of questions, making the appropriate decision for each by way of a simple keystroke, and have it all done within 15 minutes all the while free to sit and enjoy the view (such that it is on a suburban train during peak hour) rather than staring at the screen.
