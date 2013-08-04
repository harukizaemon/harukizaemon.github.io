---
layout: post
title: "Patching Rails"
alias: /2005/12/patching-rails.html
categories:
---
Last night I was writing some database migration scripts and I wanted to add a not-null column to a table. The migration stuff is so cool that I was able to write something like this:

```
class AddExternalIdToListings < ActiveRecord::Migrationdef self.upadd_column :listings, :external_id, :string, :limit => 10, :null => trueadd_index :listings, [:external_id], :unique => trueListing.reset_column_informationListing.find(:all).each do |l|l.external_id = l.idl.save!endchange_column :listings, :external_id, :string, :limit => 10, :null => falseenddef self.downremove_column :listings, :external_idendend
```

This script first adds the new column allowing nulls (`:null => true`) then creates a unique index on it, updates each record to give the new column a value and finally modifies the column to make it not null (`:null => false`).

All well and good except for one thing: it didn't seem to modify the column as expected. Looking at the source code for the PostgreSQL adapter, I could see that the code `change_column()` has (at least) two bugs. (Ok, strictly speaking one bug and one omission.) The first bug means that it never actually executes the intended statement. The second (the omission) means that even if the statement were executed, most of the specified options (in this case the `:null => false`) would have been completely ignored.

Enter the wonder of Ruby and extended/modifying classes on the fly.

Disclaimer: I'm no Ruby nor Rails expert. I'm sure there's a "better" way to do this so if anyone wants to take this code and submit it as a proper patch, then please, please, please go for it. For my needs, this does the job nicely.

Rails allows you to add functionality via [Plugins](http://wiki.rubyonrails.com/rails/pages/Plugins). In this case, I decided to make a "patches" plugin that will allow me to easily override or extend rails to get around any bugs (or omissions) that I encounter in my travels.

So I ended up with two files. The first `vendor/plugins/patches/init.rb` is executed by rails. In my case it contains only one line:

```
require 'postgresql_adapter_patches'
```

This simply loads the second file (`vendor/plugins/patches/lib/postgresql_adapter_patches.rb`) containing the actual code that performs all the black-magic (although it's actually fairly straight-forward):

```
module ActiveRecordmodule ConnectionAdaptersclass PostgreSQLAdapterdef change_column(table_name, column_name, type, options = {})native_type = native_database_types[type]sql_commands = ["ALTER TABLE #{table_name} ALTER #{column_name} TYPE #{type_to_sql(type, options[:limit])}"]if options[:default]sql_commands &lt;&lt; "ALTER TABLE #{table_name} ALTER #{column_name} SET DEFAULT '#{options[:default]}'"endif options[:null] == falsesql_commands &lt;&lt; "ALTER TABLE #{table_name} ALTER #{column_name} SET NOT NULL"endsql_commands.each { |cmd| execute(cmd) }endendendend
```

In essence, all it really does is override the definition of the `change_column()` method in the `ActiveRecord::ConnectionAdapters::PostgreSQLAdapter` class. In my case, I just copied the code from `add_column()` and modified it to suit my needs. Of course this is a blatant violation of the [DRY Principle](http://www.artima.com/intv/dry.html) but hey, shoot me :-).

All in all I'm impressed by how trivial the whole processs turned out to be and has made me think about writing some _actual_ plugins; I'll leave it to the experts to fix it for good ;-).
