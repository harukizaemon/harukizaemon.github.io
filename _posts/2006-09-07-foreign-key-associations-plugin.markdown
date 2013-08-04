---
layout: post
title: "Foreign Key Associations Plugin"
alias: /2006/09/foreign-key-associations-plugin.html
categories:
---
I've done quite a bit of refactoring of my [Ruby on Rails plugins](https://github.com/harukizaemon/redhillonrails) lately which, unfortunately, broke some stuff (thanks to all those that let me know) but the upshot is a much cleaner division of responsibility between plugins; and some sorely needed unit tests.

Another of the benefits from all of this was yet [another plugin](https://github.com/harukizaemon/redhillonrails/tree/master/foreign_key_associations), this time to automatically generate associations based on foreign-keys.

For example, given a foreign-key from a `customer_id` column in an `orders` table to an `id` column in a `customers` table, the plugin generates:

* `Order.belongs_to :customer`; and
* `Customer.has_many :orders`.

<strike>(In the near future we intend to support `has_one` associations for foreign-key columns having a unique index.)</strike>.

If there is a uniqueness constraint -- eg unique index -- on a foreign-key column, then the plugin will generate a `has_one` instead of a `has_many`.

For example, given a foreign-key from an `order_id` column with a uniqueness constraint in an `invoices` table to an `id` column in an `orders` table, the plugin generates:

* `Invoice.belongs_to :order`; and
* `Order.has_one :invoice`.

You can download the latest version directly from `svn://rubyforge.org//var/svn/redhillonrails/trunk/vendor/plugins/foreign_key_associations`

For all those that have asked for pure HTTP access, I hear you and I'm working on it. (It seems `./script/plugin install` doesn't understand the format of the _browse repository_ pages on RubyForge. DOH!)
