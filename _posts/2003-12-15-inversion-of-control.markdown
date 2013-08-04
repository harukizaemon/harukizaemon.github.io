---
layout: post
title: "Inversion of Control"
alias: /2003/12/inversion-of-control.html
categories:
---
_["I was expecting a paradigm shift and all I got was a lousy constructor!"](http://tinyurl.com/wj7p)_

[Perryn Fowler](http://www.jroller.com/page/perryn) has asked me how would we use the [Clock]({% post_url 2003-12-07-when-is-a-clock-not-a-clock %})? How do we create one for production? The [Inversion of Control (IoC)](http://javangelist.snipsnap.org/space/IoC+Introduction) fraternity frown upon factories so what are we to do?

Luckily, software developed using TDD tends to lend itself to IoC.

This example conforms just fine with so called "Type-3" IoC which is based on passing dependencies via constructors. It's a very simplistic example nonetheless. For more complex systems, you may want to check out IoC containers such as [PicoContainer](http://www.picocontainer.org) et. al. which seem to be gaining in popularity. And for good reason.

These containers allow you to easily configure components and the dependencies between them. Although I must admit that I'm not a fan of the hype as some people have been building systems like this for a while now. The up-side of course is that others will hopefully feel more comfortable looking into it as an alternative now that it has a snazzy name and the obligatory TLA :-)

Once you get your head around it, it's pretty easy to see why people love the concept of IoC containers so much. In some ways it's not that much different to [plugins](http://www.martinfowler.com/eaaCatalog/plugin.html) but it does lend itself to other cool stuff, like decorating and "passivating". No more JNDI lookups, no more creating endless factories with static methods. But as I said it can be hard to see at first how that works.

So here is the simplest example I could think of and whip up in 20 minutes. It displays a panel into which you can enter a duration to wait (in milliseconds) and a message to display once the time has elapsed.

It hopefully demonstrates how we can now use `Alarms` knowing they are fully tested and how we don't need a static factory to create anything.

{% highlight java %}
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class Main extends JFrame {
    private final JTextField _durationMillisField = new JTextField("3000");
    private final JTextField _messageField = new JTextField("Message!");
    private final JButton _setButton = new JButton("Set");
    private final Clock _clock;
    public Main(Clock clock) throws HeadlessException {
        if (clock == null) {
            throw new IllegalArgumentException("clock can't be null");
        }
        _clock = clock;
        getContentPane().setLayout(new GridLayout());
        getContentPane().add(_durationMillisField);
        getContentPane().add(_messageField);
        getContentPane().add(_setButton);
        _setButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent actionEvent) {
                setClicked();
            }
        });
        setSize(300, 50);
    }

    private void setClicked() {
        int durationMillis = new Integer(_durationMillisField.getText()).intValue();
        Popup popup = new Popup(_messageField.getText());
        Alarm alarm = new Alarm(popup, _clock.getCurrentTimeMillis() + durationMillis, _clock);
        new Thread(alarm).start();
    }

    private class Popup implements Runnable {
        private final String _message;
        public Popup(String message) {
            if (message == null) {
                throw new IllegalArgumentException("message can't be null");
            } _message = message;
        }

        public void run() {
            JOptionPane.showMessageDialog(Main.this, _message);
        }
    }

    public static void main(String[] args) {
        new Main(new SystemClock()).show();
    }
}
{% endhighlight %}

Nothing too complex here. It doesn't go into testing GUIs (a topic for a book perhaps?) but as you can see, it would be possible to break down even this example into smaller chunks for testing. But really, the only thing the `JFrame` does is convert from screen values to primitives in order to create `Alarms`.

(As an aside, my original example had a `Scheduler` that created `Alarms` and set them running in a `Thread` but again I thought I'd ke it as simple as I could so I simply put the code into `Main`.)

In some ways it seems almost trivial really. But if you can imagine a whole system done this way it's a pretty cool way to build apps. In a bizarre way, it's kind of like rule based systems such as CLIPS or JESS only instead of declarative rules that magically get evaluated, in this case, the dependencies are declarative and are magically resolved at run time.

Take a look at the other examples on the net and drop me a line if you want me to put together a more complex example using say PicoContainer or NanoContainer.
