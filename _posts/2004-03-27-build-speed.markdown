---
layout: post
title: "Build Speed"
alias: /2004/03/build-speed.html
categories:
---
I've inherited the poorest performing machine on the team. Only fitting I suppose as I'm probably the poorest performing member (it's a pretty good team!) But the main problem I have is that it takes so long (between 10 and 17 minutes depending on how long it's been since I rebooted windows) to do a pre-checkin build (to make sure I haven't screwed anything up) that by the time it finishes, I really need to to another CVS update. This invariably brings back a number of changes that good old paranoid me thinks necessitates another local build. This vicious cycle continues until I give up and just check in my code.

Of course I then need to wait until the integration build on the [cruise](http://cruisecontrol.sf.net) machine completes before I can be happy I've not broken anything. Now the chances are that there is already a build in progress and, on average, about half-way through. The integration build itself takes about 20 minutes all up (and that's on a fast machine). So, it's now about an hour by the time I've got anything checked in and I'm happy I can go home.

This morning, Andy Trigg removed the generation of the test reports from the local machine build as this was taking around 2-3 minutes on my machine and besides, it's really only needed if the tests break right?

But that didn't really help all that much and I finally cracked it and decided to take a look at what was causing the build to take so long. How long can it take to compile and run 1200 unit tests and around 250 functional tests? I mean sheesh! A lot of time and efforts has gone into ensuring that the unit tests are as close to "real" unit tests as possible and not just functional tests in disguise so they should be blindingly fast to run.

 Closer examination of the build file revealed that the tests were being forked. This was because there is some bug in the build process that causes `OutOfMemoryError`s. This was clearly going to slow things down but not much I can do about it as far as quick wins goes.

The next thing I noticed was that version 1.6 of ant has a new option on the [JUnit](http://www.junit.org) task (`reloading`) that allows you to confugure if a new class loader will be used for running each test class. I quickly set this to `false`!

And lastly, I discovered that if I ran all the unit tests in [IntelliJ](http://www.intellij.com) they took around 30 seconds but run from the [Ant](http://ant.apache.org) script they take around 3 minutes!

Then I remembered playing around with test suites some time ago. I vagualy recalled that putting tests into a suite ran orders of magnitude faster than if you ran them one at a time. A few minutes using good old of `find`, `sed`, `grep`, etc. and I created a class named `Suite` that looked something like this:

{% highlight java %}
public class Suite extends TestCase {
    public static Test suite() {
        TestSuite suite = new TestSuite();
        suite.addTestSuite(BlahBlahTest.class);
        suite.addTestSuite(FooBarTest.class);
        ...
        return suite;
    }
}
{% endhighlight %}

Then I changed the build script to run my suite instead of each individual file, making sure that I forked it just in case.

Well blow me down. The pre-checkin-build now runs in 3 1/2 minutes! That's a full clean and rebuild plus unit and acceptances tests. Whats more amazing is the actual unit tests now take an amazing 9.4 seconds to run. A clean unit test build now only takes 1 minute and an incremental build a mere 30 seconds. Woohooo! I can't wait to see how the cruise box performs now as it was already at least twice as fast as my machine.

The only real issue was then the fact that we would have to maintain the suite to ensure we didn't leave out any tests. Stuff that I thought so I quickly whipped up an ant task to generate it for me and away we went. I also suggested we could remove the need to compile the suite by generating the byte-code directly to which [James](http://www.redhillconsulting.com.au/blogs/james) just frowned dissaprovingly and accused me of being _"such a geek!"_ Maybe he's right? hehehe.

Next week I'll see if I can get similar improvements in the acceptance and regression tests.

Oh yeah the test report generation now takes a whole 1.5 seconds to run so it might make it's way back into the local build again but I doubt it. Every second counts! :-)
