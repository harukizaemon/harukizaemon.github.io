---
layout: post
title: "Continuous Integration for Rails Just Got A Whole Lot Easier"
alias: /2007/03/continuous-integration-for-rails-just.html
categories:
---
If you already have a continuous integration (ie build) server (such as [Pulse](http://www.zutubi.com/products/pulse/), [Bamboo](http://www.atlassian.com/software/bamboo/), [Hudson](https://hudson.dev.java.net/), or even the venerable old [CruiseControl](http://cruisecontrol.sourceforge.net/)) and you've been trying to include some of your Rails applications then your life just got a whole lot easier.

Enter [CI::Reporter](http://caldersphere.rubyforge.org/ci_reporter/). To quote the documentation:

> ...an add-on to Test::Unit and RSpec that allows you to generate XML reports of your test and/or spec runs. The resulting files can be read by a continuous integration system that understands Ant‘s JUnit report XML format, thus allowing your CI system to track test/spec successes and failures.

So, I simply installed the gem on the build-server and the plugin into my [plugins project](https://github.com/harukizaemon/redhillonrails/tree/master/$1), configured the build to slurp up the generated XML files and voila! I now get build notifications letting me know how many tests passed and which ones failed.

Thanks [Nick!](http://blog.nicksieger.com/)
