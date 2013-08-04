---
layout: post
title: "That Weird Devil Number"
alias: /2006/07/that-weird-devil-number.html
categories:
---
This morning, my brother was sitting at his laptop attempting to get FreeBSD to mount a ReiserFS partition without much success -- some permissions problem that means he can mount it as root but not from `fstab` -- when his girlfriend sat down beside him, peered over, and asked "What's that weird devil number?" Naturally we both had a little WTF moment. "See", she continued, "it's even called devil!"

On closer inspection, it seems the file permissions for `/dev` are, understandably, `666`. Riiiiiigghhht!
