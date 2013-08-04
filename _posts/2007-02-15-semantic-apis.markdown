---
layout: post
title: "Semantic APIs"
alias: /2007/02/semantic-apis.html
categories:
---
I was chatting with my brother today (somewhat of a professional student with degrees in neuroscience, physics and maths) about software development. I was explaining how yesterday had a been a rather unpleasant day working out how to integrate with Crystal Reports. You see, what I wanted was an interface that looked something like this:

{% highlight java %}
CrystalReport report = new CrystalReport("report.rpt");
report.setParameter("Posting Year", "2007");
report.setParameter("Account Number", "5678");
ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
report.writeTo(outputStream);
outputStream.close();
{% endhighlight %}

Only instead what I got was this:

{% highlight java %}
ReportClientDocument document = new ReportClientDocument();
document.open(_reportName, OpenReportOptions._openAsReadOnly);
DataDefController dataDefController = document.getDataDefController();
ParameterFieldController parameterFieldController = dataDefController.getParameterFieldController();
parameterFieldController.setCurrentValue("", "Posting Year", "2007");
parameterFieldController.setCurrentValue("", "Account Number", "5678");
InputStream inputStream = document.getPrintOutputController().export(ReportExportFormat.PDF);
int b;
while ((b = inputStream.read()) != -1) {
    outputStream.write(b);
}
inputStream.close();
document.close();
{% endhighlight %}

The first is nice and semantic; it's pretty obvious what the code is doing. The second requires you to read, very carefully, each line in order to work out what is going on. Talk about leaky abstractions. Apparently my Document's connected to my, `DataDefController`; my `DataDefController`'s connected to my, `ParamaterFieldController`; ...

My brother drew an analogy with explaining to someone how a telephone works. In the first case, we've gone through a very simple explanation with just enough information to allow someone to have a go themselves; in the second example, we're now explaining how the spin of each electron determines the probability of it going down the wire and thus contributing to the current that ultimately makes the phone call possible.

At first I was concerned: Having [just recently ranted](/blog/2007/02/12/just-tell-joe-sent-you) (for the umpty umpth time) that developers don't seem to understand even the most fundamental principles of software development, here I was lamenting the fact that most APIs weren't simple enough. My brother then asserted that perhaps the reason software is (in general) so poorly written is precisely because the APIs we are forced to use are so primitive. That because we are forced to follow so many steps in achieving something that is conceptually so simple (such as producing a report) the likely-hood of failure is much greater.

The scary thing is, this is certainly not an isolated example. Have you ever tried using the `javax.mail` packages? JNDI anyone?

In the end, I wrote a class named, unsurprisingly, `CrystalReport` with an interface exactly as in the first example and implemented almost exactly as in the second. But I seem to need to do this quite a lot when dealing in the "Enterprise" Java world.
