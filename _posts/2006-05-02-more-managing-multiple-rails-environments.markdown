---
layout: post
title: "More Managing Multiple Rails Environments"
alias: /2006/05/more-managing-multiple-rails.html
categories:
---
[Yesterday](/blog/2006/05/01/managing-multiple-rails-environments) I discussed how we made sure our application was talking to the correct database. Today I'm going to show you how we provide some visual feedback to the user indicating in which environment they're operating.

As mentioned yesterday, we have four different environments, two of which -- UAT and production -- are available to the end users and as such it's imperitive that we provide some visible means for discrimination (lest they mistake one for the other and accidentally add test data into production). Besides obvious gating procedures such as different access rights, etc. one of the ways we can help them out is by providing some obvious visual feedback to indicate the current environment. In this scenario, we decided to change the background colour from gold in production to red in UAT.

One approach might have been to hard-code some style information into the `head` element of the main `application.rhtml` template. Something along the lines of:

```
&lt;% if RAILS_ENV == "uat" -%&gt&lt;style type="text/css"&gt;body {background-color: red;}&lt;/style&gt;&lt;% end -%&gt
```

The problem with this approach is that, well quite frankly, it smells. For a start, we've hard-`d the environment and placed style information directly into each web page -- something I'm always reluctant to do if I can avoid it. So what other options do we have?

I'm glad you asked. It turns out that we already had a mechanism in place for including various stylesheets depending on which page is being rendered. For a start, we always include `application.css` on every page. It contains all the global, system-wide styling information. In addition, we also include a stylesheet for each page -- if it exists -- based on the path to the current action. So for example, if the current action is `list` in the `CustomerController` then we check to see if the stylesheet `customer/list.css` exists and if so, include it in the rendered output. This allows us to easily add specific styling for individual pages if necessary.

The code for the macro that handles this behaviour looks something like:

```
def stylesheet_link_tag(*sources)if sources.include? :defaultssources = sources.dupsources.delete :defaultssources &lt;&lt; 'application' if File.exists?("#{RAILS_ROOT}/public/stylesheets/application.css")page = "#{@controller.controller_name}/#{@controller.action_name}"sources &lt;&lt; page if File.exists?("#{RAILS_ROOT}/public/stylesheets/#{page}.css")endsuper *sourcesend
```

So if you call the `stylesheet_link_tag` macro with `:defaults` as one of the arguments, magic happens.

With this in mind, we decided to enhance the default stylesheets list to include `#{RAILS_ENV}.css` if it exists (with a little DRY re-factoring in the process):

```
def stylesheet_link_tag(*sources)if sources.include? :defaultssources = sources.dupsources.delete :defaults['application', **RAILS_ENV**, "#{@controller.controller_name}/#{@controller.action_name}"].each do |source|sources &lt;&lt; source if File.exists?("#{RAILS_ROOT}/public/stylesheets/#{source}.css")endendsuper *sourcesend
```

Now we can easily create a `uat.css` that looks something like:

```
body {background: #EB5E66;}
```

And voila! When users log in to production they see the default (gold) background and in UAT they're immediately hit with a red -- technically it's 'cherry' -- one. No mistaking which environment they're in now.

Again, you don't have to use a background colour nor even use stylesheets for that matter. The main point is that getting configuration management (of which we consider this to be a part) takes some thought but isn't that hard to get right, especially in Rails.
