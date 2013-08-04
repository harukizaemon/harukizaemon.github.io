---
layout: post
title: "Unit tests should play nice"
alias: /2004/04/unit-tests-should-play-nice.html
categories:
---
I've seen a few blog entries around of late demonstrating nifty things you can do to achieve setup/cleanup in your unit tests using statics or classloaders etc. And whilst I admire the creativity and ingenuity, I have to say that just like [testing private methods]({% post_url 2004-02-06-dont-touch-my-privates %}), to me it smells.

Firstly, If unit tests rely on static state and class loaders for setup and cleanup then the tests are just plain broken and wrong. The ultimate goal for me is to be able to run test classes in parallel. Just splitting them up to achieve this won't help because you likely have no way of knowing which test classes interact with which static parts of the system. The good old abstract static factory is a prime example of this. Unit tests should probably create any data and mock any infrastructure they need for the test. They certainly shouldn't be relying on the behaviour of a class loader! If there's too much infrastructure to mock, thats a smell too!

Next, ideally we'd like to run test methods in a single test class on a single instance of said test class, implementing `setUp()` and `tearDown()` as appropriate. Maybe the idea of a single instance isn't always practical (I can't think why) but just assuming the tests will be run in a new instance isn't acceptable either. What could I possibly want to do in a constructor that I couldn't do in `setUp()`? Again, relying on the behaviour of the test runner to create new instances or the class loader to unload static state is just wrong!

There should be no assertions in `tearDown()`. Assertions failures in `tearDown()` will mask any failures in the test itself making it all but impossible to track down the actual problem.

And lastly, avoid extending other test classes as much as possible. Remember, any tests that are defined in the super class wil also be run in the subclass. Not only do we end up redundantly running the super class tests but if you have inadvertently overidden some public or protected methods (say `setUp()` or `tearDown()` for example) and forgotten to call the super method of the same signature, all hell breaks loose.

As a general rule, excessive refactoring of unit tests can make them difficult to understand. Sure it might be testing the code but it WILL make maintenence of the tests very difficult and surely hinders the ability of anyone to get a handle on the expected behaviour/role of the class under test.
