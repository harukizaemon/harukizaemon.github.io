---
layout: post
title: "Row Version Migrations"
alias: /2006/06/row-version-migrations.html
categories:
---
The more I use Rails, the more plugins I seem to create. Each time I start a new project, almost the first thing I find I want to do is to copy a chunk of code from an existing project. The moment that happens, I think: Plugin!

So [here](https://github.com/harukizaemon/redhillonrails/tree/master/row_version_migrations) it is. A plugin that automatically generates the following row version columns for every table:

{% highlight ruby %}
:created_at,   :datetime,  :null => false
{% endhighlight %}

Not so difficult. In fact it seems almost trivial (as usual) but it means all my tables now get date/time stamps and optimistic locking for free.

Enjoy. As always, feedback, suggestions, patches, criticism, all welcome.
