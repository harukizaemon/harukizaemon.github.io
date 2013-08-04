---
layout: post
title: "ActiveRecord Identity Map for Rails Transactions"
alias: /2006/09/activerecord-identity-map-for-rails.html
categories:
---
I happened to be reading [a blog entry](http://www.adam-bien.com/roller/page/abien?entry=ror_and_transactions_the_solution) last night that mentioned some "short comings" in Rails' ActiveRecord and its handling of record loading. Specifically, AR will load the same record twice, into two different instances, within the same transaction. Ie. the following test fails:

```
Customer.transaction doc = Customer.find_by_name('RedHill Consulting, Pty. Ltd.')assert_same c, Customer.find(c.id)end
```

To be honest, I've not _yet_ been burned by this but it may just catch-out some so I quickly whipped up a very basic plugin to see how difficult it would be solve:

```
module RedHillConsultingmodule IdentityMapclass Cachedef initialize@objects = {}enddef put(object)objects = @objects[object.class] ||= {}objects[object.id] ||= objectendendmodule Basedef self.included(base)base.extend(ClassMethods)base.class_eval doalias_method_chain :create, :identity_mapendendmodule ClassMethodsdef self.extended(base)class &lt;&lt; base[:instantiate, :increment_open_transactions, :decrement_open_transactions].each do |method|alias_method_chain method, :identity_mapendendenddef instantiate_with_identity_map(record)enlist_in_transaction(instantiate_without_identity_map(record))enddef enlist_in_transaction(object)identity_map = Thread.current['identity_map']return object unless identity_mapidentity_map.put(object)endprivatedef increment_open_transactions_with_identity_mapincrement_open_transactions_without_identity_mapThread.current['identity_map'] ||= Cache.newenddef decrement_open_transactions_with_identity_mapThread.current['identity_map'] = nil if decrement_open_transactions_without_identity_map &lt; 1endenddef create_with_identity_map()create_without_identity_mapself.class.enlist_in_transaction(self)idendendendend
```

The code essentially interferes with `create` and `instantiate` (called from `find`) and ensures that, within a transactions, the same record will always be returned for the same id ([IdentityMap](http://www.martinfowler.com/eaaCatalog/identityMap.html)).

As I mentioned, unlike all my other plugins, I've never used nor needed to use this one -- and I'm not sure I will unless it proves to be a problem for me -- but it's yet another example of how easy it is to extend Rails to do pretty much whatever you might imagine.
