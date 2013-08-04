---
layout: post
title: "No, Sleep, Till Bedtime"
alias: /2009/06/no-sleep-till-bedtime.html
categories:
---
Or at least until all those [Twitter](http://twitter.com/) client developers have fixed their [Twitpocalypse](http://www.twitpocalypse.com/) bugs. In case you didn't know, a few days ago the ID range used for Twitter's messages exceeded 2^31 (approximately 2 billion) causing any apps that stored them as 32-bit integers to think they were really small negative numbers.

It's usual -- and I say usual because I don't always adhere to it -- policy for storing external identifiers is to treat them as text, even when I know they are numbers. Why? Essentially because I consider it a coincidence that they're numbers. That identifier, number though it may be, has no special significance to me over and above being an opaque handle to some entity in another system. As such, I like to treat them as text.

Discussing this with a good friend and colleague of mine, the question of column width came up. Ie. if you're going to make it text, how long should the column be? If you're lucky enough to be using a database such as PostgreSQL, then the answer is: it doesn't matter -- there's no performance benefit to artificially limiting the size of the column. For other databases, the common practice is to use something like `VARCHAR(255)`. Think about it, even if it is a number that's 10^255!

Twitter claims that its [API](http://apiwiki.twitter.com/Twitter-API-Documentation) is RESTful. And if to you, REST means nice, predictable URLs with some semantic path possibly followed by a numeric id and returning numeric ids in search results, then yes, it's RESTful. Want to see the most recent messages for a user? There's a simple HTTP request you can make to a nice, semantic (if you speak English) URL that returns a list of them and their identifiers. And, as expected, our Twitter clients have been dutifully squirrelling these ids away in integer fields (probably because that's the default) and all has well until 2 days ago.

Now, without going into too much of an ideological rant, I happen subscribe to the principle that RESTful URLs should be opaque. That is, a URL is a URL is a URL. No slicing, no dicing, no assembling, no joining. If I have a URL to a resource then that's what I use. Period. End of story. (You can find plenty of discussion on this by Roy Fielding using Google.)

So, back to our column widths. Assuming we have a text field in our database large enough to accommodate a URL, we could go one step further. Rather than treat the identifier as text and storing that, why not go the whole hog and store the URL instead?

As far as I can tell, the only reason is that Twitter's API, RESTful though they may claim, sends back numeric identifiers rather than URLs which in turn leads developers to incorrectly assume that they should be storing them as numbers.

On their own, identifiers are meaningless and in fact, useless. To utilise an identifier requires us to know the system in which it is stored and the collection in which it belongs. If instead each piece of information was identified by a URL we get all that context for free and the power to share information grows phenomenally.

To me, the beauty and power of the internet is the ability to link together disparate systems in ways no one had previously imagined. More specifically, in ways the publishers of the information never considered.

Opaque URLs combined with idiomatic use of HTTP verbs can help reduce the coupling between producers and consumers by giving back control to producers in how and where they store information and at the same time increasing the freedom for others to share and use that information.

(That last paragraph reads like an [Amnesty International](http://www.amnesty.org/) commercial!)

