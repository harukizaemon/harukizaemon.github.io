---
layout: post
title: "Testing thread safety revisited"
alias: /2004/02/testing-thread-safety-revisited.html
categories:
---
At a loose end today I turned my attention back to a [recent blog of mine]({% post_url 2004-02-01-ignorance-was-once-bliss %}) on testing for thread safety. Spurred on by your feedback and possibly just to prove a point ;-), I decided I'd spend a few hours and see what I could come up with.

Before delving into the code, I'll set out the scope or terms of reference for the excercise:

* I wanted to test a very simple class for thread-safety;
* The class should be written (designed) with thread-safety in mind, ie. with synchronization in place;
* I will only deal with synchronization at the method delcaration level
* The tests should prove that the class works correctly with synchronization in place;
* The tests should also prove that the class fails when sychronization is removed; and finally;
* I want it to be automated so I need a way to have a synchronized and unsychronized version of the class.

The last point here could be handled by using an interface much like the synchronized collection wrappers but I felt this was kind of cheating. Instead I decided to use the [ASM byte-code library](http://asm.objectweb.org/) to do some magic on the class files.

Now onto the code and some brief explanation of the classes. You should be able to copy and paste the code into your [favourite IDE](http://www.intellij.org), compile and run. You'll obviously need [JUnit](http://www.junit.org) and the [ASM byte-code library](http://asm.objectweb.org/).

First the class under test which I based on some [code I'd seen](http://www.npac.syr.edu/projects/cps615fall95/students/jgyip5/public_html/cps616/conflict.html) when researching threading:

{% highlight java %}
import java.util.Random;

public final class ThreadSafe {
    private int _common;

    /**
     * Increment the common variable
     * @return true if we managed to update it "atomically", otherwise false to indicate failure
     */
    public synchronized boolean increment() {
        int common = _common;
        spin();
        boolean successfull = (_common == common);
        _common = common + 1;
        return successfull;
    }

    private void spin() {
        Random random = new Random();
        try {
            Thread.sleep(random.nextInt(500));
        } catch (InterruptedException e) {
            // Ignore it
        }
    }
}
{% endhighlight %}

As you can see, very very simple. Its sole reason for existing is to demonstrate how multi-threading can cause conflicts. The class is correctly synchronized. That is, if left unmodified it should behave correctly and with no failures in a mutli-threaded environment. However, if multiple threads were to execute through a single unsychronized instance, we would hopefully end up with a failure. The call to `sleep()` with a random duration is a means to that end.

A slight variation on this would be to update an unsyhronized `Collection` instead of a simple counter. The `Collection` classes typically throw a `ConcurrentModificationException` which we could either catch and return false or modify our test to catch the exception itself.

Next, the somewhat large test class which is broken up into a few inner classes for simplicity. Don't believe that it's simpler this way? Try splitting them out and see what happens. It's possible, but some of the logic becomes too complex for my liking.

Anyway back to the code:

```
import junit.framework.TestCase;import org.objectweb.asm.Attribute;import org.objectweb.asm.ClassAdapter;import org.objectweb.asm.ClassReader;import org.objectweb.asm.ClassVisitor;import org.objectweb.asm.ClassWriter;import org.objectweb.asm.CodeVisitor;import org.objectweb.asm.Constants;import java.io.IOException;import java.lang.reflect.Method;import java.util.Iterator;import java.util.LinkedList;import java.util.List;public class ThreadSafeTest extends TestCase {public ThreadSafeTest(String name) {super(name);}public final void testSucceedsAsis() {assertTrue(execute(getName()));}public final void testFailsWithoutSynchronization() throws Exception {// We'll need our custom class loader to perform byte-code manipulationCustomClassLoader loader = new CustomClassLoader();// We need an instance of this test within the new class loaderClass testClass = loader.loadClass(getClass().getName());// Run itMethod method = testClass.getMethod("execute", new Class[] {String.class});assertFalse(((Boolean) method.invoke(null, new Object[] {getName()})).booleanValue());}/*** Runs the actual test.* @param name The name of the test* @return true on success, otherwise false to indicate failure*/public static boolean execute(String name) {// The class under testfinal ThreadSafe target = new ThreadSafe();// All threads are to share the same thread groupfinal ThreadGroup group = new ThreadGroup(name);// Create a bunch of threadsfinal List threads = new LinkedList();for (int i = 0; i < 5; ++i) {threads.add(new CustomThread(group, target));}// Start up the threadsfor (Iterator i = threads.iterator(); i.hasNext();) {((CustomThread) i.next()).start();}// Wait for them to finish and determine if we were successfull or notboolean successfull = true;for (Iterator i = threads.iterator(); i.hasNext();) {CustomThread thread = (CustomThread) i.next();try {thread.join();} catch (InterruptedException e) {// Ignore it}successfull &= thread.successfull();}return successfull;}/*** Runs the class under test and reports on the success or failure.*/private static final class CustomThread extends Thread {private final ThreadSafe _target;private boolean _successfull = false;public CustomThread(ThreadGroup group, ThreadSafe target) {super(group, "");_target = target;}public void run() {try {_successfull = _target.increment();} finally {if (!successfull()) {// Fail-fast - interrupts all sleeping threadsThread.currentThread().getThreadGroup().interrupt();}}}public boolean successfull() {return _successfull;}}/*** Performs byte-code modification to remove synchronization from the class under test.*/private static final class CustomClassLoader extends ClassLoader {public Class loadClass(String name) throws ClassNotFoundException {if (name.equals(ThreadSafe.class.getName())|| name.startsWith(ThreadSafeTest.class.getName())) {return defineClass(name);} else {return super.loadClass(name);}}private Class defineClass(String name) throws ClassNotFoundException {// Setup the class file to readClassReader reader = null;try {reader = new ClassReader(getResourceAsStream(name.replace('.', '/') + ".class"));} catch (IOException e) {throw new ClassNotFoundException(name, e);}// Setup an in-memory writer for the byte-`ClassWriter writer = new ClassWriter(false);// Determine if we need to modify the classClassVisitor visitor = writer;if (name.equals(ThreadSafe.class.getName())) {visitor = new ClassModifier(writer);}// And load itreader.accept(visitor, false);byte[] byteCode = writer.toByteArray();return defineClass(name, byteCode, 0, byteCode.length);}}/*** Removes synchronization from a method. Currently only removes the `synchronized` access flag* from the method declaration.*/private static final class ClassModifier extends ClassAdapter {public ClassModifier(ClassVisitor visitor) {super(visitor);}public CodeVisitor visitMethod(int access, String name, String desc, String[] exceptions,Attribute attributes) {int newAccess = access;if ((access & Constants.ACC_SYNCHRONIZED) != 0) {newAccess -= Constants.ACC_SYNCHRONIZED;}return super.visitMethod(newAccess, name, desc, exceptions, attributes);}}}
```

There are two tests defined. One for success and one for failure as I mentioned at the start.

The aptly named `testSucceedsAsis` method executes a number of threads through a single instance of an unmodified class and hopes for the best :-).

Whilst probably non-obvious at first, the method `testFailsWithoutSynchronization` ensures that the test executes against an unsynchronized version of the class. It does this by creating a custom class loader that removes method synchronization through byte-code manipulating.

Some points of note off the top of my head:

* Although it would be almost trivial to add support for removing all synchronization from the code, for purposes of this excercise that wasn't necessary;
* It may be necessary to inject random sleeps into code to try and get it to break as quickly as possible.
* Class loading is notoriously problematic and may become too difficult for more complex classes with many dependencies; and;
* I'm almost positive some AOP weenies will have something to add ;-).

This was largely an academic excercise and even though I clearly made a lot of assumptions, overall I was pretty happy with the outcome. We have demonstrated that it IS possible to test the thread-safety of a class. Whether this approach can be extended usefully to real-world examples remains to be seen. I leave that as an excercise for the reader ;-)
