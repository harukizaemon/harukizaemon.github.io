---
layout: post
title: "Don't touch my privates"
alias: /2004/02/dont-touch-my-privates.html
categories:
---
I was giving a talk today on design and testing, refactoring, tools etc. and a question regarding the testing private methods came up.

We were discussing reducing cyclomatic complexity by encapsulating conditional statements in private methods. I showed a block of fictional code that included a test that the customer was at least 18 years old and contained a number of conditionals including the 2 or 3 checks required to test the age.

So we re-factored the code to extract the age check and demonstrated that the code was now much more readable and understanable. The newly created `isAtLeast18YearsOld(Date dob)` method made it clear what the calling method was trying to achieve.

The next question was _"should we write a test for this?"_ and if so, _"how are we going to do that if it's private?"_

Now, I _almost_ never test private methods. I say almost, just in case i've either done it once before and forgotten or I need to change my mind at some point in the future. But as far as I'm aware, I never test private methods.

Private methods are private for a reason. They're implementation detail. Yet another reason I dislike Java Beans so much - they simply expose the innards of  classes such that the private instance fields might as well have been marked as public. "Guns don't kill people, people kill people." True enough but give a developer a `getter` and it's death to good design.

Naturally, my first reaction is that I do believe there is enough logic in there to warrant a test but that it's private and I don't test private methods. Instead, maybe the method deserves to be public and therefore testable and if so where does it belong?

_"how about we put it into the `Person` class?"_ someone asks. What a sensational idea I replied! No more passing around a `Date`.

Sometimes it's more natural to place the the logic in say a strategy, making it pluggable. In this case you may choose to make the method more general such as `meetsAgeRequirements()`. Then, once it's pluggable you could move the real implementation into a rules engine. Whatever you do, resist the temptation to put it into a [`static helper`](/blog/2003/12/05/help-save-the-object)! IMHO `statics` are the last resort of the scoundrel programmer ;-).

In one simple example we'd managed to:

* greatly simplify our code;
* extract and make obvious some business logic;
* put that logic back into the class where it's closest to the data on which it operates;
* Justify making the method publicly accessible and therefore testable; and;
* remove (from the class we're implementing) a dependency on data (ie date of birth) contained within another class.

We've [given our classes behaviour!](/blog/2003/12/03/arent-classes-supposed-to-have-both-data-and-behaviour)

Private methods exist primarily to reduce the complexity of other methods and/or to remove code duplication. Either way, they are incidental to the implementation detail.

If you feel you need to test a private method (maybe because it's complex or contains some kind of business logic), rather than subverting Javas access protection mechanisms or perverting your code, have a think about what the method really does and where it belongs. Chances are, you've [missed an important abstraction or concept](/blog/2004/02/04/lots-of-little-classes).
