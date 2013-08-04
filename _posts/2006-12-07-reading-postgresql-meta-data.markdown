---
layout: post
title: "Reading PostgreSQL Meta Data"
alias: /2006/12/reading-postgresql-meta-data.html
categories:
---
If, like me, you need to read in various bits and pieces of meta-data in PostgreSQL, you've probably found it rather tiresome navigating your way around the various `pg_catalog` tables in order to determine where the one piece of information you need lives.

After a bit of googling, I discovered a nifty option you can set in `psql`: `\set ECHO_HIDDEN 'yes'`

Then whenever you describe a table, index, etc. using `\d` and friends, you'll also get the SQL that was used!
