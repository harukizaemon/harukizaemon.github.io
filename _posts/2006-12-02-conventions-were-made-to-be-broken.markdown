---
layout: post
title: "Conventions Were Made to be Broken"
alias: /2006/12/conventions-were-made-to-be-broken.html
categories:
---
The Rails convention for naming foreign key fields is to use the singular name of the table with a suffix of `_id`.

In the past few days a number of people using the [Foreign Key Migrations](https://github.com/harukizaemon/redhillonrails/tree/master/foreign_key_migrations) plugin together with ActiveRecord session store have discovered that the exception to the rule is `session_id` in the `sessions` table. In this case, `session_id` is not a recursive relationship but an unfortunately named field.

The solution is to update the migration script and add `:references => nil` to the line that adds the `session_id`. The plugin will then ignore the column and not attempt to generate a database foreign key constraint.
