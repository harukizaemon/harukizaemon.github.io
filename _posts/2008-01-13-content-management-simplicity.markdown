---
layout: post
title: "Content Management Simplicity"
alias: /2008/01/content-management-simplicity.html
categories:
---
I've been wanting to put together a new website for my [Aikido School](http://www.iwamaaikido.com.au/) for some time now and had been looking around for possible tools to help out. Possible choices included: continuing to maintain it by hand with a redesign; use some kind of site generation tool; write a Rails app; or find a CMS tool.

The first wasn't really an option -- I'm time poor as it is. The second didn't seem like a much better solution either to be honest. And If the first two didn't seem like attractive solutions the third was possibly even worse. So, it came to finding a CMS tool and after a little investigation, googling, reading forums and talking with friends, colleagues, ISPs and a few drinks I had narrowed the field down to two: [Joomla](http://www.joomla.org/); or [Drupal](http://www.drupal.org/).

(Interestingly, both are built on Perl/PHP -- quite frankly the idea of a Java-based CMS running inside a J2EE container makes me feel ill just thinking about it and I couldn't find any Ruby-based solutions that even rate a mention.)

Both Joomla and Drupal are solid, mature products. Both are open source and have a thriving community. Both support extensions/plugins/modules and both have had books written about them. All good things. But in the end I settled on Drupal for a number of reasons some emotive, some based on other's experience and some technical -- perhaps the fact that Joomla only runs on MySQL could be considered both technical AND emotive.

So far, I'm highly impressed. It was very easy to install and setup. The admin screens even tell me when the underlying OS or database isn't configured properly and even go as far as telling me how to fix the problem! There are a bewildering number of modules available that do almost everything you can imagine. And when I finally wanted to do something outside the "box" -- aggregating images based on taxonomies and displayed in the side navigation of the home page -- it was almost trivial to programatically construct the content. My distinct impression is that for many sites I've traditionally built as yet another new application, a CMS like Drupal plus some custom code would be sufficient.

I'm still not a huge fan of Perl. I like many of the language features such as built in regular expressions, etc. making it ideal for command-line scripting, but it still feels a little too close to the metal for me and lacks much of the syntactic sugar around objects that make languages like Ruby more attractive. That said, I've thus far only needed to write custom PHP code once and I reckon it would make a nice module -- assuming I could find the time to publish yet another open source project destined to become abandonware ;-)

If only someone in the Ruby community would put something together that's as good as these two, I'd consider switching. Perhaps something built around [merb](http://merbivore.com/) however maturity and a strong community are certainly big selling points for me, perhaps making it difficult for a new player to gain traction?
