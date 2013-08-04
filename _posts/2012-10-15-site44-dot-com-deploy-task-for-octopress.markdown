---
layout: post
title: "Site44.com deploy task for Octopress"
date: 2012-10-15 12:53
categories:
---
Further to my [previous post]({% post_url 2012-10-15-generate-a-site44-dot-com-redirects-file-for-your-octopress-blog %}), I also added a rake task to my `Rakefile` that uses rsync to deploy:

{% highlight ruby %}
rsync_delete   = false
deploy_default = "local"

# snip

desc "Deploy website via local rsync"
task :local do
  exclude = ""
  if File.exists?('./rsync-exclude')
    exclude = "--exclude-from '#{File.expand_path('./rsync-exclude')}'"
  end
  puts "## Deploying website via local rsync"
  ok_failed system("rsync -avz #{exclude} #{"--delete" unless rsync_delete == false} #{public_dir}/ #{deploy_dir}")
end
{% endhighlight %}

I initally thought it would be pretty neat to work out the local website location in Dropbox but in the end I decided it was simpler to just leave the `deploy_dir` variable set to the default `"_deploy"` and have that directory symlinked to the approproprate Dropbox folder.
