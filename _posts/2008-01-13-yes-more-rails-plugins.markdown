---
layout: post
title: "Yes, more Rails plugins"
alias: /2008/01/yes-more-rails-plugins.html
categories:
---
FWIW, I've just created two very new, very simple and VERY BETA Rails plugins.

The first, [Simian](/simian), is pretty obvious: it adds a couple of rake tasks to run duplicate code checks against your rails project using, you guessed it, [Simian](/simian).

The second is [Restful Transactions](https://github.com/harukizaemon/redhillonrails/tree/master/restful_transactions). This plugin ensures that your controller's `create`, `update` and `destroy` actions are wrapped in a database transaction meaning you don't have to think about it.

As always, you'll find these plugins and more over at the RedHill on Rails [plugin page](https://github.com/harukizaemon/redhillonrails/tree/master/$1)

**Update:** The plugin now wraps any action executed as a POST, PUT or DELETE rather than just the create, update and destroy actions.
