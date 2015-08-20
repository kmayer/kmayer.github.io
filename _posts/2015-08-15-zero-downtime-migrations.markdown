---
layout: theme:post
title: "Zero Downtime Migrations"
date: 2015-08-15T19:35:44-07:00
---

For any sufficiently large application, you want to minimize interruptions to service
while deploying new code. This is especially challenging for migrations on Heroku and
other Platforms-as-a-Service, where you have a Catch-22 problem; you want to run your
migrations, *first*, but you can't run them until you've deployed the migrations to
production. Usually, at this point, your new migration-dependent features are in the
deploy, too. So when your application restarts, and the migration hasn't run, yet,
your poor application will trip some exceptions, and perhaps, create problems to your
users. Of course, you can put the app into maintenance mode, but that creates more
downtime for your users.

The technique described here will allow you to gather your migrations into a separate,
deployable, commit, that you can run *before* you deploy your features, thus minimizing
downtime.

Some notes:

* We try _very_ hard to make migrations non-destructive so there's no rollback required. The alternative is much more complicated.
* This also assumes that all QA tests are complete and the next task on your checklist is to deploy to production.
* The `production` [git] remote is on Heroku. It can be named anything, but we call it that.
* We don't use GitFlow, instead, the head of `origin/master` is always production-ready. If you _are_ using GitFlow, I'm sure that this is still applicable.

## The Recipe

#### 0. On a clean, working copy of your code.

```sh
git checkout master
```
    
#### 1. Create a patch file of all migrations, added since your last production deploy:

{% highlight sh %}
git fetch production
git diff production/master... -- 'db/migrate/' > patch0
{% endhighlight %}

#### 2. Check out a branch, starting at _production/master_'s head.

{% highlight sh %}
git checkout production/master -b pre-deploy-migrations
{% endhighlight %}

#### 3. Reset your development database to current state of the production database.

> *WARNING* This will delete *all* of your data in your development environment. But
  that should not be a problem, right? If it is, you have a problem with your
  development environment.

{% highlight sh %}
rake db:reset
{% endhighlight %}

This also assumes that you've been diligent, OCD-diligent, about keeping your `schema.rb` file synchronized with production.

#### 4. Apply the patch file.

{% highlight sh %}
patch -p1 < patch0
{% endhighlight %}

#### 5. Run your migrations.

{% highlight sh %}
rake db:migrate
{% endhighlight %}

#### 6. Add and commit your changes.

{% highlight sh %}
git add db/migrate/*
git add db/schema.rb
git commit -m "pre-deploy migrations only"
git status # => should be "reasonably" empty
{% endhighlight %}

#### 7. Re-run your test suite to make sure nothing 'asplodes.

> What happens if your tests *do* break? __You might have a destructive migration!__
  For example, I recently changed a
  table from single to polymorphic ownership. Instead of adding the 
  columns, `owner_id` and `owner_type`, I renamed `organization_id` to 
  `owner_id`. A whole bunch of tests broke. 
  If you continue with these steps, your app will crash when you run the 
  migrations. See Appendix A for work-arounds.

#### 8. Compare your commit against master.

Make sure that nothing snuck in by accident. Conceptually, `schema.rb` will represent the
state of the database after the migrations have been run, and should exactly match the
schema that we want to have for our new feature.

{% highlight sh %}
git diff head..origin/master -- 'db/migrate/' # => This should return zero changes!
git diff head..origin/master -- 'db/schema.rb' # => This should return zero changes!
{% endhighlight %}

> There __might__ be differences that are okay. You'll have to do a manual code
  review to be sure.

#### 9. Push __just__ this commit into production.

You can do it by pushing the branch, or merging the commit into your deploy / mainline, rebase it *ahead* of your other commits, then push just that sha. We use the `heroku_san` gem, so, for us, it is a simple shell command (while still on this branch).

{% highlight sh %}
rake production deploy
{% endhighlight %}
   
#### 10. Run your migrations on your production instance.

{% highlight sh %}
heroku run rake db:migrate -r production
{% endhighlight %}

#### 11. Merge the branch into master.

It should be a no-op since the code is already there. This is optional, but it keeps git happy for the next deploy.

{% highlight sh %}
git checkout master
git merge -m "Merge branch 'pre-deploy-migrations'" pre-deploy-migrations 
git push origin master
{% endhighlight %}

#### 12. You are ready to deploy to production.

{% highlight sh %}
rake production deploy
{% endhighlight %}

#### 13. Clean up.

{% highlight sh %}
git branch -D pre-deploy-migrations
rm patch0
{% endhighlight %}

#### 14. Bask in the glow of your mastery!

---

## Appendix A: How to Handle Deprecated Columns

Don't remove them! That would break your existing production code and any rollbacks
become infinitely more difficult. Many practitioners *never* remove old columns, but 
I think that's bad too. Code costs. Maintaining these extra columns, over time, will cost
you time and money. Here's a way to "soft" remove columns until you are *sure* that
you won't need them again. I schedule a chore a few iterations in the future with the
task of doing the actual removal.

{% highlight ruby %}
class YourModel < ActiveRecord::Base
  ...
  # TODO: https://www.pivotaltracker.com/story/show/...
  def self.columns
    super.reject { |c| c.name == "_column_name_" }
  end
  ...
end
{% endhighlight %}

## Appendix B: Copy Data to New Columns

Making copies to a new column is safe (and "disks" are "cheap"), although it does open a
window where your data will be inconsistent. In your migrations, make SQL calls to copy
a column from the old to the new. There is a down side to this, especially on very large 
tables. You want tall, narrow tables for efficiency and performance. This does neither. 
So plan accordingly.

{% highlight ruby %}
class YourMigration < ActiveRecord::Migration
  def change
    add_column :_table_name, :_new_column_name_, :string

    ActiveRecord::Base.connection.execute <<-SQL
      UPDATE _table_name_ SET _new_column_name_ = _old_column_name_;
    SQL
{% endhighlight %}

If you are using MySQL, you might need the help of the Large Hadron Migrator to keep your site up and running while you copy over the data.
