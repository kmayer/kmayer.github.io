---
layout: post
title: Sencha Touch BDD - Part 4 - PhantomJS
date: 2013-05-10 14:57:14.000000000 -07:00
tags:
- bdd
- jasmine
- mobile
- sencha
- Sencha Touch
- testing
---

[Part 3]({% post_url 2013-05-05-sencha-touch-bdd-part-3-testing-views-and-mocking-stores %}) added `jasmine-ajax` so we can verify that stores and models react properly to back-end data. We also learned how to use stores to test views, without depending on a back-end server. In [Part 2]({% post_url 2013-04-26-sencha-touch-bdd-part-2 %}) I showed you how to unit test Sencha model classes in Jasmine. In [Part 1]({% post_url 2013-04-17-sencha-touch-bdd-part-1 %}) I showed you how to set up your Sencha Touch development environment to use the <a href="http://jasmine.github.io">Jasmine</a> JavaScript test framework.

### I hear it’s all about the cloud these days

Not only do we use Test Driven Development, we test all the time (see also <a href="https://www.google.com/search?q=tatft">TATFT</a>). In fact, continuous testing is a key component in a well-run agile practice. Pivotal developed <a href="http://github.com/pivotal-ciborg">ciborg</a> (previously known as Lobot), to automatically deploy <a href="http://jenkins-ci.org/">Jenkins</a> servers in the cloud. Jasmine has a continuous integration target in its rake tasks, <code>rake jasmine:ci</code>. If you’ve never run it before, now is a good time to try it. You’ll notice that Jasmine launches an instance of the Firefox browser, runs the test suite, then reports the results. That’s great if you’ve got (1) Firefox and (2) a machine that has a <em>display</em>! When you run your CI tests in the cloud, the virtual machines may not be configured with a virtual display. Furthermore, the process is terribly slow.

In this installment, we’ll see how to mix in PhantomJS so we can run our Jasmine tests without the need for a visual browser. PhantomJS is orders of magnitude faster, too.

### Do the install dance

#### 0. Add ‘jasmine-phantom’ to your <code>Gemfile</code>

```diff
index e6b5daf..612fb25 100644
--- a/Gemfile
+++ b/Gemfile
@@ -2,3 +2,4 @@ source "https://rubygems.org"

 gem "rake"
 gem "jasmine"
+gem "jasmine-phantom"
```

and modify your <code>Rakefile</code> to load the PhantomJS tasks:

```diff
--- a/Rakefile
+++ b/Rakefile
@@ -2,6 +2,7 @@
 begin
   require 'jasmine'
   load 'jasmine/tasks/jasmine.rake'
+  load 'jasmine-phantom/tasks.rake'
 rescue LoadError
   task :jasmine do
```

Finally, run <code>bundle install</code>

Now, when you run <code>rake -T</code> from the command line, you’ll see a new target:

```sh
$ rake -T jasmine
rake jasmine             # Run specs via server
rake jasmine:ci          # Run continuous integration tests
rake jasmine:phantom:ci  # Run jasmine specs using phantomjs and report the results
```

### Run rake

Running it will the phantom:ci target will start the Jasmine continuous integration tests in PhantomJS instead of Firefox.

```sh
$ rake jasmine:phantom:ci 2>1 /dev/null
[2013-05-06 20:28:15] INFO  WEBrick 1.3.1
[2013-05-06 20:28:15] INFO  ruby 1.9.3 (2012-11-10) [x86_64-darwin12.2.0]
[2013-05-06 20:28:15] INFO  WEBrick::HTTPServer#start: pid=17061 port=60080
Waiting for jasmine server on 60080...

8 specs | 0 failing
```