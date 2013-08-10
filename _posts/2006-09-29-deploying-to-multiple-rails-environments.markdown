---
layout: post
title: "Deploying to Multiple Rails Environments"
alias: /2006/09/deploying-to-multiple-rails-environments.html
categories:
---
On one Rails project, we have two deployment environments: production; and UAT. Using the default Capistrano configuration makes deploying to these two environments rather difficult so, I thought I'd share our `deploy.rb` with a bit of explanation along the way. Ok, here goes:

For a start, we deploy to a directory that includes the environment as part of the path:

```
set :deploy_to, lambda { "/home/#{user}/www/#{rails_env}" }
```

For subversion, we checkout the code as the user who is running the deployment making sure not to cache authentication details on the server:

```
set :svn_user, ENV['USER']set :svn_password, lambda { Capistrano::CLI.password_prompt('SVN Password: ') }set :repository, lambda { " -- username #{svn_user} --password #{svn_password} --no-auth-cache svnurl/trunk/#{application}" }
```

In both cases, we run a mongrel cluster. Because the mongrel configuration files share a lot in common and because they largely duplicate information contained within the deployment script, we generate an appropriate configuration on deployment. More of that in a bit but for now, the common bits look like:

```
set :mongrel_address, "127.0.0.1"set :mongrel_environment, lambda { rails_env }set :mongrel_conf, lambda { "#{current_path}/config/mongrel_cluster.yml" }
```

Now, for the environment specific portions. For each environment we have a task that simply sets variables appropriately -- I toyed with using an environment variable such as `RAILS_ENV` rather than the pseudo-tasks but it was more typing and I'm allergic to typing :).

For production, we want 3 mongrel instances in the cluster, listening on ports 8000-8002:

```
desc "Production specific setup"task :production doset :rails_env, :productionset :mongrel_servers, 3set :mongrel_port, 8000end
```

For UAT, we want 2 mongrel instances in the cluster, listening on ports 8010-8011:

```
desc "UAT specific setup"task :uat doset :rails_env, :uatset :mongrel_servers, 2set :mongrel_port, 8010end
```

And finally, a custom deployment script based almost entirely on the built-in `deploy_with_migrations` with the major difference being the configuration of the mongrel cluster just prior to `restart`:

```
desc "Generic deployment"task :deploy doupdate_`beginold_migrate_target = migrate_targetset :migrate_target, :latestmigrateensureset :migrate_target, old_migrate_targetendsymlink**configure_mongrel_cluster**restartend
```

That's it really. Now whenever we need to deploy to a particular environment, say for example UAT, we do something like:

```
cap uat deploy
```

**Update**

By request, here is our `database.yml` file :

```
common: &commonadapter: postgresqlusername: &lt%= ENV['USER'] %&gt;development:database: foo_development&lt;&lt;: *commontest:database: foo_test&lt;&lt;: *commonuat:database: foo_uat&lt;&lt;: *commonproduction:database: foo_production&lt;&lt;: *common
```

As you can probably tell, we're lucky enough that the database user is always the same as the user under which the application will be run and is that the database itself is named according to the environment. That makes it very easy to wrap up most of the common parts -- Thanks goes to [Jon Tirsen](http://jutopia.tirsen.com/) for that YAML tip.

This could also easily be generated. I guess it just hasn't needed any attention since it was created so YAGNI overrode DRY ;-)
