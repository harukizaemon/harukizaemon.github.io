---
layout: post
title: "Zip and Preserve File Permissions with Ant"
alias: /2006/10/zip-and-preserve-file-permissions-with.html
categories:
---
Yes, it's been a while since I posted an entry related to Java! Believe it or not, we still do Java development, lots of it in fact, but it's mostly large-scale re-factoring and cleanup work on what can best be described as "legacy" applications so there's rarely much if anything to write home about. That said, I have a couple of posts just itching to be written when I find some time. Until then, a relatively short entry will have to do :)

 A client distributes one particular Java-based web application to hundreds of customers using a zip file. The distribution contains, among other things, the war file and some scripts for database migration, etc. It's these scripts that cause us some headaches as they need to have execute permission. The problem arises because Ant's built-in zip task specifically doesn't handle file permissions. So, naturally, we concocted our own using `macrodef`:

{% highlight xml %}
<macrodef name="zipdir">
  <attribute name="destfile"/>
  <attribute name="sourcedir"/>
  <echo>Building zip: @{destfile}</echo>
  <exec executable="zip" dir="@{sourcedir}">
    <arg value="-qR"/>
    <arg value="@{destfile}"/>
    <arg value="*"/>
    <arg value="-x *.svn* "/>
  </exec>
</macrodef>
{% endhighlight %}

This simply calls the operating system's -- read *nix -- `zip` command to compress the specified directory thus preserving all the file permissions that SVN lovingly maintains.
