---
layout: post
title: "Prevent accidental deployment with a prompt"
alias: /2008/06/prevent-accidental-deployment-with.html
categories:
---
This morning I went to push out a new version of an application to our staging environment on an Engine Yard slice. I knew I had done exactly that last night so I navigated through my bash history and hit enter. Two minutes later the new version had been deployed and I was about to walk out the door to do some chores before coming back to start playing with the app. Thankfully, [Steve](htp://steve.cogent.co/) came online and informed me that production was broken.

A quick look through my bash history and it seemed I'd used the deploy to production rather than deploy to staging but, being in a hurry, hadn't looked carefully enough. Of course some might argue that I should have looked more carefully. That I shouldn't have deployed before heading out. All valid points but I very rarely have full control over what's going on around me. So, while it's all very well and good to hope that I will be more careful next time, that's a bit like hoping global warming isn't a reality: we all hope it's not but maybe we should do something about it just in case?

And so I added the following at the start of the `:production` task in my `deploy.rb` file:

{% highlight ruby %}
unless Capistrano::CLI.ui.agree("Are you sure you want to deploy to production? (yes/no): ")
  puts "Phew! That was a close call."
  exit
end
{% endhighlight %}

For all other environments, the deployment goes through without question. Attempt to deploy to production however, and I'm now forced to be explicit about my intentions.
