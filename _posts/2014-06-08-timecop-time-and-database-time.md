---
layout: post
title: Timecop time and database time
date: 2014-06-08 12:36:35.000000000 -07:00
tags:
- rails
- testing
---


[Timecop](https://github.com/travisjeffery/timecop) is a great addition to your testing toolbox, but if you've ever tried to use Timecop to interact with a database, and then got the most mysterious of existential errors:

```
expected: Sun, 08 Jun 2014 19:18:22 UTC +00:00
     got: Sun, 08 Jun 2014 19:18:22 UTC +00:00
```

I feel your pain.

The trouble comes from the fact that Timecop is freezing time at the resolution of the native Time class, which on my Mac OS X machine is microseconds (aka "usec"), but the database, in this case MySQL only records time to the second. RSpec is trying to be helpful with its matchers by calling `<code>`#to_s`, but that hides the significant, sub-second difference.


Here's a quick little monkey patch that I added to my spec support:


```ruby
class Timecop
  def self.database_compatible_time(now = Time.zone.now)
    now.change(usec: 0)
  end
end
```


This uses a Rails helper to reset the microsecond portion of the time object. So, instead of


```ruby
now = Time.zone.now

Timecop.freeze(now) do
  ...
end
```


I do this:


```ruby
now = Timecop.database_compatible_time

Timecop.freeze(now) do
  ...
end
```


And no more head scratching. It's sort of database specific, so your mileage may vary.

