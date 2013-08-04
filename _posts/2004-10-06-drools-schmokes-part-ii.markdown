---
layout: post
title: "Drools Schmokes! - Part II"
alias: /2004/10/drools-schmokes-part-ii.html
categories:
---
So once we'd worked out what the [major hot spot in drools]({% post_url 2004-10-05-drools-schmokes %}) was, it was time to find an alternative method of conflict resolution.

As a bit of background, in simple terms, as facts are asserted, new items (or activations) are added to the agenda. In the general sense, all agenda items are equal. But some are more equal than others.

Although you should stay away from attempting to infer or impose ordering on rules, sometimes it is necessary. Sometimes you just need a couple of "cleanup" or "setup" rules, that are guaranteed to fire before or after all others. In Drools (and JESS) this is known as salience. In JRules it's called priority.

There are other reasons to order the agenda and Drools has a number of different strategies: Random; Complexity; Load Order; etc. These are then chained together. Each Resolver then gets a chance to add the item to the agenda. If it succeeds, no more resolvers are called. If however the item conflicts with one or more existing ones, all are returned and passed to the next resolver to, well, resolve LOL.

Confused? Here's a [better explanation](http://www.sis.pitt.edu/~is2300dm/newtutorial/c6.html).

Looking at the implementation it was apparent that the complexity was O(n^2). Each resolver seemed to be doing a similar thing. It was also optimised quite a bit meaning there was necessarily duplicated code.

My initial gut feeling was that a [priority queue](http://www2.toki.or.id/book/AlgDesignManual/BOOK/BOOK3/NODE130.HTM) was what we needed but how would we do the chaining of the different concerns?

Maybe something like a [Red-Black Tree](http://www.geocities.com/SiliconValley/Network/1854/Rbt.html) would be useful. Maybe we could implement a comparator for each strategy. Conceptually at least, if we used the first comparator to insert into the tree until we found items that were equal. From then on we would continue to insert using the next comparator, etc.This seemed too complicated and I don't do complicated very well. Makes my head hurt.

It seemed that each of the strategies was really just using a different dimension or aspect of the item to perform a sort. It was like a composite key. So whats the easiest way to sort on a composite key? Use a composite comparator. Something like:

```
public class CompositeComparator implements Comparator {private final Comparator[] _comparators;public CompositeComparator(List comparators) {this((Comparator[]) comparators.toArray(new Comparator[comparators.size()]));}public CompositeComparator(Comparator[] comparators) {_comparators = comparators;}public int compare(Object o1, Object o2) {int result = 0;for (int i = 0; result == 0 && i < _comparators.length; ++i) {result = _comparators[i].compare(o1, o2);}return result;}}
```

I tried it out using a TreeSet but it performed just as badly. Maybe I was wrong I thought to myself. So I jumped online and chatted to some of the Drools guys, Mark Proctor in particular. I described my ideas and he seemed to like them.

We did a bit of searching around for implementations we could use. I found one [here](http://gee.cs.oswego.edu/dl/classes/EDU/oswego/cs/dl/util/concurrent/intro.html) but the license wasn't right. Next we thought of [Peter Royal](">Doug Lea's stuff</a> but it was overkill. Finally <a href="http://fotap.org/~osi/) suggested looking at the commons-collections stuff and voila, there it was - [PriorityBuffer](http://www.jdocs.com/commons-collections/3.1/api/org/apache/commons/collections/buffer/PriorityBuffer.html) - and it took a Comparator!

Hackedy, hackedy, hack and we'd replaced the original stuff with the priority queue. Time to give it a whirl.

The first step was to run the queue with a simple `Comparator`. Although it doesn't really do anything much, it would at least allow us to see what the basic overhead of the queue implementation was:

```
public class ApatheticComparator implements Comparator {public int compare(Object o1, Object o2) {return -1;}}
```

Hit run. Damn that's quick! Once more to be sure. Yup. Hmmm. Still not convinced. Add a breakpoint and run in the debugger. Sure enough it's being called. Cool! Ok now to try `LoadOrder` and `Salience`.

```
public class SalienceComparator implements Comparator {public int compare(Object o1, Object o2) {return ((Activation) o1).getRule().getSalience() - ((Activation) o2).getRule( ).getSalience();}}
```

Same deal. All works just fine and after implementing a few more I was convinced that this was going to be a winner.

Now we have O(n log n). Even with all the comparators chained in, the peformance doesn't change one bit. What's more, the different strategies are simple one liners making implementing new strategies almost trivial!

So once more I must applaud the Drools guys for a flexible and performant design!
