---
layout: post
title: "Business Rules Fallacy #1"
alias: /2004/05/business-rules-fallacy-1.html
categories:
---
Remember the good old days when [Crystal Reports](http://www.businessobjects.com/products/reporting/crystalreports/default.asp) was going to save the world? That's right. By about now (2004) every user on the planet would be writing up their own ad-hoc reports straight from the database. All they needed was the database schema and away they would go.

What? You mean your business users aren't doing this? Really? Say it ain't so!

No, the truth is it never really eventuated the way we (the IT industry) had envisaged. End users just don't get fully normalised data structures. FWIW, most people I work with probably don't understand why `25,000,000 + NULL == NULL` so how did we ever expect users to? Oh and let's not forget the IT manager who has enough knowledge of the system to be dangerous. He has a big picture of the database schema on his wall and knows just enough SQL to build queries that do multiple table scans over the millions of rows of inventory data, causing the DBMS  to not so quietly tell every other use of the system to please get nicked :-) The plain fact is that writing reports typically requires as much help from IT infrastructure as to make it an IT task.

So now that Business Rules are looking more cylindrical with that silver sheen each day, some of us naively believe that our so called "Business Users" should be able to code up/modify rules for direct inclusion into a production system all by themselves. To me, this is an even bigger problem than reporting.

All the problems that plague end-user reporting apply. Users don't understand our lovely, normalised, domain model. They surely don't understand why they get a `NullPointerExceptions` when adding numbers. And when it comes to knowing the difference between `and` and `or` you can forget it!

What's worse is that Business Rules are used directly by the application to "reason" on appropriate behaviour. By comparison, with the exception of the table scan problem and of course any bad decisions that might be made based on incorrect data, reporting seems rather innocuous.

Now, if I said to the man (and in this case yes it is a man) who writes the cheques, hey how about we don't test any of this code before we put it into production, I'd get the sack immediately. And quite rightly so I might add. Application components interact in subtle and non-obvious ways that necessitate large-scale unit/functional and integration testing. Business rules are no different. Actually they can be worse. We have many years of collective experience managing essentially procedural languages such as Java, C, C++, etc. Most developers I know think a lisp is a speech impediment and surely wouldn't know a prolog if they tripped over one :-)

Just like with reporting, we can try going down the path of writing views and buulding neato tools to try and make this stuff more like human readable languages and structures but when it comes down to it, like reporting, business rules require about the same (if not more) intervention from IT departments when end users write them as when the developers themsleves write them. The "best" tools in the world won't solve real problems with allowing end users the ability to directly modify business rules. I mean, glasses aren't much good if the patient is blind.

Business rule maintenence is and should be an IT responsibility. However, the rules must be representable in a way that makes it easy for an end user to verify the translation from written/spoken languages. Appropriate use of the lower-level [JRules](http://www.ilog.com/products/jrules/) or [OPSJ](http://www.pst.com/opsj.htm) languages makes this a reality without the need for tools to render rules from/to "plain english". With a tiny bit of coaching, our business users are finding they can understand the rules sufficiently to know when we've made a mistake. In fact this iteration, our business rep has indicated he'd like to try his hand at writing one. No points for picking the irony in that.
