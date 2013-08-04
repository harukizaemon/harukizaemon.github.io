---
layout: post
title: "Random thoughts on our current Agile process"
alias: /2009/08/random-thoughts-on-our-current-agile.html
extract: |
    <p>It's late and I don't seem to be able to sleep so for something to do I thought I'd jot down (ok copy from an email I sent out earlier in the week) some thoughts about our development process as it has evolved over the last few months or so.</p>
    <p>As always, I can only speak from personal experience so here's my totally subjective perspective, YMMV:</p>
categories:
---
It's late and I don't seem to be able to sleep so for something to do I thought I'd jot down (ok copy from an email I sent out earlier in the week) some thoughts about our development process as it has evolved over the last few months or so.

As always, I can only speak from personal experience (one data point doesn't really count for much) so here's my totally subjective perspective, YMMV:

* Full-time pairing when possible/practical
* No more than one card per pair in dev at any time
* No more than one card per pair in ready-for-dev at any time
* Nothing to be blocked; If it's blocked we work to remove the blockage
* New cards are demand pulled into read-for-dev as cards are moved from dev to ready-for-test
* We have big picture story card sessions as required
* We have planning meetings at the start of each iteration where we discuss what's "planned"
* We give everything t-shirt sizes (S, M, L)
* No technical stories; everything must be done because it delivers business value
* We don't measure velocity
* We aggressively split cards
* I repeat, we aggressively split cards
* Daily stand-ups
* Parking lot for post-stand up discussions
* 2-week iterations with retro followed by a kick-off to discuss the stories
* We try to focus on doing things as simply as possible
* We try to focus on building things correctly rather than as fast as possible
* We fight to have a REAL user available to better understand their needs
* We rally against the usual cries of "but I know we'll need it"; We trust that by building things simply we can always add on the extra functionality later
* Trying to deliver everything to everyone leads to delivering nothing at all to anyone
* T-shirt sizes help the business prioritise not estimate delivery dates; Business value is a function of, among other things, time/cost to build
* Rolling technical stories into stories that deliver business value force us to change course slowly and justify changes
* We usually have parallel implementations of some things as a consequence
* Minimal changes are allowed on "old" implementations; anything substantial requires a migration to the "new" implementation
* If we can deliver _some_ business value early by splitting the cards we do so as soon as possible; Eg. business can view existing data in the new form but editing is a new card because they can still use the old mechanism for that.
* It's critical that the whole team is taken on the "journey" so they understand _why_ things are being built. Doing so brings the team into alignment and also allows the team to make informed decisions such as re-structuring work to enable splitting cards for aggressively.
* Bringing the team along for the journey can be painful, never gets easier, and is always worth the effort.
* Just-in-time stories really does enable the business to leave the decision as to what's important to the last possible moment
* Implementing the smallest amount of code possible for each story is critical to enabling just-in-time development; less code == greater flexibility; E.g. don't use a database when the data comes from a spreadsheet and presently only ever changes in a spreadsheet.
* We do as much forward thinking as possible/practical; We think of as many likely scenarios as we can and keep reducing the scope of the implementation so as not to preclude implementing them later on
* Almost nothing ends up looking as we thought it would when first envisaged.
* More important than writing the code is working out how to structure the implementation so that we get the job done without precluding possible future work; sometimes this means at least thinking through a strategy for migrating from one implementation to another later on if necessary.
* It's amazing how splitting stories reveals just how little the business value certain aspects of stories
* We almost never get through everything that was "planned"
* We almost always end up doing stories that weren't "planned"
* We are as close as we can get (due to the bureaucratic nature of the client's operations group) to on-demand deployment into production. Ideally we'd like it to be automated but that's just not going to happen anytime soon
* We're motivated by getting things done; call that velocity if you will but we really haven't found a need to measure velocity. Delivering a constant stream of small but valuable stuff into production every week is VERY motivating.
* We value delivering something over delivering nothing
* We actively plan for change
Caveats:

* We have a highly competent team
* We have a fixed budget
* We have internal and external users
* We have UI and data-only users
* We have an existing implementation we are evolving away from
It's far from perfect and is constantly evolving but as [Travis](http://www.prozacblues.com/) observed, it kinda represents a snapshot of what being Agile means to me right now.
