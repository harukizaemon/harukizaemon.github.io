---
layout: post
title: "Transactional DDL"
alias: /2006/05/transactional-ddl.html
categories:
---
Have you ever been half way through a rails migration script only to have it bomb out with some error or another? You then correct the error and try to re-run it but because some of the statements have already been executed they fail when executed a second time.

Some databases (PostgreSQL for example) allow you to wrap DDL (Data Definition Language, `create table`, etc.) as well as DML (Data Manipluation Language, `insert`, etc.) in a transaction! This means that if any part of the script fails, the whole lot is rolled back. Imagine this: you drop a table, script rolls back and the table magically re-appears as if nothing had happened.

In rails migration scripts it's a simple matter of wrapping the entire `up` and/or `down` method with a `_Model_.transaction do ... end`. (Yet another time I really think the idea of making transactions a part of the model is particularly ridiculous. Transactions are cross-cutting concerns just like logging. Just ask the Aspect weenies, they'll tell ya.)

Here's a simple example:

``` ruby
def self.up
  SystemProperty.transaction do
    create_table :system_property do |t|
      t.column :name,         :text,      :null => false
      t.column :value,        :text,      :null => false
      t.column :created_at,   :datetime,  :null => false
      t.column :updated_at,   :datetime,  :null => false
      t.column :lock_version, :integer,   :null => false, :default => 0
    end
    add_index :system_property, [:name], :unique => true
  end
end
```

Now, I realise that `create_table` also supports the `:force => true` option so this is possibly not the best example but at least you can see _how_ to go about making your migration scripts fully transactional, DDL and all.
