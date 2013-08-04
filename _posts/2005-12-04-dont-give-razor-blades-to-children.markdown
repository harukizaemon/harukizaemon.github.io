---
layout: post
title: "Don't Give Razor-Blades to Children"
alias: /2005/12/don-give-razor-blades-to-children.html
categories:
---
I've no idea who first coined this phrase but I heard it from [Steve](http://www.jroller.com/page/sxh).

This was to be solely about Rails schema migration but it turned into a partial rant (surprise surprise) about playing with Ruby and Rails. Never fear, the migration bits are still here; they're just at the end instead :)

Developing in Rails as a one- or two-man-band is very rewarding but I'd like to see how it scales to many developers and more importantly, to developers of radically different levels of experience. As you probably gathered from my [previous post]({% post_url 2005-11-26-ruby-on-rails-100-000-morons-cant-be-wrong %}), my prognosis on this last point isn't particularly flattering.

I like convention over configuration so long as the conventions don't get in my way; and so far they haven't. I think the idea of "flexibility" in software is highly overrated. The whole J2EE stack is a perfect example of bloat in an attempt at being all things to all people.

Don't get me wrong, I love Java. I will always like it. It does pretty much everything I could want and do feel very productive with it -- especially when compared with the pile of steaming turd that is [Flash/Flex](http://www.macromedia.com/software/flex/) -- but I have to admit that I do like The Ruby Way &trade;. I must have been a Smalltalk wanna-be in a previous life. It's not for all things nor for all people but it might just be for me on the kinds of projects I enjoy doing.

So far I've found Ruby/Rails to be easy to test; that it facilitates adding new behaviour VERY quickly; and that the teeny bits of SQL I do need every now and then aren't painful in the slightest. I love partials; caching; declarative relationships; validation; acts_as_xxx; components; etc.

I don't like fact that there doesn't seem to be a way for components to have relationships to domain objects -- for example my address component needs a State which is an active record object. Tool support is pre-historic when compared with Java. I use TextMate on the Mac which is pretty darn good and there is RadRails of course but to be honest, right now -- although the road-map looks very promising-- it isn't actually much better than TextMate except for debugging.

Anyway, the upshot of that little rant is to say that whilst I'm still splashing around in the kiddies pool, the water still feels good and is tempting me to want to try swimming in deeper water.

So anyway, back to the topic I originally had in my head.

If anyone has been playing with rails then at some point they'll want to have a play with data/schema migration. I used it tonight and it worked as advertised.

In short, you create a number of files named `001_xxx`, `002_xxx`, etc. Rails then creates a table (`schema_info`) that contains the current database schema version. When you run a migrate, it (rails) will execute the scripts necessary to bring you from whatever version you are currently at (may be 0) to the latest version (by default) or a nominated version (using `VERSION=#`). You can even downgrade by specifying a previous version.

Besides migrations based on version numbers it essentially supports a DB independent  schema description with the ability to escape to SQL if need be (like for those pesky little FK constraints that no one wants to talk about but should be locked-up for doing without). Honestly the "DB independence" thing isn't the biggest concern for me in the world but it is nice. It would be even nicer if rails extended the concept to include foreign key constraints (the model has declarative relationships anyway!).

You can also use your rich domain model to play with the new schema in the migration script too! So, for example, you can add reference data or munge the existing records to suit the new schema. Again you can even execute raw SQL if need be!

If you're interesting in how this _really_ works, I can recommending reading [this](http://wiki.rubyonrails.org/rails/pages/UnderstandingMigrations)and maybe [this](http://glu.ttono.us/articles/2005/10/27/the-joy-of-migrations)and possibly [this](http://www.robbyonrails.com/articles/2005/11/11/rails-migrations-and-postgresql-constraints)and the RDOC can be found [here](http://api.rubyonrails.com/classes/ActiveRecord/Migration.html).

The only issues I've had are that transaction management within the migration is a little dubious: I can't seem to find a way to execute a batch of DML -- as opposed to DDL-- in a single transaction. Rather, it seems to commit each statement. The upshot of this is that it's possible to end up with a database in a weird state where some of the statements executed but the schema version hasn't been incremented. Thankfully you do get an error message so you know something screwed up and you can manually increment the version number and then have rails downgrade for you.

This only happened to me once and then only in development so it's not a huge issue but one to be mindful of.

So, my new shiny toy is still shiny but I'm not about to give up my day job just yet. I still haven't heard enough people bitching about it to make me feel comfortable that there is enough understanding of the technology in the main. I am however doing a real-life project using it and so far so good.
