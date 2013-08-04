---
layout: post
title: "Just Tell 'Em Joe Sent You"
alias: /2007/02/just-tell-joe-sent-you.html
categories:
---
A customer walks into a bank and, after a waiting in the queue, reaches the teller:

> **Teller:** _How may I help you?_
> **Customer:** _I'd like to transfer a million, billion, squintillion dollars from Jo Blogg's account into mine please._
> **Teller:** _Certainly but first I'll need some proof of identification._
> **Customer:** enters account number and PIN and clicks enter._
> **Teller:** _Ok, now I'll need to authorise the transfer, please wait a moment._
> **Customer:** _Oh, that's ok, Joe gave me permission._
> **Teller:** _I'll get to it immediately then._

As ridiculous (and somewhat contrived) as this scenario seems, it's a pretty apt reflection of how many large companies implement security in their web applications. Indeed many VERY large companies. Companies that, if you knew who they were, would certainly have a lot of explaining to do.

The problem as I see it is that developers in general don't understand security. Actually, no, the problem is a little more fundamental than that. Developers in general don't understand the technology stack with which they work day in, day out.

I can appreciate how difficult it might be to accept that you're forking out in excess of $80k a year for a developer that doesn't seem to understand that just because the user can't see a hidden field on a form doesn't mean they can't look at the page source; that just because a user typically interacts with the server using a browser, doesn't preclude the use of something as simple as a telnet client. In fact, I'm willing to bet that if you asked something along the lines of "have you tried seeing if you can connect to the server using telnet" you'll get blank stares all around.

The sad truth is that using hidden form fields, magic request parameters, default credentials, and even sending permissions to the client (even if that means web server as a client of EJB) on login and then trusting the client to send those same permissions back with each request in order to, I don't know, perhaps "save database lookups" or something, is disturbingly common in practise.

Heck, I don't even claim to be a security expert but I'm pretty sure that not using SQL bind parameters is generally considered a bad idea. Then again, perhaps it's not so much of a problem when all you're doing is reading clear-text passwords out of a database?!
