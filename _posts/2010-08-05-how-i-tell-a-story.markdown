---
layout: post
title: "How I tell a story"
alias: /2010/08/how-i-tell-a-story.html
categories:
---
As far as I can tell, the most widely accepted format for agile user stories looks like this:

``` text
As a [role]
I would like to [action]
So that [reason for action]
```

I'm not entirely certain of the origins. Some have suggested [Dan North](http://blog.dannorth.net/whats-in-a-story/) and others have have suggested much earlier use. In any event, it's use is so common I can't recall a single "agile" project where that wasn't the format but I've also never really been that comfortable with it and I've not really known why until recently.

Here's an example:

``` text
As a domain novice
I want the domain experts names made prominent on a domain listing
So that I know who to turn to for help on a given domain
```

Though entirely contrived for the purpose of this discussion, it demonstrates the kind of stories people are inclined to write.

On the positive side, the format seems quite natural -- people often have an implementation in mind and they can articulate it quite easily but find it much harder to articulate the intent. This seems to be reflected in the format with the primary focus ("I want") expressed as the desired behaviour and the intent ("So that") of conceptually lesser importance. The problem I have is the story centres around how the user will interact with the system and only then describes why. It presupposes an implementation with a cursory nod as to the user's intent.

<!--more-->
In my various roles from analyst to developer, I've always wanted to understand why we're building a new feature. Experience has taught me that when I understand the why, I am better able to build a solution that satisfies the real need. As a consequence, when running workshops, I've always tried to have the intent stated as the primary desire/need. For example:

``` text
As a domain novice
I want to know who to turn to for help on a given domain
So that .... [what the hell goes here?]
```

When people go to the trouble of putting the intent first, the "So that" becomes a bit of a WTF, often resulting in a simple re-statement of the intent just to satisfy the template:

``` text
As a domain novice
I want to know who to turn to for help on a given domain
So that I can learn more about the domain [well d'uh!]
```

Moreover, we've now lost the implementation detail which, although we're more interested in the intent, is still of value by providing context for further discussion/understanding.

What I've wanted was a way to encourage people to describe their intent and leave the implementation details to be decided closer to the iteration in which the story will be delivered. Yes, training is one way to achieve this but my experience is that the standard format effectively devalues the intent in favour of implementation detail. What I'd really like is to somehow [nudge](http://www.amazon.com/Nudge-Improving-Decisions-Health-Happiness/dp/0300122233) people into writing a story where the primary focus is the intent.

Enter the format I've been using recently and really liking:

``` text
As a [role]
I want to [intent]
[For example] By [action]
```

It's a small but subtle change. To some so subtle that it verges on the trivial. However, my experience is that it's similar enough to the original format so as not to seem too radical a change and at the same time different enough to encourage the behaviour I am looking for. By way of example:

``` text
As a domain novice
I want to know who to turn to for help on a given domain
e.g by having the domain experts names made prominent on a domain listing
```

The intent is now first and clearly specified as a higher order issue. It makes no sense to express the intent as "For example by wanting to know who to turn to for help on a given domain" so the only sensible place for it is as "I want ...".

That the last line begins with "By" identifies it as an implementation detail, acknowledging that most people have some idea of how they want it implemented and allowing them to express that without feeling awkward. With the addition of "For example" or "e.g."-- as suggested by [Mike Williams](http://dogbiscuit.org/mdub/weblog/) -- we further clarify that it as up for debate once the story hits an iteration. Finally, by placing it last and making it optional we indicate that it lends less weight than the intent.

The more I use this format the more it becomes clear when the intent is expressed as implementation, my stories tend to read much more like a plot or a narrative of the software, and I'm finding it nudges/leads me to write stories that express the intent rather than work backwards from the implementation. I've also noticed I'm developing some heuristics for "validation":

* When the implementation contains a conjunction (and) it probably indicates the story is an epic.
* When the intent contains a conjunction, it probably indicates more than one story/epic.
