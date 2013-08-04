---
layout: post
title: "Drag & Drop Prioritizable Lists"
alias: /2008/04/drag-drop-prioritizable-lists.html
categories:
---
Yes, it's true, [Scriptaculous](http://script.aculo.us/) already provides a [`Sortable`](http://wiki.script.aculo.us/scriptaculous/show/Sortable.create) that makes it almost trivial to enable drag'n'drop sorting of your HTML lists. Whenever an item is moved an `onUpdate()` event is called (if provided) allowing you to inspect the new order and presumably perform an AJAX request to record the change. In principle, this sounds great but I've never really liked it for a couple of reasons.

For a start, if you have any appreciable number of items updating each in the database just to re-order one seems somewhat unnecessary. Not withstanding the fact that we need to send all those ids to the server in the first place.

Secondly, if you're doing any kind of filtering, it's difficult at best to take the newly constructed ordering and apply that at the back-end; what happens to all the items that may be lurking in between that aren't presently displayed?

Enter `Prioritizable` (itself built on top of `Sortable`).

You use it in much the same way as `Sortable` with the major difference being that the `onUpdate()` event is called with threearguments: the item that was moved, the sibling relative to which it was moved, and the relative position (`"higher"` or `"lower"`). And, if like me, you're feeling a bit RESTful, it's pretty easy to turn these arguments into a nice semantic URL and parameters as shown:

{% highlight javascript %}
Prioritizable.create($("chores"), {
    onUpdate: function(item, position, sibling) {
        id = item.substring(6);                           // "chore_17" => "17"
        sibling_id = to.substring(6);                     // "chore_2" => "2"
        url = "/chores/" + sibling_id + "/" + position;   // "/chores/2/higher"
        new Ajax.Request(url, {
            method: "post",parameters: { id: id }
        });
    }
});
{% endhighlight %}

When the `onUpdate()` event is called we POST the id of the item to be moved to a path constructed from the id of the sibling and the relative position. Assuming the the user moves `chore_17` just above `chore_2` we would POST `"id=17"` to `/chores/2/higher`.

In practice, I combine this client-side behaviour with some server-side code that provides `move_higher_than()` and `move_lower_than()` methods that efficiently handle all the necessary database updates.

All the pieces mentioned will eventually be available alongside [Cogent's](http://www.cogent.co/) other [Rails plugins](https://rubyforge.org/projects/cogent-rails/) but until then, here's enough of the Javascript side of things to get you going.

{% highlight javascript %}
var Prioritizable = {
    create: function(element) {
        options = Object.extend(arguments[1] || {}, {
            onChange: Prioritizable.onChange,
            onUpdate: Prioritizable.onUpdate
        });
        Sortable.create(element, options);
    },
    destroy: function(element) {
        Sortable.destroy(element);
    },
    onChange: function(item) {
        Sortable.options(item)._item = item;
    },
    onUpdate: function(element) {
        options = Sortable.options(element);
        item = options._item;
        options._item = null;
        sibling = item.previous();
        if (other) {
            position = "higher";
        } else {
            sibling = item.next();
            position = "lower";
        }
        options.onUpdate(item, position, sibling);
    }
};
{% endhighlight %}

Enjoy!
