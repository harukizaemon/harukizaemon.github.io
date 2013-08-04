---
layout: post
title: "A Diamond in the Rough"
alias: /2007/10/diamond-in-rough.html
categories:
---
No, for once this has nothing to do with Ruby (deathly silence) rather, it concerns some [problems](http://forums.opensymphony.com/message.jspa?messageID=12549) many people have experienced when trying to use JNDI resources from [Quartz](http://www.opensymphony.com/quartz/) jobs under Websphere.

In short, Websphere does not propogate any `java:comp/env/` JNDI context to application created (ie non-container) threads. This is apparently by design with the upshot being that if you're using quartz you won't be able to use JDBC data sources from any jobs.

There a number of forum topics that discuss this problem along with various solutions including "use a 3rd-party connection pool instead." Hardly music to the ears when you've just spend the last 3 months converting a customer's entire application over to using JNDI resources. However, after much Googling, I happened upon an [article](http://www.ibm.com/developerworks/websphere/techjournal/0606_johnson/0606_johnson.html) that ultimately solved the problem in a Websphere-friendly manner. It even comes with sample code so you can get started almost immediately.

In essence, the solution makes use of Websphere's [Asychronous Beans](http://publib.boulder.ibm.com/infocenter/wsdoc400/v6r0/index.jsp?topic=/com.ibm.websphere.iseries.doc/info/ae/asyncbns/concepts/casb_workmgr.html) (and the [Work Manager](http://publib.boulder.ibm.com/infocenter/wsdoc400/v6r0/index.jsp?topic=/com.ibm.websphere.iseries.doc/info/ae/asyncbns/concepts/casb_workmgr.html) in particular) as a sort of thread pool. We then took this idea and adapted it around the use of Quartz's [pluggable<a/> <a href="http://www.opensymphony.com/quartz/api/">`ThreadPool`](http://www.opensymphony.com/quartz/wikidocs/ConfigThreadPool.html).

The final twist was the need to support both Websphere and Tomcat seamlessly without resorting to either build or deployment time configuration. For this we simply created a `ThreadPool` implementation that looks up the Work Manager in the JNDI context. If it exists, we assume that we're in Websphere and go from there; otherwise we use the [default Quartz implementation](http://www.opensymphony.com/quartz/api/org/quartz/simpl/SimpleThreadPool.html) and hope for the best :)
