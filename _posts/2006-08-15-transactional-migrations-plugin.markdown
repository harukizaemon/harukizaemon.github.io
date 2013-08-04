---
layout: post
title: "Transactional Migrations Plugin"
alias: /2006/08/transactional-migrations-plugin.html
categories:
---
I wrote a while ago on utilising [transactional DDL]({% post_url 2006-05-04-transactional-ddl %}) in your ruby on rails migration scripts so I decided to create a [plugin](https://github.com/harukizaemon/redhillonrails/tree/master/transactional_migrations).

In a nutshell:

> Transactional Migrations is a plugin that ensures your migration scripts -- both `up` and `down` -- run within a transaction. When used in conjunction with a database that supports transactional Data Definition Language (DDL) -- such as [PostgreSQL](http://www.postgresql.org) -- this ensures that if any statement within your migration script fails, the entire script is rolled-back.
