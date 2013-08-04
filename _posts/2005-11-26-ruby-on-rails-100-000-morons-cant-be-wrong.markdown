---
layout: post
title: "Ruby on Rails: 100,000 morons can't be wrong"
alias: /2005/11/ruby-on-rails-100-000-morons-cant-be.html
categories:
---
So I have to admit that not only am I a <strike>card</strike>pod-carrying member of the apple cult, but lately I've been dabbling in a bit of <strike>voodo</strike>Ruby-on-Rails as well and so-far, me likey. I think it's one of those tools that people will either get or not. If they do, it'll be fantastic; if not...

Rather than give you my personal take on this, I thought I'd take a slightly different approach and get in touch with one of my [inner-morons](http://www.eaves.org/blog-archive/000229.html) for a more balanced perspective.

Ahem...

I'd like to start by saying outright that I think RoR will be a good fit for most large corporate development shops. I can see it delivering 20x, 30x maybe even 50x improvements in efficiency however I do see some short-comings, probably due to it's relative immaturity when compared with Java.

If you look at most of the projects that are using Rails and Ruby at the moment, the code-base consists of dozens, maybe hundreds or possibly even a few thousand lines-of-code at most; Certainly nothing that compares to the size of projects we are used to working on which are in the order of tens if not hundreds-of-thousands of lines-of-code. But fear not, we have a plan to ensure it's readiness for enterprise use.

We have two choices: develop applications and components on an as-needs basis; or try to build some common infrastructure that all projects can use.

The first option sounds dangerous as it will no doubt lead to everyone doing it slightly differently which in turn leads to lots of unnecessary duplication of effort. So as part of the next project, we plan to produce a framework (Ruts?) that will sit on top of RoR upon which all the teams can build. This way we will have better management and control over the infrastructure.

We'll start by checking the source code for Ruby and Rails into our repository so we can patch them whenever we need to. This means we don't have to wait for patches to become available in production versions. It also has the added benefit that when the API's change our code won't need to be re-factored as a consequence. Isn't open-source great!

Next we'll address configuration. There really needs to be a way to place all the configuration in XML files. XML is, after all, THE standard; we've been using XML successfully in Java for years and everyone understand it. We'll start with database connection details, then move on to other areas. One area of particular interest will be database schema generation. At the moment Rails only supports coding the SQL directly. Unfortunately this doesn't allow us to write database agnostic DDL; enter XML. If we create an XML-based language for defining the schema, we can then generate the SQL for each desired database; heck we can also generate the appropriate Rails model classes as well including relationships, etc.

We'll also need clustering -- every enterprise application needs clustering. To this end we'll extend rails -- we have the source -- to support transparent clustering by sending session state to a master controller server that will then send it to all the nodes in the cluster. This will require serialisation and class version management which ruby already has built-in.

We see other improvements in efficiency in terms of testing. We find that due to the very slow turn-around time of Java web applications, our developers spend a great deal of their time writing tests. Rails on-the-other-hand has such a quick development cycle time that we can dispense with much of the automated testing in favour of manual testing.

Having said that, we have noticed that each time the database schema or configuration changes we have to stop and start the server so we will also include an extension to Rails that looks for changes in the configuration and automatically refreshes rails. Again, once implemented in the framework, each project will benefit.

Another limitation in Rails is the MVC implementation. Again, it's pretty good but it could still be improved. We've known through years of experience now that trying to put all that code into one controller class just doesn't work. Instead we will create command objects, wired together using XML and a generic `AbstractController` that will read the configuration file and work out what to do. This will be much simpler and we envisage huge time savings as a result.

Also the active-record stuff might be a good starting-point but it will need to be enhanced a bit for real enterpise applications. For a start, what it needs are DAO's, one for each table in the database. Again a few classes in this respect would do wonders for productivity. We might even write some code that could generate DAO's from the schema XML configuration discussed earlier.

Come to think of it, if we had some kind of container, then it could manage all this for us and more including transaction management, dependency injection based off an XML file, etc.

We've also been looking at the query language. It's pretty good but it's not very rich and it's basically just SQL injected into your source. What Rails really needs is some kind of Object Query Language along the lines of hibernate. In fact, because hibernate is open-source, we might want to consider replacing active-record with a hibernate re-write (Rhibernate?)

So, as you can see, with a few little tweaks here and there, Ruby-on-Rails might just be what we need to get our enterprise development teams moving towards Web 2.0. We just need to work out how to get Rational Rose and ClearQuest integrated to generate all the code for us and we're set to go.
