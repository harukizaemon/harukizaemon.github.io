---
layout: post
title: "Managing Views with ActiveRecord Migrations"
alias: /2008/02/managing-views-with-activerecord.html
categories:
---
I just added very simple view support[^1] to [redhillonrails_core](https://github.com/harukizaemon/redhillonrails/tree/master/redhillonrails_core) trunk. The changes give you the ability to create and drop views using `create_view :name, "definition"` and `drop_view :name` respectively as well as preserving the views in `schema.rb`.

[^1]: WARNING: This is currently only supported for PostgreSQL. Creating views in MySQL will cause extra tables to be created in `schema.rb`! Probably not what you wanted.
