---
layout: post
title: "Monitoring Java Processes under Mac OS X"
alias: /2007/03/monitoring-java-processes-under-mac-os.html
categories:
---
**Update:** After a bit of googling I discovered that it's a [bug](http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=6332311) which is fixed in Java 6 (Mustang) and relates to, wait for it, user names containing an underscore. Go figure!

So today, after I don't know how many years of doing Java development, I find out there is [a command to list all the running Java processes](http://java.sun.com/j2se/1.5.0/docs/tooldocs/share/jps.html). It's available on all platforms I could find and it's called `jps`. On most platforms it'll display something like:

```
1170 Jps -lm1162 org.apache.tools.ant.launch.Launcher -cp  dist
```

Except under Mac OS X where you'll get something more like:

```
1170 Jps -lm1162 -- process information unavailable
```

Pretty bloody useless!

So, I wrote a little ruby script to simulate the desired behaviour as much as possible (by using the built-in `jps` to collect the process ids; then filtering the output from `ps` accordingly):

```
#!/usr/bin/env rubyrequire 'set'pids = Set.new`#{ENV['JAVA_HOME']}/bin/jps`.each_line do |line|pids.add($1) if line =~ /^(\d+) /end`ps -x -w -w -o pid,command`.each_line do |line|print line if line =~ /^ *(\d+) / && pids.include?($1)end
```

Which, when run, produces output similar to (line continuation not included):

```
1162 /System/Library/Frameworks/JavaVM.framework/Versions/CurrentJDK/Home/bin/java \-classpath /opt/local/share/java/apache-ant/lib/ant-launcher.jar \-Dant.home=/opt/local/share/java/apache-ant \-Dant.library.dir=/opt/local/share/java/apache-ant/lib \org.apache.tools.ant.launch.Launcher -cp  dist
```

Granted it does produce lots more output than standard `jps` but it does at least produce something useful to look at when monitoring Java processes. Best of all, it means we can automate the monitoring in a manner that works across most platforms.

If anyone has a Better Way&trade;, I'm all ears.
