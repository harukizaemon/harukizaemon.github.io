---
layout: post
title: "Pulse on PostgreSQL"
alias: /2007/02/pulse-on-postgresql.html
categories:
---
By default, my current toy ([Pulse](http://www.zutubi.com/products/pulse/)) stores its meta-data in an [HSQLDB](http://hsqldb.org/) database. Which is fine (it's a darn good database) but I already have plenty of infrastructure around periodically dumping all PostgreSQL databases on my system for backups. Wouldn't it be nice if Pulse ran on PostgreSQL.

Thankfully, the fellas at [Zutubi](http://www.zutubi.org/) (purveyors of the afore-mentioned toy) have written up a quick [HOWTO](http://confluence.zutubi.com/display/pulse0102/CB+-+Migrating+To+MySQL+Or+PostgreSQL) on migrating to PostgreSQL and it worked like a charm.

About the only downside is that each time I upgrade, I need to remember to add the `postgres.jar` to the `lib` directory. Hmmm. Perhaps I need to add it to the classpath before starting the server...?
