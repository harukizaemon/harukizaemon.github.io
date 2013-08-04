---
layout: post
title: "Schema Validations Plugin"
alias: /2006/07/schema-validations-plugin.html
categories:
---
After listening to Prag Dave's [Keynote Speech](http://blog.scribestudio.com/articles/2006/06/30/railsconf-2006-keynote-series-dave-thomas) this afternoon, I was motivated to implement some of the things he'd been asking for. [Here's](https://github.com/harukizaemon/redhillonrails/tree/master/schema_validations) my first cut at it.

As the doco says, the plugin reads some -- ok only one at the moment but we'll see how many others I get done before the beer runs out -- database column constraints and tries to apply the closest corresponding rails validation. The first one I implemented reads the `NOT NULL` constraints against columns and generates a corresponding `validates_presence_of`.

I literally just whipped it up with no tests or what-not and I've only played with it against PostgreSQL, so if it has bugs or behaves oddly for whatever reason, please let me know, send me as much info as possible and I'll make it work. Nothing better than having real people testing it for me ;-)

**Update:** Ok, so far the beer has lasted long enough to implement validation of numbers (including specific support for integers) and lengths of strings.

**Update:** Now calls `validates_presence_of` anytime you declare a `belongs_to` association for a `NOT NULL` foreign-key column.

**Update:** Single-column unique indexes are now converted to `validates_uniqueness_of`.
