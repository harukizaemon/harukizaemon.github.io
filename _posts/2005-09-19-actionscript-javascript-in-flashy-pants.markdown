---
layout: post
title: "ActionScript: JavaScript in Flashy Pants?"
alias: /2005/09/actionscript-javascript-in-flashy-pants.html
categories:
---
One of the jobs I'm doing at the moment involves [Flex](http://www.macromedia.com/software/flex/) -- and consequently [Flash](http://www.macromedia.com/software/flash/flashpro/) -- for the front-end and Java at the back-end, most of which involves writing ActionScript.

If you haven't used ActionScript before, you'd be forgiven for thinking you were writing JavaScript. Well, in fact you are writing JavaScript with a few changes, not necessarily for the better IMHO -- If you read the documentation you'll discover that ActionScript is indeed based on [ECMA Script](http://www.ecma-international.org/publications/standards/Ecma-262.htm), the same specification that was retro-fitted to JavaScript.

From what I can tell, ActionScript 1.x was pretty much _exactly_ JavaScript. Version 2 on the other-hand adds some syntactic sugar to the language; no doubt an attempt to make a [prototype-based language](http://en.wikipedia.org/wiki/Prototype-based_programming) _appear_ more like a class-based language in the hope of attracting all the Java developers. I'll get into these additions in a bit but in any event, I find it highly amusing when Macromedia proclaim that version 1 was somehow a functional language and that version 2 is now a fully-blown O-O language.

(As an aside, I interviewed some potential developers the other day who said they loved ActionScript but thought JavaScript was a language for doing hack-work. I can only assume that opinion comes from having only used JavaScript for handling `onXXX` events in a browser rather than any objective comparison of the langauges themselves.)

So, on to some of the more notable changes to the language starting with type-safety. JavaScript is weakly typed. This is a topic of biblical proportions so I'll leave alone the merits or otherwise of weak-typing suffice to say that I like it. In their infinite wisdom, Macromedia decided that what was needed was a bit of strong (or strict) typing. This is enforced by the compiler with a combination of type declaration after variable and parameter names:

```
var foo **: Number**;
```

Unfortunately this type-safety is limited to compile-time checking -- nothing happens at runtime, much like Java's generics. Not so much of a problem I guess -- I didn't want the strong typing in the first place -- but amusing nonetheless.

So the next question is, how do you know what functions are available for a given type? In JavaScript these are essentially defined at runtime by adding behavior to a prototype. Well why not add some more syntax to the language?

```
**class** MyClass {**private** var num : Number;**public** MyClass(num : Number) {this.num = num;}public getNum() : Number {return this.num;}}
```

Ok, so that's probably a little easier for most people to understand than:

```
MyClass = function(num) {this.num = num;};MyClass.prototype.getNum = function() {return this.num;};
```

But I actually find the "new" way actually involves a lot more typing than the good-old-fashioned way -- which once you're used to is easy to read anyway -- and hides the fact that it is still a prototype-based language and NOT a class-based one. (I also quite like the way [prototype](http://prototype.conio.net/) declares "classes" but I haven't found the time to start doing it that way just yet.)

You can also declare property accessors just like in VB/C#/etc:

```
class MyClass {private var num : Number;...public **get** num : Number {return this.num;}public **set** num(num : Number) : Void {this.num = num;}}
```

Again, great if you like that kind of thing; I'm still not convinced I do.

The other thing that gets people is the scoping rules. JavaScript has some pretty funky scoping rules which once you're used to and understand work just fine. Again, because ActionScript _looks_ like Java, developers don't realise the importance of understanding the scoping rules.

ActionScript also introduces another new keyword: `dynamic`. This can be used when defining a class and indicates to the compiler that it should **NOT** do strong type checking when invoking methods and accessing properties of the class -- either from within or from outside the class. Again, I find this a highly amusing construct as I can always defeat the type checking the old-fashioned way anyhow: `myObj["doSomething"](15);`. Yes, I agree, that's a bit of malicious code but what I'd prefer is an option to turn on/off strict typing at a project level and not on a per-class basis.

Interestingly, someone recently pointed me at an [open source compiler](http://www.mtasc.org/) for ActionScript. I wonder if I could just download the code, remove the strict type checking and be done with it ;-). Oh and there's also a [unit testing toolkit](http://sourceforge.net/projects/flexunit/) as well.

All in all, I quite like using ActionScript. I've only played around a bit with Flash/Flex bindings but they seem quite nice too. I actually think it would be pretty easy to build a Cocoa -- JavaBeans the way they _should_ have worked -- like framework (eek there's the word!) which I thoroughly enjoy using at the moment.

But when it comes down to it, It has been my experience -- and no doubt the experience of others in the JavaScript/Ruby/Smalltalk/Objective-C world -- that I can achieve a lot more in fewer lines of **readable** code _without_ all the syntactic sugar and strong typing. (The caveat being that I'm also a big fan of automated testing.) Moreover, understanding that JavaScript uses prototypes opens up a whole new world of possibilities that further increase my productivity -- a fact that became apparent to the attendees of a mini presentation I gave on this subject just recently.
