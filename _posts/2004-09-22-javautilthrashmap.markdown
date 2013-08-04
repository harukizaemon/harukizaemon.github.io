---
layout: post
title: "java.util.ThrashMap"
alias: /2004/09/javautilthrashmap.html
categories:
---
I received a very interesting [post](http://www.mail-archive.com/jess-users@sandia.gov/msg07290.html) from the [JESS mailing list](http://www.mail-archive.com/jess-users@sandia.gov/) last night and thought it was worth a mention.

> There's a weird threshold that can occur with any Java data structurethat uses large arrays of Object references (Object[]). If the size ofthe Object array exceeds the maximum size of objects that can be placedin the "new generation", garbage collection performance can be severelyimpacted...

I'm not sure if I'm likely to see this problem really but I do ue `HashMap`s a fair bit so I thought it was interesting. In general I don't find the need to use arrays much if at all these days. In fact, for some inexplicable reason, I tend to use `LinkedList` over `ArrayList` and, except for [Simian](/simian) which uses lovingly hand-crufted data structures, I can't recall ever holding maps of data large enough to exhibit this behaviour. But then again I'm not implementing a Rules Engine.
