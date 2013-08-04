---
layout: post
title: "Procrastinating in Ruby is Delicious"
alias: /2006/09/procrastinating-in-ruby-is-delicious.html
categories:
---
As I was bookmarking something on [del.icio.us](http://del.icio.us/haruki_zaemon) today, I noticed the dates on which I had bookmarked the last couple of times and wondered if there was any correlation between frequency and day of the week. So, I downloaded a summary using `https://api.del.icio.us/v1/posts/all?` and whipped up a little ruby script to compile some statistics:

```
Wednesday = 41Tuesday = 39Thursday = 37Friday = 32Monday = 26Saturday = 24Sunday = 12
```

Looks like Wednesday is the biggest day for bookmarking -- also known as procrastinating -- and what do you know? Today is...Wednesday!

So then I thought I'd see if there was anything interesting in the time of day:

```
12 = 2613 = 204 = 1722 = 150 = 1423 = 125 = 122 = 1220 = 1011 = 101 = 107 = 103 = 96 = 721 = 79 = 615 = 414 = 38 = 310 = 219 = 2
```

Phew! Most of my bookmarking is done around lunchtime although an awful lot were done at 4am!
