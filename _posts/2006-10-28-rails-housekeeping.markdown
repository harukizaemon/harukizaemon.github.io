---
layout: post
title: "Rails Housekeeping"
alias: /2006/10/rails-housekeeping.html
categories:
---
Since moving from lighttpd+FasCGI to Apache 2.2+mongrel our production rails application has been rock solid -- one unexplained ruby core-dump notwithstanding.

To keep everything humming along, we run a few cron jobs which I thought I'd share.

The first is to ensure the application starts on boot. There is an rc-script to do this but I never bothered to get it running on FreeBSD. Instead, we use the `@reboot` keyword built into vixie-cron:

```
cd ~/www/production/current; mongrel_rails cluster::stop; mongrel_rails cluster::start
```

Next, session expiration. Even though plenty have argued against them in favour of memcached, we've found file-system sessions to be just fine for our, relatively low traffic, application. To keep timeout sessions after one hour -- with a margin of error of an extra hour -- we run an hourly cron job to delete session files that haven't been updated since it last ran:

```
cd ~/www/production/current; find tmp/sessions -name 'ruby_sess.*' -amin +60 -exec rm -rf {} \;
```

Next, to keep log file sizes manageable, we run a cron job once a day to rotate the log files using [`logrotate`](http://packages.debian.org/unstable/admin/logrotate), followed by a re-cycle of the mongrel cluster:

```
cd ~/www/production/current; logrotate -s log/logrotate.status config/logrotate.conf; mongrel_rails cluster::restart
```

And here's `config/logrotate.conf`:

```
"log/*.log" {compressdailydelaycompressmissingoknotifemptyrotate 7}
```

And finally, just because, we run another daily cron job to vacuum the PostgreSQL database:

```
cd ~/www/production/current; psql cjp_production -c 'vacuum full'
```
