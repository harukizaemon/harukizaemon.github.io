---
layout: post
title: "Real-estate on Rails"
alias: /2006/04/real-estate-on-rails.html
categories:
---
Our [first Ruby on Rails application](http://www.campbelljonesproperty.com.au) finally went live...almost.</a> Having never even looked at Ruby, let-alone Rails, before embarking on the project, it's been a huge learning experience curve but certainly one worth having.

The site is, as you may have guessed it, a Real-Estate web site for Bowral (and surrounds) in New South Wales -- I don't actually know where that is. The original spec was somewhat of a Design-by-Quark-Express&trade; and although it was excruciantingly precise on look, lacked a little on functionality.

The result is not too bad for a first go -- a spike as [James](http://www.redhillconsulting.com.au/blogs/james) keeps re-assuring me. It's all very Web 2.0ish with its 800x600 borders and AJAX forms, etc. It has scrolling images, dynamic content and layout here and there, server-side emailing and even an RSS feed or two just for fun. Most of it looks great in all major browsers but sometimes we did have to compromise in favour of Internet Explo<strike>d</strike>rer.

Rails really is a lot of fun to work with, most of the time -- indeed the hardest part of the application, besides learning RoR, was the HTML+CSS. The bits that work well, work really, really well: views, routing, configuration, database connections, migrations, plugins, etc. The bits that didn't work so well were the mailer stuff and, sadly, active record.

The mailer stuff smacks of J2EE: layer, upon layer, upon layer. They got some bits right: emails are essential views just like their HTML counterparts. Unfortunately though, the mailer itself (and consequently the email templates) doesn't have access to all the cool controller/helper functionality meaning you end up doing a lot of redundant stuff in a controller just so you can send URL's etc. in emails.

And the of course there's active record which, I must admit, works wonders for the most part. Single-table inheritence sux big fat wobbly things that don't fit down your gullet and thus nearly choke you to death before you manage to cough them up. In the end we implemented an `acts_as_subclass_of` plugin that assumes you have two tables sharing a primary key. Silently failing saves on relationships aren't much fun either. And I have to say that making transaction a part of a record is kinda dumb. The two things have little to do with one another and arbitrarily choosing to use one class over another when updating multiple records seems so un-railsy. Ok so now I'm being petty but it does irk me that they didn't get absolutely EVERYTHING right ;-)

Caching has it's issues as well but we worked around most of the limitations for our needs and it's working a treat. We essentially have pretty close to a fully on-demand website -- include images -- generated from a PostgreSQL database. As parts of the site are accessed, the results are cached, eventually resulting in an almost entirely static website. This "warming up" process could easily be automated on deployment.

We really only used two third-party plugins: [enumerations](http://wiki.rubyonrails.com/rails/pages/Acts+As+Enumerated+Plugin); and [verbose migrations](http://svn.jamisbuck.org/rails-plugins/verbose_migrations/) though we tried a few more but didn't really see the need in the context of this particular application.

Oh yeah, we're using <strike>SwitchTower</strike>[Capistrano](http://manuals.rubyonrails.com/read/chapter/97) making deployment a breeze. Except for the fact that, for some reason, [lighttpd](http://www.lighttpd.net/) doesn't seem to like being re-started from within the script. Not really a problem as the site needs to migrate to Apache 2.0 + FCGI sometime soon anyway to fit in with the customer's site management tools but annoying.

Database migrations work fairly well as long as you don't change your model too much. Pretty much anytime you delete a model class, you need to make a copy of it inside your migration script(s) so they can use it if/when you deploy a brand new application. Somewhat annoying to say the least but I don't have a better solution, save laboriously checking out each successive "version" of the application one at a time and running migrate. Suggestions anyone?

Still on the database front, I also patched some of the PostgreSQL driver code to fix some bugs (I think I reported them) and to add in missing features such as foreign-key constraints, column check constraints and [VACUUM](http://www.postgresql.org/docs/8.1/interactive/sql-vacuum.html). I know everyone loves MySQL and you're all probably aware that I don't but, prejudices aside, you have to love a database that allows you to back-up using:

{% highlight console %}
> pg_dump -o -O cjp_production | gzip > cjp_production.gz
{% endhighlight %}

Development was done (for the most part) using [TextMate](http://macromates.com/)+Subversion on our lovely PowerBooks (yes, we're so December 2005) and deployed to a FreeBSD box which, I have to say has a very nice package management system. (I always liked FreeBSD back in the version 2/3 days and I like it even more now. It certainly feels like a great platform for production environments.) Virtual PC also came in very handy as for running Windoze in order to test the site.

I think I only used rails' `generate` script once to see what it would produce -- lot's of noise -- after which I chose to hand-cruft my code. I suspect I'm a control-freak at heart ;-).

The real winner for me though is Ruby. It's just nice. It too has it's foibles but I really do find I write less verbose (and certainly less syntactic) code. It has already started to change the way I think about code and I like where I'm headed. It'll be interesting to see how much it screws up my Java code ;-)

All-in all an very pleasant experience. I'm not a huge fan of web apps in general but Rails certainly makes them much more fun and the rise of DHTML for client-side processing definitely makes for far more interesting client-side applications. I could certainly enjoy making a living this way.

So, anyway, if you've ever been interested in owning a property with your own 2-acres of truffles, I know [just the place](http://www.campbelljonesproperty.com.au/buy/property_listing/141/main) ;-)
