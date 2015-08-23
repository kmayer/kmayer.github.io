---
layout: theme:post
title: Sencha Touch BDD - Part 5 - Controller Testing
date: 2013-05-18 14:59:04.000000000 -07:00
tags:
- bdd
- jasmine
- javascript
- mobile
- sencha
- Sencha Touch
- testing
---

[Part 4]({% post_url 2013-05-10-sencha-touch-bdd-part-4-phantomjs %}) Introduced PhantomJS as an easy and faster alternative to headful Jasmine testing.
  [Part 3]({% post_url 2013-05-05-sencha-touch-bdd-part-3-testing-views-and-mocking-stores %}) added
  [jasmine-ajax](https://github.com/pivotal/jasmine-ajax/tree/2_0) so we can verify that stores and models react properly to back-end data. We also learned how to use stores to test views, without depending on a back-end server. In
  [Part 2]({% post_url 2013-04-26-sencha-touch-bdd-part-2 %}) I showed you how to unit test Sencha model classes in Jasmine. In
  [Part 1]({% post_url 2013-04-17-sencha-touch-bdd-part-1 %}) I showed you how to set up your Sencha Touch development environment to use the
  [Jasmine](http://jasmine.github.io) JavaScript test framework.


##It’s a control thing, but I will let you understand


Sencha Touch controllers usually live within the context of a single application object. Normally, this is handled for you when you invoke `Ext.Application()` in your app.js file. It creates a singleton object for you in the namespace of your application. For example, if you configured your application’s name to be ‘SenchaBdd’, then the application will be available as the `.app` attribute of the global SenchaBdd object, that is, `SenchaBdd.app`.

Unit testing should not have a running application, however. The point is that we are testing classes in _isolation_. There’s nothing isolated about an integrated, running, Javascript application. There is a relatively simple solution, however; You need to create you own “test” application object that you can then pass as a configuration option when you create controllers under test.

```js
$ cat spec/javascripts/controller/MyControllerSpec.js
describe('SenchaBdd.controller.MyController', function() {
    var controller, app;
    beforeEach(function () {
        app = Ext.create('Ext.app.Application', {name: 'SenchaBdd'});
        controller = Ext.create('SenchaBdd.controller.MyController', { application: app });
        controller.launch();
    });

    afterEach(function() { app.destroy(); })
```

You may want to refactor the application creation and tear-down into a spec helper, to DRY out your tests.


##Test behaviors, not events


It’s tempting to write a Jasmine test that tries to trigger an event in the DOM, then follow the event handling through the application. This is the road to hell. If you find yourself trying to simulate an event, please stop. That is what integration tests are better at doing. Controllers are classes like any other, and you should test methods in the same way. For example, let’s drive out a behavior where, when a user taps on the ‘Buy’ button our application sends a request to the back-end.

```js
describe('SenchaBdd.controller.MyController', function () {
  var controller, app;
  beforeEach(function () {
    app = Ext.create('Ext.app.Application', {name: 'SenchaBdd'});
    controller = Ext.create('SenchaBdd.controller.MyController', { application: app });
    controller.launch();
  });

  afterEach(function () {
    app.destroy();
  });

  it('#newOrder', function () {
    var order = controller.newOrder();
    expect(order.$className).toEqual('SenchaBdd.model.MyModel');
    expect(order.phantom).toBeTruthy();
  });

  describe('#onBuy', function() {
    it('calls save on the order', function() {
      var myOrder = Ext.create('SenchaBdd.model.MyModel');
      spyOn(myOrder, 'save');
      spyOn(controller, 'newOrder').andCallFake(function() {
        return myOrder;
      });

      controller.onBuy()

      expect(myOrder.save).toHaveBeenCalled();
    })
  })
});
```


You might notice that I neither ‘tap’, nor do I test for a ‘POST’ ajax call. The former is better tested through integration tests. The latter is better tested in the model. All the controller need do assert that the model was saved. We trust external classes to function (because they’re tested, too, right?) Testing the #save method on the model follows the same process as testing stores, as I outlined in
[Part3](http://pivotallabs.com/sencha-touch-bdd-part-3/). Another thing to note is that, under test, this controller does not have any views associated with it; Ext.ComponentQuery calls will return empty (undefined) results. This is to be expected in an isolated test, but may make for some head scratching when you first encounter it. If you must test something in the DOM, you should be writing an integration test anyway.

```js
Ext.define('SenchaBdd.controller.MyController', {
  extend:   'Ext.app.Controller',
  config:   {
    views:   ['MyView'],
    refs:    {
      buyButton: 'myview #buyButton'
    },
    control: {
      buyButton: { tap: 'onBuy' }
    }
  },
  newOrder: function () {
    return Ext.create('SenchaBdd.model.MyModel');
  },
  onBuy:    function () {
    this.newOrder().save();
  }
});
```

As a personal preference, and make it easier to test and refactor, I have a #newOrder method that delegates to the model to create a new instance.
