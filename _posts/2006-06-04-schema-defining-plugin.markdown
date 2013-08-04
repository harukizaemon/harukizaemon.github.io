---
layout: post
title: "Schema Defining Plugin"
alias: /2006/06/schema-defining-plugin.html
categories:
---
Just a quick update on my plugins. Two of them -- [Foreign Key Migrations](https://github.com/harukizaemon/redhillonrails/tree/master/foreign_key_migrations) and [Row Version Migrations](https://github.com/harukizaemon/redhillonrails/tree/master/row_version_migrations) -- needed to know if they were being run as part of `ActiveRecord::Schema.define()` so I extracted the common code into yet another plugin: Schema Defining. [This new plugin](https://github.com/harukizaemon/redhillonrails/tree/master/schema_defining) provides a class method -- `ActiveRecord::Schema.defining?` -- that returns `true` if `ActiveRecord::Schema.define()` is currently running; `false` otherwise. Probably not a hugely useful plugin in the main but certainly a must have if, like me, you have a number of migration plugins that need to alter their behaviour accordingly.
