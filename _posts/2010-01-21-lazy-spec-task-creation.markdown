---
layout: post
title: "Lazy spec task creation"
alias: /2010/01/lazy-spec-task-creation.html
categories:
---
I converted a Ruby project over to use [Bundler](http://github.com/wycats/bundler) for gem dependency management today. For the most part it worked flawlessly except, that is, when the CI build ran for the first time after the conversion:

``` console
LoadError: no such file to load -- vendor/gems/environment

Stacktrace:
tasks/spec.rb:8:in `require'
tasks/spec.rb:8:in `<top (required)>'
...
Rake aborted!
```

The short story: The spec task definition needed the RSpec gem to be loaded but it wasn't until _after_ all tasks had been defined.

Now, I could single out the spec task definition and ensure it was loaded last but that would mean adding a bunch of code to my otherwise trivial `Rakefile`. The other option was to somehow defer the creation of the spec task until actually needed. After a bit of searching I couldn't find anything particularly useful so I rolled my own:

``` ruby
namespace :spec do

  desc "Run specifications"
  task :run => :define do
    Rake::Task[:_run].invoke
  end

  task :define do

    require 'spec/rake/spectask'

    Spec::Rake::SpecTask.new(:_run) do |t|
      t.spec_opts << "--options" << "spec/spec.opts" if File.exists?("spec/spec.opts")
    end

  end

end
```

The `spec:run` task depends on the `spec:define` task to create a "hidden" `spec:_run` task to actually do the work.

The nice thing about this all the ickiness is hidden -- remove the `tasks/spec.rb` file and nothing else really cares -- and means I can treat the spec task like any other when creating my task dependencies.

As always, YMMV.
