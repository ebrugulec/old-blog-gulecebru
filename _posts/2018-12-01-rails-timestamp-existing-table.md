---
layout: post
title: Add timestamps to existing table in db Rails 5+
date:   2018-12-01 19:07:19
modified:  2018-12-01
comments: true
---

When you forget to add timestamps in your table, you can generate migration. But you will get a little error. Like this:

```ruby
def change_table
    add_column :users, :created_at, :datetime, null: false
    add_column :users, :updated_at, :datetime, null: false
  end
>> Cannot add a NOT NULL column with default value NULL: ALTER TABLE “table_name” ADD “created_at” datetime NOT NULL
```

Because we created data without timestamps before. But you can handle it pass the value all model.

```ruby
def change
  add_timestamps :animals, null: true 
long_ago = DateTime.new(2000, 1, 1)
  Animal.update_all(created_at: long_ago, updated_at: long_ago)

  change_column_null :animals, :created_at, false
  change_column_null :animals, :updated_at, false
end
```

We did it. Cong. 🎊

[Helpfull Resource](http://stackoverflow.com/questions/46520907/add-timestamps-to-existing-table-in-db-rails-5?rq=1)