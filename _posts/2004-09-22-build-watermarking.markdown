---
layout: post
title: "Build Watermarking"
alias: /2004/09/build-watermarking.html
categories:
---
Desktop software products, especially of the Windows variety, invariably come with an "About" Dialog listing, among other things, the version of the software. The product version number helps support staff and developers solve problems when they occur out in the wild. Without a version number, tracking down a problem can often be rather difficult.

Especially on web-based applications, making the product build number, build date, and other configuration information (was it a production or a training build, etc.) accessible to the end user is an invaluable aid to developers, testers and support staff. In fact of you look to the side-bar on this blog, you'll find the version of [MovableType](http://www.movabletype.org/) used.

On our last project we made this meta-info available as, funnily enough, META tags (though we could just as easily have used comments) in our JSPs and HTML files. To source the info we simply passed the [CruiseControl](http://cruisecontrol.sourceforge.net) build number and date through into our [Ant](http://ant.apache.org) scripts to use as replacement parameters when copying the [Struts](http://struts.apache.org) (*sigh*) `application.properties` file into the web archive. The person responsible for deployment can always be sure that the correct version of the application has actually been deployed. Then, when testers and users need to report a problem, they simply view the source for the page they are on and, hey-presto, there it is.

On a project I worked on with [Dave](http://www.redhillconsulting.com.au/blogs/david) some time back, we stored the current build number in the database as well. The build number was inserted into a table at the end of the database schema update scripts. A DBA could then visually inspect the data in the table to ensure the correct updates had been applied. Our update scripts also checked this table to ensure a script could not be applied again. At runtime, the application would also double check that it was running against the expected database schema ensuring we never had a, possibly catastrophic, mismatch.

You can add build watermarks to templates used to generate documents such as PDFs, to XML messages sent between systems, in emails as header tags, you name it. In fact anytime traceability back to a particular version of an application might be useful, consider adding some kind of meta-information about your application. Your support staff will love you for it!
