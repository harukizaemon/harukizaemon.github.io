---
layout: post
title: "Novel use of Hibernate?"
alias: /2003/07/novel-use-of-hibernate.html
categories:
---
I've been working on and off on a project that uses Hibernate persitent classes in Rhino Javascripts.

For various reasons, the database tables (and therefore the persistent classes) aren't known until runtime at which point the meta-data is read from a database.

I used the ASM byte-code library to dynamically generate classes at runtime, map them using Hibernate and expose them with some &quot;fancy&quot; wrappers in Rhino. So anytime they update the database schema, all they need to do is either restart the server or send it a &quot;re-load signal&quot;.

All in all a very simple solution that was very easy to get up and running.

Interestingly, the fact that hibernate doesn't require code generation was my saving grace. I'm not suggesting thate code generation is bad but in this particular case it ouldn't have worked as well, IMHO.

I'm now working on doing some file-import using the same "framework". Basically a legacy system spits out EDI-ish data with record types, etc. that needs to be imported. The database meta-data contains a mapping between record types and layout to tables and columns espectively.Again, this will no doubt involve some dynamic class generation to perform the     import? But maybe not. I'll start with some tests and see what happens ;-) Gotta love TDD.
