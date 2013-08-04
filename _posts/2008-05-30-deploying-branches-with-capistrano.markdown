---
layout: post
title: "Deploying branches with Capistrano"
alias: /2008/05/deploying-branches-with-capistrano.html
categories:
---
This morning I had occasion to deploy a branch of a git repository to a staging server but hadn't the foggiest idea how. A quick search through the capistrano source code revealed that I could use `set :branch "branch_name"` in my deploy script. I tried it and it worked. I then figured I would need to make a similar change across all my branches. Of course, I'm a lazy sod and wondered if there wasn't a better way.

If you're not familiar with git, the output of the `git branch` command is a list of branches with an asterisk marking the one currently checked out on your local machine. For example:

``` console
> git branch
* drupal_authentication
fragment_caching
master
```

So, I figured, what if I just parsed the output and searched for the branch marked as current:

``` ruby
set :branch, $1 if `git branch` =~ /\* (\S+)\s/m
```

Now I'm able to deploy whatever branch is current on my local machine from a single, shared, deploy script.
