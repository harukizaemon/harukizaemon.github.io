---
layout: post
title: "Auto-Generate Foreign-Key Constraints in Rails"
alias: /2006/05/auto-generate-foreign-key-constraints-in-rails.html
categories:
---
I literally just lobbed into my hotel room in Oslo after 30+ hours in transit. (I left home @ 12:30pm on Monday 15th and arrived here @ 21:30 on Tuesday 16th Melbourne time.) It's my first time in Scandinavia and so far I'm loving it. The people are friendly (and *ahem* rather attractive I must say) and I'm hanging out to try some scandinavian beer!

So anyways, after resisting the temptation to turn on my laptop, I finally caved and whipped up a little bit of code to auto-generate foreign-key relationships for migration scripts:

{% highlight ruby %}
module ActiveRecord
  module ConnectionAdapters
    class ColumnDefinition
      alias nofk_to_sql :to_sql

      def to_sql
        name.to_s =~ /(.*)_id$/ ? "#{nofk_to_sql} REFERENCES #{Migrator.proper_table_name($1)} (id)" : nofk_to_sql
      end
      alias to_s :to_sql
    end
  end
end
{% endhighlight %}

The code assumes that if you have a column named `customer_id` in say an `order_s_` table, then you want a foreign key to the `id` column in the `customer_s_` table. <strike>That doesn't handle situations where you have multiple foreign keys to the same table but...meh...I don't have models that sophisticated yet so bite me :)</strike>

Just looking back on it now, it was so trivial to implement that there is more syntactic _noise_ than _actual_ code. Hmmmm.

**Update:** the plugin is now available in [downloadable form](https://github.com/harukizaemon/redhillonrails/tree/master/foreign_key_migrations) and supports a **:references** option for multiple columns or columns that aren't named for the table they reference.
