---
layout: theme:post
title: Sencha Touch BDD - Part 3 - Testing Views and Mocking Stores
date: 2013-05-05 14:54:33.000000000 -07:00
tags:
- bdd
- jasmine
- javascript
- mobile
- sencha
- Sencha Touch
- testing
---

In
[Part 1]({% post_url 2013-04-17-sencha-touch-bdd-part-1 %}) I showed you how to set up your Sencha Touch development environment to use the
[Jasmine](http://jasmine.github.io) JavaScript test framework. In
[Part 2]({% post_url 2013-04-26-sencha-touch-bdd-part-2 %}) I showed you to unit test Sencha model classes in Jasmine.

###I don’t normally test views, but when I do

There’s an old MVC mantra: “Fat models, skinny controllers and stupid views.” We don’t want complex logic in our views; that makes them hard to maintain. We don’t want any business logic in our controllers, either. That’s why we have models. Views should be so simple that they don’t require tests. There is a gray area with Sencha, however, where we found testing to be useful in our design process. It has to do with how views interact with stores.

Stores are essentially collections of models. They also encapsulate the persistence layer logic, separate from the business logic of the model. DataViews are a special class of Views in Sencha Touch that will consume a collection to generate a list-type view via a template.

Here are two goals for testing views and stores:

 * Does the view consume the right fields from the store, without hitting the back-end for data?
 * Does the store organize the data from the back-end, i.e. create the interface that is used by the view? Again, without hitting the back-end for data.

###Small steps and iterate

Let’s define a very simple view via tests, we’ll make it work using an in-line store, then we’ll refactor the store into its own class. Next, we’ll refactor the storage class to use a remote back-end.

```js
Ext.require('SenchaBdd.view.MyView');

describe('SenchaBdd.view.MyView', function () {
  it("has a list of colors", function () {
    var view = Ext.create('SenchaBdd.view.MyView', {
      renderTo: 'jasmine_content',
      store:    {
        fields: ['color'],
        data:   [
          {color: 'red'},
          {color: 'green'},
          {color: 'blue'}
        ]
      }
    });

    expect(Ext.DomQuery.select('.favorite-color').map(function (el) {
      return el.textContent
    }).join(', ')).toEqual('red, green, blue');

  });
});
```



Sencha does not come bundled with jQuery, so if you were expecting a DOM query like, $(“.favorite-color”), you might be surprised by the expectation. Ext has its own flavor of querying the DOM, using Ext.DomQuery.select.

Try implementing the view on your own to make the test pass. It should look remarkably similar to this:

```js
Ext.define('SenchaBdd.view.MyView', {
  extend: 'Ext.dataview.DataView',
  xtype:  'myview',
  config: {
    itemTpl: '<div class="favorite-color">{color}</div>'
  }
});
```



In your application, you probably won’t hardwire a store into the view. In fact, you will probably embed this little view inside a larger container, like so (this adds another tab to the sample app):

```diff
--- a/app/view/Main.js
+++ b/app/view/Main.js
@@ -10,6 +10,11 @@ Ext.define('SenchaBdd.view.Main', {

         items: [
+            {
+              title: 'Favorites',
+              iconCls: 'star',
+              xtype: 'myview',
+              store: 'mystore',
+              styleHtmlContent: true
+            },
             {
                 title: 'Welcome',
                 iconCls: 'home',
```



Let’s create a store so our view will show us something:

```js
Ext.define('SenchaBdd.store.MyStore', {
  extend: 'Ext.data.Store',
  config:    {
    storeId: 'mystore',
    fields: ['color'],
    data:   [
      {color: 'red'},
      {color: 'green'},
      {color: 'blue'}
    ]
  }
});
```



In order to see this view, we’ll need to add it to our app.js file:

```diff
--- a/app.js
+++ b/app.js
@@ -31,7 +31,11 @@ Ext.application({
     ],

     views: [
-        'Main'
+        'Main', 'MyView'
     ],

+    stores: [
+            'MyStore'
+    ],

     icon: {
```



Now, let’s go back and refactor our test so it uses MyStore instead of one that was hard-wired.

```diff
--- a/spec/javascripts/view/MyViewSpec.js
+++ b/spec/javascripts/view/MyViewSpec.js
@@ -2,17 +2,16 @@ Ext.require('SenchaBdd.view.MyView');

 describe('SenchaBdd.view.MyView', function () {
   it("has a list of colors", function () {
+    var store = Ext.create('SenchaBdd.store.MyStore', {
+      data:     [
+        {color: 'red'},
+        {color: 'green'},
+        {color: 'blue'}
+      ]
+    });
     var view = Ext.create('SenchaBdd.view.MyView', {
       renderTo: 'jasmine_content',
-      store:    {
-        fields: ['color'],
-        data:   [
-          {color: 'red'},
-          {color: 'green'},
-          {color: 'blue'}
-        ]
-      }
-
+      store:    store
     });
     expect(Ext.DomQuery.select('.favorite-color').map(function (el) {
       return el.textContent
```


All of our tests should remain green, but we’ve removed the “fake” store from our test.


###Let’s get dynamic



Static data stores are boring. The fun starts when you start talking to a back-end API. Let’s say that we have a server that responds to an end point of ‘/colors.json’ with a list of favorite colors. We can even “fake” it by placing a file in the appropriate place. Even so, we don’t want our tests to make network calls to the back-end. That’s not appropriate for unit testing. We’ll use Jasmine’s AJAX mocking helper,
[jasmine-ajax](https://github.com/pivotal/jasmine-ajax/tree/2_0). At the time of this writing, the 2.0 branch had not been merged into the main line, and we need the 2.0 branch in order to work with Ext.

```sh
cd spec/javascripts/helpers
curl -O 'https://raw.github.com/pivotal/jasmine-ajax/2_0/lib/mock-ajax.js'
git add ./mock-ajax.js
```


And we’ll create our first store spec in
`spec/javascripts/store/MyStoreSpec.js`:

```js
describe('SenchaBdd.store.MyStore', function () {
  var store;
  beforeEach(function () {
    jasmine.Ajax.useMock();
    clearAjaxRequests();
    store = Ext.create('SenchaBdd.store.MyStore')
  });

  it('calls out to the proper url', function () {
    store.load();
    var request = mostRecentAjaxRequest();
    expect(request.url).toEqual('/colors.json');
  });
});
```


Notice that I call
`jasmine.Ajax.useMock()` and
`clearAjaxRequests()` in the set up block. This is because I want to wait until the very last moment to turn on ajax mocking. The Ext class loader might still be trying to load a class (via xhr), and the mocker will prevent that from happening. I also clear all previous requests (in case there were any left over, to prevent test polution).

When you run Jasmine, you’ll get a test failure, “TypeError: Cannot read property ‘url’ of null” because we haven’t set up the proxy in the store, yet. Let’s do that.

```js
$ cat app/store/MyStore.js
Ext.define('SenchaBdd.store.MyStore', {
  extend: 'Ext.data.Store',
  config: {
    autoLoad: true,
    storeId:  'mystore',
    fields:   ['color'],
    proxy:    {
      type:       'ajax',
      url:        '/colors.json'
    }
  }
});
```


Now, when you run the test suite, you’ll get a
_different_ error! This is because the default settings for the ajax proxy enables caching, paging, etc. Things we don’t need, so we have to turn them off:

```js
    proxy:   {
      type:       'ajax',
      url:        '/colors.json',
      noCache:    false,
      pageParam:  false,
      startParam: false,
      limitParam: false
    }
```


Now our api test is green! You can even test this in the application by dropping a file, ‘colors.json’ into the public directory.


Let’s add one more test to finish things off.

```js
  it('populates the collection', function () {
    store.load();
    var mockedRequest = mostRecentAjaxRequest();

    mockedRequest.response({
      status:       200,
      responseText: [
        {color: 'red'},
        {color: 'green'},
        {color: 'blue'}
      ]
    });

    expect(store.getCount()).toEqual(3);
    expect(store.getAt(0).get('color')).toEqual('red');
    expect(store.getAt(1).get('color')).toEqual('green');
    expect(store.getAt(2).get('color')).toEqual('blue');
  });
```


Every mocked ajax request has a
`response()` method that you can use to inject your own response, synchronously, to your tests. This confirms that MyStore properly parses and arranges the received data so that it can be presented by the view.


###Validate your mocks



We now have 2 tests that depend on a back-end to respond in a specified way. How do we keep these tests from drifting out of sync? Since we’ve mocked the store in our view test, we might never know if the back-end API changes!


You can wrap Jasmine expectation in functions so that they are reusable. Then we can mix this matcher into our view test to confirm that the store we use there is the same.


Let’s add this function to our
SpecHelper.js file:

```js
function myStoreDataIsValid(store) {
  expect(store.getCount()).toEqual(3);
  expect(store.getAt(0).get('color')).toEqual('red');
  expect(store.getAt(1).get('color')).toEqual('green');
  expect(store.getAt(2).get('color')).toEqual('blue');
}
```



And we’ll replace our existing tests with a single line:

```js
  myStoreDataIsValid(store);
```


We’ll also modify the MyViewSpec.js:

```diff
--- a/spec/javascripts/view/MyViewSpec.js
+++ b/spec/javascripts/view/MyViewSpec.js
@@ -9,6 +9,7 @@ describe('SenchaBdd.view.MyView', function () {
         {color: 'blue'}
       ]
     });
+    myStoreDataIsValid(store);
     var view = Ext.create('SenchaBdd.view.MyView', {
       renderTo: 'jasmine_content',
       store:    store
```



Similarly, we can refactor our colors array into var:
```diff
--- a/spec/javascripts/helpers/SpecHelper.js
+++ b/spec/javascripts/helpers/SpecHelper.js
@@ -16,6 +16,15 @@ afterEach(function () {
     domEl.setAttribute('style', 'display:none;');
 });

+var colorsJSON;
+beforeEach(function() {
+  colorsJSON = [
+    {color: 'red'},
+    {color: 'green'},
+    {color: 'blue'}
+  ];
+});
```



And then use `colorsJSON` in our tests.

```diff
--- a/spec/javascripts/store/MyStoreSpec.js
+++ b/spec/javascripts/store/MyStoreSpec.js
@@ -18,11 +18,7 @@ describe('SenchaBdd.store.MyStore', function () {

     request.response({
       status:       200,
-      responseText: [
-        {color: 'red'},
-        {color: 'green'},
-        {color: 'blue'}
-      ]
+      responseText: colorsJSON
     });
     myStoreDataIsValid(store);
   });
```



and

```diff
--- a/spec/javascripts/view/MyViewSpec.js
+++ b/spec/javascripts/view/MyViewSpec.js
@@ -3,12 +3,9 @@ Ext.require('SenchaBdd.view.MyView');
 describe('SenchaBdd.view.MyView', function () {
   it("has a list of colors", function () {
     var store = Ext.create('SenchaBdd.store.MyStore', {
-      data:     [
-        {color: 'red'},
-        {color: 'green'},
-        {color: 'blue'}
-      ]
+      data:     colorsJSON
     });
+    myStoreDataIsValid(store);
     var view = Ext.create('SenchaBdd.view.MyView', {
       renderTo: 'jasmine_content',
       store:    store
```


### What we’ve managed, so far

The toy app is coming along. We’ve test driven a DataView that consumes a back-end API. We’ve added the mock-ajax library so we can unit tests our stores in isolation. We’ve even seen a few techniques for keeping our mocks from getting out of sync (although, if you’ve been paying attention, I’ve still left a gaping hole, that needs to be plugged).