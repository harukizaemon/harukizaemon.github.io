---
layout: post
title: "Installing RedHill on Rails Plugins via HTTP"
alias: /2007/03/installing-redhill-on-rails-plugins-via.html
categories:
---
After much installation pain and anguish experienced by some users of our plugins (due mostly to outages with RubyForge SVN servers and the inability of some users to access SVN from behind firewalls), I've finally managed to spend some time getting a plugin installer-friendly HTTP mirror up and running.

The root of the mirror can be found at [http://www.redhillonrails.org/svn](http://www.redhillonrails.org/svn) from which you can browse the entire repository.

If you live on the edge, you'll find them at [http://www.redhillonrails.org/svn/trunk/vendor/plugins](http://www.redhillonrails.org/svn/trunk/vendor/plugins/).

If you're after are the Rails 1.2 stable compatible plugins, you'll find them at [http://www.redhillonrails.org/svn/branches/stable-1.2/vendor/plugins](http://www.redhillonrails.org/svn/branches/stable-1.2/vendor/plugins/).

If you're after the older Rails 1.1.6 versions, they're available at [http://www.redhillonrails.org/svn/tags/release-1.1.6/vendor/plugins](http://www.redhillonrails.org/svn/tags/release-1.1.6/vendor/plugins/).

The mirror is presently refreshed once a day which should be enough for most people.

As always, let me know if I've right royally screwed something up. I've manually tested all of the above but "it works on my machine" definitely applies in the case I'm sure.
