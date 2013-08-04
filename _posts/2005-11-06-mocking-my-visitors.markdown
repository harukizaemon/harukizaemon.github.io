---
layout: post
title: "Mocking my Visitors"
alias: /2005/11/mocking-my-visitors.html
categories:
---
I mentioned in a comment earlier this week that I like [Builders](http://en.wikipedia.org/wiki/Builder_pattern) as they allow me to separate out the ickyness involved with construction from my nice pristine objects. Besides anything else, it allows me to layer on functionality: first implement the domain objects, assembled by hand for testing purposes; then create another layer -- the builder -- that will automate the construction process.

Of course being the good little TDD weenie that I aspire to be, at some point I want to test that the builders are generating the correct structure.

IMHO, the utterly evil-broken-and-wrong approach is to expose all the properties on all the nodes in the graph -- I use the terms node and graph generically here to refer to objects related in some fashion -- for reasons I've harped on about more than enough.

Another approach -- that maintains encapsulation -- is to implement `equals()` for all the nodes in the graph. The idea for testing then is to create the expected structure by hand (just as I had already been doing) then use the builder to create another and assert that the two are identical. I have actually used this approach before and while it worked just fine, it never really felt Good&trade;. I'm not sure why; just an intuitive ookyness.

More recently though I tried a different approach: using a visitor. Again the approach was similar: constructed the expected structure by hand; then use the builder to make another; and finally assert that the two are identical. This time however, the idea was to traverse the expected structure and match, node-for-node, with the actual. The neat thing about this approach though was that because my visitor class was an interface, it was trivial to use [EasyMock](http://www.easymock.org/) to do all the grunt-work for me. (EasyMock also allows mocking classes however I still prefer interfaces.)

The idea is to create a mock visitor using EasyMock and pass that to the `accept()` method of the structure I had created by hand to set-up the expectation. Once that was done, I could then simply pass the same mock to the `accept()` method of the structure constructed by the builder:

``` java
public void testBuilderMakesIdenticalStructureToOneBuiltByHand() {
    // Create the two structures
    Node createdByHand = createByHand();
    Node createdByBuilder = createByBuilder();

    // Visit the one created by hand to set-up the expectations
    NodeVisitor visitor = EasyMock.createStrictMock(NodeVisitor.class);
    createdByHand.accept(visitor);
    EasyMock.replay(visitor);

    // Visit the one created by the builder to verify
    createdByBuilder.accept(visitor);
    EasyMock.verify(visitor);
}

private Node createByHand() {...}

private Node createByBuilder {...}
```

_Note: I'm using the latest version of EasyMock that supporst JDK 1.5 generics however I'm not (nor will I ever be) using static imports as suggested in the documentation!_

This time, it felt Good&trade;. I could use the same visitor for testing, reporting and persistence, all without the need to break encapsulation.

Now while I'm really not keen on igniting a religious flame-war over mock objects in general nor mock object libraries in particular, the fact that EasyMock works against the real interfaces -- as opposed to using string descriptions ala [JMock](http://www.jmock.org/)-- was a huge advantage in this case.
