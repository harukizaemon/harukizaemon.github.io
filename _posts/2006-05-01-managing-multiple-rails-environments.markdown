---
layout: post
title: "Managing Multiple Rails Environments"
alias: /2006/04/managing-multiple-rails-environments.html
categories:
---
I've written briefly on [build watermarking]({% post_url 2004-09-22-build-watermarking %}) and [environmentally friendly builds]({% post_url 2004-11-17-environmentally-friendly-configuration %}) before: Some useful techniques to aid support staff in identifying which particular build of your application is causing your end-users -- or testers as is often the case -- grief.

In the first of these two articles, I mentioned a project we did many years ago where we stored the build number in the database. When the application connected, it checked to that it was running against the correct database version. This was prompted by one of our biggest clients at the time who had several hundred users all running a client-server application (yes it was a loooong time ago) and we needed to ensure they couldn't accidentally run the wrong version and potentially corrupt the database.

A similar problem has now cropped up with a rails application we're developing. This time, rather than a specific build, we're more interested in ensuring that the application is running against the correct database instance. In this scenario, we have four databases: `development`; `test`; `uat`; and `production`. Not so much a problem for the first two, but it'd be pretty catastrophic if somehow the UAT application were started against the production database!

Our solution is pretty simple and inline with the previous discussion: put something into the database to identifiy the appropriate environment then check for it in the rails application. The implementation (naturally being rails and all) was also quite simple. We already had a generalised `SystemProperty` class -- essentially just a set of key-value pairs supporting hash-like indexing -- o we merely created a new instance, `:environment`, which is then checked via `before_filter` in the `ApplicationController` like so:

```
class ApplicationController < ActionController::Basebefore_filter :check_rails_env...def check_rails_envraise "Incorrect database: '#{SystemProperty[:environment]}' expected: '#{RAILS_ENV}'" \unless SystemProperty[:environment].value == RAILS_ENVendend
```

Once the code is in place, all you need do is insert a record into the `system_properties` table with a key of `'environment'` and a value of say `'production'` (for, you guessed it the production database) and you're good to go. If the application ever runs against the wrong database, you'll be greeted with something along the lines of:

```
Incorrect database: 'production' expected: 'uat'
```

A slightly riskier alternative to creating the `environment` record manually is to perform the task in a database migration script:

```
class CreateEnvironmentSystemProperty < ActiveRecord::Migrationdef self.upSystemProperty.create! :name => "environment", :value => RAILS_ENVenddef self.downSystemProperty[:environment].destroyendend
```

I can't say I particularly recommend this approach though unless you are REALLY, REALLY certain of your environment settings -- precisely what you probably aren't if you're considering the whole idea in the first place -- but, nevertheless, maybe, like me, you just want to ensure that no one can screw up in the future, say while you're on holidays and you've left it up to the new guy ;-)

The one place where this approach falls down is with caching. Because we've used a before filter, any actions that are cached will be served up directly by the web server, without going through the rails controller. Practically though, I doubt this will be much of an issue, especially in our particular case where what we're really worried about is back-office staff either entering data into the UAT database when they thought it was production -- a bit of a waste of time but not that bad -- or worse, testers entering data into a live system.

Ofcourse this isn't the only problem faced by developers tasked with managing multiple rails environments and I hope to address some more in future posts.
