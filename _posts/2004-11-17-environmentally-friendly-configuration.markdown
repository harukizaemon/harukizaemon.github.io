---
layout: post
title: "Environmentally-Friendly Configuration"
alias: /2004/11/environmentally-friendly-configuration.html
categories:
---
[Dave](http://www.redhillconsulting.com.au/blogs/david) came over to me the other day and said he wanted a new build check written that searches the entire source base for `"C:"`. It's a familiar problem where supposedly automated scripts refer to specific directories, paths, etc. Move the script, run it in another environment, and BLAMMO!

I recall some colleagues telling me of a problem they had once where the system had been running fine for months in development and then one day it mysteriously started failing, everytime, on every developers machine. It turned out that ALL developer machines had been configured to communicate with the message queue on another developers machine because that's what had been checked in to CVS.

More recently, we encountered a problem where the UAT environment worked but not System Test. Again, it turned out that the default configuration for a remote URL was, lo and behold, set for use in a UAT environment. Each time the System Test deployment was run, it was communicating with the wrong server. No one noticed it at first because, aside from the server name, the string of request params is the same in all cases.

These days, as a bare minimum, we strive to have ALL "default" configuration values set to something along the lines of `"THIS_IS_WHERE_THE_VALUE_OF_X_NEEDS_TO_GO"`. This way it sticks out like the proverbial Dogs Bits when you've forgotten to tailor something for a specific environment.

Then we use build properties such as `configuration=development`, `configuration=uat`, etc. specified on the command-line that allow the build scripts to substitute in all the various values appropriate for the intended target environment. This, coupled with some [Build Watermarking]({% post_url 2004-09-22-build-watermarking %}), almost guarantees that this class of configuration problem is a thing of the past.

That is of course until one of the developers comes to you and proudly explains that they have _"discovered the problem. There were these strange values in the reference data scripts, so I changed them all to sensible defaults and checked it in."_
