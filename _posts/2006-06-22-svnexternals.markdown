---
layout: post
title: "svn:externals"
alias: /2006/06/svnexternals.html
categories:
---
[This relatively unknown feature](http://svnbook.red-bean.com/en/1.2/svn.advanced.externals.html) of [subversion](http://subversion.tigris.org/) allows you to include one subversion repository URL inside another. By that I don't mean export and add, rather `svn:externals` allows you to create a virtual directory that links to an external repository.

I'm sure there are plenty of other uses but I mostly use `svn:externals` for adding plugins to my rails projects. For example, if you wanted to link to the [Foreign Key Migrations](https://github.com/harukizaemon/redhillonrails/tree/master/foreign_key_migrations) plugin, from inside your rails project directory you would type:

```
simon$ svn propedit svn:externals vendor/plugins/
```

Then add the following lines:

```
foreign_key_migrations svn://rubyforge.org//var/svn/redhillonrails/trunk/vendor/plugins/foreign_key_migrationsschema_defining svn://rubyforge.org//var/svn/redhillonrails/trunk/vendor/plugins/schema_defining
```

And voila! Now when you update your project you'll see something like:

```
simon$ svn upFetching external item into 'vendor/plugins/foreign_key_migrations'External at revision 25.Fetching external item into 'vendor/plugins/schema_defining'External at revision 25.At revision 2533.simon$
```

Notice how subversion updates the virtual directories as well.

Be aware though that this does mean you'll always get the latest code when you update. This is exactly what I want but may not suitable on all projects. Sometimes a simple `svn export` is more appropriate.

We're currently toying with the possibility of using `svn:externals` to share common code between some projects that are presently all in one big code-soup project but will be split up over the next couple of months. I'll let you know what we decide to do in the end.
