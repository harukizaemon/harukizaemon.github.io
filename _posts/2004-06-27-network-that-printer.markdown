---
layout: post
title: "Network That Printer"
alias: /2004/06/network-that-printer.html
categories:
---
Today I shall depart from my usual software development rants. It's not often that a piece of hardware tickles my geek bone but I found something recently that did just that.

I have a 1.5Mbps DSL connection to the internet and run everything on a wireless laptop running [Gentoo Linux](http://www.gentoo.org) and Windows under [VM Ware](http://www.vmware.com) for the times when I absolutely need to test something on windows.

The only piece of equipment I need to connect to my laptop is my ancient HP Laserject 5MP printer. Or should I say, needed. On Thursday I picked up a [NetGear PS101 Mini PrintServer](http://www.netgear.com/products/prod_details.php?prodID=143). A tiny (half-cigarette packet-sized) device that attaches directly to the printer and then via a CAT-3 cable to my hub. Granted it's not wireless (though NetGear and [LinkSys](http://www.linksys.com) do have them) but at AU$115 it's sensational value for money.

It supports both static and DHCP (the default) IP address assignment. It comes with Windows drivers (if you really feel the need hehe) but importantly took no more than about 5-mins to get working with my [CUPS](http://www.cups.org) installation on Gentoo. I just needed to work out its IP address and internal print server name and set CUPS to print to: `lpd://ip_address/internel_print_server_name`

Don't forget to set your [iptables](http://www.netfilter.org/) configuration to allow printing on port 515 (printer):

{% highlight console %}
> iptables -A OUTPUT -p tcp --dport printer  -m state --state NEW -j ACCEPT
{% endhighlight %}

And you're done. Sweeeet.
