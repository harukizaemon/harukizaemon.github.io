---
layout: post
title: "Quickly Migrate all database times to UTC"
alias: /2008/06/quickly-migrate-all-database-times-to.html
categories:
---
If you're thinking about updating to Rails 2.1 to get the timezone support, you'll need to update all database records to UTC. Here's a quick migration script to do just that:

{% highlight ruby %}
class ConvertTimestampsToUtc < ActiveRecord::Migration
  # Assume all times were in UTC+10:00
  OFFSET = "interval '10 hours'"

  # Adjust any date/time column
  COLUMN_TYPES = [:datetime, :timestamp]

  def self.up
    adjust("-")
  end

  def self.down
    adjust("+")
  end

  private

  def self.adjust(direction)
    connection = ActiveRecord::Base.connection
    connection.tables.each do |table|
      columns = connection.columns(table).select { |column| COLUMN_TYPES.include?(column.type) }
      updates = columns.map { |column| "#{column.name} = #{column.name} #{direction} #{OFFSET}"}.join(", ")
      execute("UPDATE #{table} SET #{updates}") unless updates.blank?
    end
  end
end
{% endhighlight %}

As you can see, I've assumed that the dates were previously stored as AEST (UTC+10:00) so you'll likely need to adjustthat and I'm also assuming PostgreSQL for date manipulation though it should be pretty simple to convert to run under MySQL. It may even work asis.
