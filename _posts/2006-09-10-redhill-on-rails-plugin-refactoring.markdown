---
layout: post
title: "RedHill on Rails Plugin Refactoring"
alias: /2006/09/redhill-on-rails-plugin-refactoring.html
categories:
---
I mentioned in my [previous entry]({% post_url 2006-09-07-foreign-key-associations-plugin %}) that I'd done quite a bit of refactoring of [the plugins](https://github.com/harukizaemon/redhillonrails). Among the various changes that will affect developers using them are:

* Schema Defining (`schema_defining`) has been deleted;
* Foreign Key Support (`foreign_key_support`) has been deleted; and
* RedHill on Rails Core (`redhillonrails_core`) has been added to replace the previous two as well as subsuming some of the more generic functionality from other plugins.

So, why all these changes?

The main reason is manageability. We're actually eating our own dog food and using these plugins in production applications and we're adding functionality at quite a surprising rate. Each time we add something, we first put it into the plugin that needs it directly. That works great for a while but then, someday, we decide we need that functionality in two or more plugins. What to do?

Our original idea had been to create new plugins and this worked for us up to a point. Unfortunately, of late, the number of extra plugins -- with very specific functionality mind you -- was just getting out of hand and needed to be simplified.

In the end, we decided on a two-tiered approach to plugins: those which add functionality but no (or at least minimal) behaviour; and those that add behavioural magic.

As an example, the new core plugin adds functionality to manage foreign keys, lookup indexes, add unique column meta-data, etc. but doesn't do anything particularly magic that will affect the running of your application.

On the other hand, the foreign key migrations, foreign key associations, schema validations, etc. plugins -- which all rely on core -- add funky rails magic to automatically generate foreign keys, associations, model validation, etc.

Another change we made was in the way documentation is generated. We used to manually generate a nice HTML file containing all the plugins. This was becoming rather tedious and meant that the documentation was often quite out of date. We've now remedied this with a nice ruby script using Erb and RDoc to generate the online documentation directly from the `README` files.

I also mentioned previously that we've added "lots" of tests. I say lots because we're still playing catchup so relatively, there are lots but we still need lots more. As a group of developers that are ardent TDD evangelists, the conspicuous lack of tests was somewhat embarrassing to say the least. Unfortunately, testing plugins (especially those related to schema and database) is pretty difficult so we opted to bypass the whole problem and just create a standard rails app with standard rails tests and all is well again.

And lastly, besides all the extrat features we've added (see the `CHANGELOG`s for the specific plugins), you'll notice that the subversion URL has changed slightly -- it used to contain an extra slash (/) which was not only unnecessary but caused SVN to regularly crap out.

My aplogies to all those that have been trying to keep up but we hope that's the last of it. From now on, we'll continue to beef up core as we need and then add plugins only when we need new behaviour.

Of course we'll always reserve the right to change our minds ;-)
