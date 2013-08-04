---
layout: post
title: "Servlets are for integration"
alias: /2003/11/servlets-are-for-integration.html
categories:
---
It seems to me that most people I work with are fixated on the idea that servlets and JSPs are for presentation, session beans for business logic, message driven beans for integration, JNDI for service lookup, etc. I tend to have a different perspective on this.

JSPs are a templating language where one possible use is the creation of HTML which is likely (though not guranteed) to be used for presentation. But JSPs may also be used for generating XML again possibly for presentation via XSLT or maybe for SOAP or even CSV, etc.

JNDI may well be used for service lookup in most J2EE applications but JNDI is really an interface to heirarchical databases allow us to query LDAP, NIS, DNS, File Systems, etc. Nothing new here for some people but I bet if you did a poll at work you'd find that most J2EE developers didn't know that!

Message driven beans, well theres no doubting that these are for integration. Just make sure you don't code any business logic directly into them please! Aha you say, of course not that's what session beans are for right? Not in my opinion, no.

Session beans and Servlets are also for integration! Now I know thats not what Application Server vendors such as IBM, Sun, BEA, etc. would like you to believe but IMHO that's all they are.

Session beans provide you with a mechanism for remoting services via RMI, IIOP, Corba, etc. Servlets provide you with a mechanism for remoting services via HTTP (think SOAPServlet) and Message Driven Beans provide you with a mecahnsim for remoting services via JMS albeit an asynchronous-only service.

Again, IMHO, it's all about abstraction. In my mind, these technologies are all different solutions to the problem of distributing services.

If you code your business logic into POJOs then you have the flexibility to distribe your services to RMI clients via Session Beans, web browsers via servlets, SOAP clients via your choice of session beans, servlets and message driven beans. You can even use JavaMail (POP and SMTP) as transports for SOAP messages. For that matter you can serialize java objects and send them via HTTP if you wanted a simple binary protocol for java clients to communicate with an application server through firewalls.

Now try telling that to your Sun-certified J2EE architect :-)

Best of all you can now fully unit test your code, out of container, which means there's no excuse for not doing TDD.
