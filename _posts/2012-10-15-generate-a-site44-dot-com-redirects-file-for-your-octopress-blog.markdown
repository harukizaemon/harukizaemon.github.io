---
layout: post
title: "Generate a site44.com redirects file for your Octopress blog"
date: 2012-10-15 09:51
categories:
---
Over the weekend I converted my, albeit languishing blog from straight [Jekyll](http://jekyllrb.com) on [GitHub Pages](http://pages.github.com) to use [Octopress](http://octopress.org) served via [Site44](http://www.site44.com).

In the process I also took the opportunity to switch the URLs I was using for blog entries to the Octopress default. This in turn left me needing a bunch of redirects.

Site44 supports redirects via a [well-known text file](http://www.site44.com/advanced) in the root of your website. The text file provides a mapping between source and destination paths. That's all very well and good but I didn't much feel like creating 300+ redirect mappings by hand. Besides the tedious nature of the task, the chances I was going to screw one or more of them up in the process were fairly high.

Thankfully, due to a previous blog move, I happened to have a bunch of `permalink` definitions in the front-matter of most of my blog entries. All I really needed then was a way to turn those into said text file.

A quick Google turned up the [Alias Generator](http://github.com/tsmango/jekyll_alias_generator) plugin for Octopress by [Thomas Mango](http://thomasmango.com) which was very close to, but not quite what I needed.

Hack hack hack on the plugin, a quick rename of all the `permalink` attributes in my posts to `alias`, and voila! a new plugin that generates a `redirects.site44.txt` with all my redirects:

{% highlight ruby %}
# Site 44 Redirects Text Generator for Posts.
#
# Generates a www.site44.com compatible redirects file pages for posts with aliases set in the YAML Front Matter.
#
# Place the full path of the alias (place to redirect from) inside the
# destination post's YAML Front Matter. One or more aliases may be given.
#
# Example Post Configuration:
#
#   ---
#     layout: post
#     title: "How I Keep Limited Pressing Running"
#     alias: /post/6301645915/how-i-keep-limited-pressing-running/index.html
#   ---
#
# Example Post Configuration:
#
#   ---
#     layout: post
#     title: "How I Keep Limited Pressing Running"
#     alias: [/first-alias/index.html, /second-alias/index.html]
#   ---
#
# Author: Simon Harris
# Site: http://harukizaemon.com

module Jekyll
  REDIRECTS_SITE44_TXT_FILE_NAME = "redirects.site44.txt"

  class RedirectsSite44TxtFile < StaticFile
    def write(dest)
      begin
        super(dest)
      rescue
      end

      true
    end
  end

  class RedirectsSite44TxtGenerator < Generator
    def generate(site)
      unless File.exists?(site.dest)
        FileUtils.mkdir_p(site.dest)
      end

      File.open(File.join(site.dest, REDIRECTS_SITE44_TXT_FILE_NAME), "w") do |file|
        process_posts(site, file)
        process_pages(site, file)
      end

      site.static_files << Jekyll::RedirectsSite44TxtFile.new(site, site.dest, "/", REDIRECTS_SITE44_TXT_FILE_NAME)
    end

    def process_posts(site, file)
      site.posts.each do |post|
        generate_aliases(file, post.url, post.data['alias'])
      end
    end

    def process_pages(site, file)
      site.pages.each do |page|
        generate_aliases(file, page.destination('').gsub(/index\.(html|htm)$/, ''), page.data['alias'])
      end
    end

    def generate_aliases(file, destination_path, aliases)
      Array(aliases).compact.each do |alias_path|
        file.puts("#{alias_path} #{destination_path}")
      end
    end
  end
end
{% endhighlight %}
