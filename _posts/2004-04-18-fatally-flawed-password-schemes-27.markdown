---
layout: post
title: "Fatally flawed password schemes #27"
alias: /2004/04/fatally-flawed-password-schemes-27.html
categories:
---
I feel so stupid! I honestly can't believe it's taken me this long to realise what a drongo I've been.

Many years ago (well probably 3 or 4 years ago) a work colleage of mine at a new job was was using a password scheme that, at that time at least, was quite common amongst his colleagues (yes I know that by inference, they were now my colleagues also hehe). Anyway, this scheme involved replacing vowels with numbers. So, for example, "Hello" would become H3ll0.

At the time this seemed to me like a sensible thing to do so I blindly followed along. But just now it suddenly occurred to me how crack-headed (no pun intended) and brain-dead that was. I mean if the whole point was to prevent brute-force dictionary attacks then its clear (to all but me it seems) that attempting to obfuscate a password with a deterministic algorithm such as this is just pointless.

Fortunately I think I only ever used it on one machine I had at home those many years ago. But I wonder how many others continue to use it?
