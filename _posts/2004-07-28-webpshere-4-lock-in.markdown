---
layout: post
title: "Webpshere 4 Lock-In"
alias: /2004/07/webpshere-4-lock-in.html
categories:
---
Lately I'd come to think of Websphere 4 as a steaming pile of crap but then the other day I read an [article](http://www.newscientist.com/news/news.jsp?id=ns99993512) which changed my mind; _Human_ waste is actually useful for something.

We have a an application that runs within the web container against a DB2 database. For development we use [OrionServer](http://www.orionserver.com) which, sensibly, leaves the isolation level at `READ_COMMITTED` by default. For production we'll be using Websphere 4  which gives a default transaction isolation level of `REPEATABLE_READ`.

Sidebar: There are four basic transaction isolation levels:

* `READ_UNCOMMITTED` - Let me read anything even if it's not been committed yet; Actually you should be committed for using this;
* `READ_COMMITTED` - Only allow me to read stuff that's been committed; The sensible choice for the descerning developer;
* `REPEATABLE_READ` - I want to lock every row I've read because somehow I think I might want to read the same stuff again with the exact same results in the exact same transaction; Also known as please Sir can I have a deadlock; and;
* `SERIALIZABLE` - My users are nostalgic for the response times they enjoyed during the good old days of 1200/300 baud accoustic couplers.
Yay! Bring on those deadlocks. And the only way to change the isolation level in Websphere 4 is to deploy an EJB and specify the isolation level in the custom descriptor (I assume this is pretty common but no doubt not specified in the J2EE standard- anyone?). All very well and dandy IF YOU HAPPEN TO HAVE AN EJB FROM WHICH TO DO THIS!

 breath...in...out

Unable to get Websphere to cooperate in any sensible fashion (no I don't count EJB's as sensible) we turn to the database. Ahh good old DB2 you old thing you..hey...hey...nudge nudge...wink wink. What can you do for us?

Bugger all that's what! Well, not quite. A good-old Google search reveals some interesting stuff. It seems I can go in and set a server-level parameter `DB2_RR_TO_RS` which basically says "hey mister DB2, if that Websphere kid comes around here asking for `REPEATABLE_READ`, just smile and say yes yes and send him blissfully on his way but don't actually do anything. ok?" Even more interesting, we found this solution right on IBMs website for their own [Portal Server](http://www-306.ibm.com/software/genservers/portal/library/enable411/InfoCenter/wps/release_notes.html) Software! And what do you know? It worked. No more deadlocks.

So that should be it right? Wrong! The problem with this little solution is two-fold: Firstly, it's at the server instance not dabase instance level so it affects EVERY database; and secondly; it's not a supported option in DB2 version 8.1 (works in 7.1) which we'll be  using for production. Instead, 8.1 supports so-called [Type 2 indexes](http://www-306.ibm.com/cgi-bin/db2www/data/db2/udb/winos2unix/support/document.d2w/report?fn=db2e0db2e066.htm) which supposedly solve the same problem. The index solution seems a little fragile to me though. I bet my bottom dollar someone will add a new table and forget to add the appropriate indexes. BLAMMO!

Back to square one. We try every combination of `locklists` and `maxlock` parameters, you name it. None work and frankly although they would have decreased the likelyhood of deadlocks, they don't actually solve the underlying problem namely that that `REPEATABLE_READ` is a retarded default.

But what can we expect? I mean we aren't doing it the J2EE way. "Please Sir, can I have `READ_COMMITTED`" we feebly plead. "How can you have any `READ_COMMITTED` if you don't have any EJB's?" Websphere replies cruely.

In the end it looks like I'll need to dust off the old [train book](http://www.amazon.com/exec/obidos/tg/detail/-/0596002262/104-0733478-6224731?v=glance), wrap our [Hibernate](http://www.hibernate.org) persistence stuff in a session bean and hopefully get back to doing some load testing.

Even after all that, compared with the performance of OrionServer, Websphere seems to mysteriously turn my P4 into a Casio wrist watch. Given the amount of CPU power required to run it, I think we're going to need to find [alternative means](http://www.newscientist.com/news/news.jsp?id=ns99994761) of power.

Update: Having jadded the websphere code, it seems that the default isolation level for Sybase and Oracle is a more sensible `READ_COMMITTED`. That of course doesn't help us. It also turns out that once you've called an EJB, even a nop method, the database connection is then set to whatever isolation level was set for the EJB! Oh we're having fun!
