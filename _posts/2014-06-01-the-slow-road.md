---
layout: theme:post
title: The slow road
date: 2014-06-01 08:00:37.000000000 -07:00
tags:
- database
- heroku
- rails
---

Once a week, we run a query on our production system that takes a very long time to complete. Invariably, it times out because the default settings on Heroku are (quite reasonably) for web / API interactions; anything longer than 30 seconds is too long and the connection should be closed to let other requests through. How do you get around this, then? You could set up your `database.yml` file so that you connect to your production database from a developer workstation. This is actually quite useful, but opens up a hellacious security hole. Let's not get started about the consequences of accidentally running `rake db:drop:all`! Moreover, you want your system to be automated as much as possible, and that means running your tasks on a Heroku instance. Fortunately, with a little hackery, you can modify the connection timeout settings within your application. Do this carefully, and make sure that it can not leak into your running web application dynos.

```ruby
ActiveRecord::Base.connection.raw_connection.query_options.tap do |opts|
  opts[:read_timeout] = 600
end

ActiveRecord::Base.connection_pool.with_connection do
  # ... long running database queries go here ...
end
```

These settings are database specific, so read the documentation / source. This connection is using the `mysql2` adapter.