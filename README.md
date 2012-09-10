[![Build Status](https://secure.travis-ci.org/kmalakoff/knockback.png)](http://travis-ci.org/kmalakoff/knockback)

![logo](https://github.com/kmalakoff/knockback/raw/master/media/logo.png)

Knockback.js provides Knockout.js magic for Backbone.js Models and Collections.

These resources can help you get started:

* [Knockback.js Website](http://kmalakoff.github.com/knockback/)

* [API Docs](http://kmalakoff.github.com/knockback/doc/index.html)

* [Tutorials](http://kmalakoff.github.com/knockback/tutorials_introduction.html)

* [TodoMVC App (Live!)](http://kmalakoff.github.com/knockback-todos-app/)

* [Knockback.js Reference App (Live!)](http://kmalakoff.github.com/knockback-reference-app/) (***new!***): demonstrates best practices when using Knockback.js including page routing and lifecycle management.

* [Knockback-Navigators.js (Live!)](http://kmalakoff.github.com/knockback-navigators) (***new!***): demonstrates page and embedded pane transitions. They are platform-agnostic so you can even use them without using Knockback.js or Knockout.js!


#Download Latest (0.16.1):

Please see the [release notes](https://github.com/kmalakoff/knockback/wiki/Release-Notes) for upgrade pointers.

###Full

Bundles advanced features including: localization, formatting, triggering, and defaults. Stack provides Underscore.js + Backbone.js + Knockout.js + Knockback.js in a single file.

* [Full Library (64k dev)](https://raw.github.com/kmalakoff/knockback/0.16.1/knockback.js)
* [Full Library (8k min+gzip)](https://raw.github.com/kmalakoff/knockback/0.16.1/knockback.min.js)
* [Full Stack (330k dev)](https://raw.github.com/kmalakoff/knockback/0.16.1/knockback-full-stack.js)
* [Full Stack (32k min+gzip)](https://raw.github.com/kmalakoff/knockback/0.16.1/knockback-full-stack.min.js)

###Core

Removes advanced features that can be included separately: localization, formatting, triggering, defaults, and statistics. Stack provides Underscore.js + Backbone.js + Knockout.js + Knockback.js in a single file.

* [Core Library (54k dev)](https://raw.github.com/kmalakoff/knockback/0.16.1/knockback-core.js)
* [Core Library (7k min+gzip)](https://raw.github.com/kmalakoff/knockback/0.16.1/knockback-core.min.js)
* [Core Stack (315k dev)](https://raw.github.com/kmalakoff/knockback/0.16.1/knockback-core-stack.js)
* [Core Stack (31k min+gzip)](https://raw.github.com/kmalakoff/knockback/0.16.1/knockback-core-stack.min.js)

###Dependencies

* [Backbone.js](http://backbonejs.org/)
* [Underscore.js](http://underscorejs.org/)
* [Knockout.js](http://knockoutjs.com/)

###Compatible Components

* [BackboneRelational.js](https://github.com/PaulUithol/Backbone-relational/) - provides helpers for one-to-one and one-to-many relationships between your Backbone.Models.
* [BackboneModelRef.js](https://github.com/kmalakoff/backbone-modelref/) - provides a reference to a Backbone.Model that can be bound to your view before the model is loaded from the server (along with relevant load state notifications).
* [KnockbackNavigators.js](https://github.com/kmalakoff/knockback-navigators/) (***new!***) - provides page and pane navigation including history and state (useful for single-page and mobile apps). Can be used independently from Knockback.js.
* [KnockbackInspector.js](https://github.com/kmalakoff/knockback-inspector/) - provides customizable tree view of models and collections for viewing and editing your data (useful for debugging and visualizaing JSON).


Why Write Knockback.js?
-----------------------

When I was evaluating client-side frameworks, I liked lots of the pieces, but wanted to "mix and match" the best features. I started with [Backbone.js](http://documentcloud.github.com/backbone/) and really loved the Models and Collections, and used [Brunch](http://brunch.io/) to get me up and running quickly.

After a while, I found the view coding too slow so I wrote [Mixin.js](https://github.com/kmalakoff/mixin) to extract out reusable aspects of my views. When I was looking for my next productivity increase, an ex-work colleague suggested [Sproutcore](http://www.sproutcore.com/), but at the time, it wasn't yet micro-frameworky enough meaning I would need to learn something big and "to throw the baby out with the bathwater" as they say (it is hard to give up Backbone models and collections!). Then, I discovered [Knockout](http://knockoutjs.com/) and knew it was for me!

Knockout provided just the right building blocks for a layer between my templates and data. As I used it more, I built additional functionality like [Backbone.ModelRefs](https://github.com/kmalakoff/backbone-modelref) for lazy model loading, localization helpers for truly dynamic views, and most recently, an easier way to sync collections and their model's view models.

So here it is...the refactored and shareable version of my Backbone bindings for Knockout: Knockback.js

Enjoy!

Kevin

An Example
----------

### The view model:

```coffeescript
ContactViewModel = (model) ->
  kb.viewModel(model, {
    name:     'name'
    email:    {key:'email', default: 'your.name@yourplace.com'}
    date:     {key:'date', localizer: LongDateLocalizer}
  }, this)
  @           # must return this or Coffeescript will return the last statement which is not what we want!
```

or (Coffeescript)

```coffeescript
class ContactViewModel extends kb.ViewModel
  constructor: (model) ->
    super(model, {internals: ['email', 'date']})  # call super constructor: @name, @_email, and @_date created in super from the model attributes
    @email = kb.defaultObservable(@_email, 'your.name@yourplace.com')
    @date = new LongDateLocalizer(@_date)
```

or (Javascript)

```javascript
var ContactViewModel = kb.ViewModel.extend({
  constructor: function(model) {
    kb.ViewModel.prototype.constructor.call(this, model, {internals: ['email', 'date']});   // call super constructor: @name, @_email, and @_date created in super from the model attributes
    this.email = kb.defaultObservable(this._email, 'your.name@yourplace.com');
    this.date = new LongDateLocalizer(this._date);
    return this;
  }
});
```

### The HTML:

```html
<label>Name: </label><input data-bind="value: name" />
<label>Email: </label><input data-bind="value: email" />
<label>Birthdate: </label><input data-bind="value: date" />
```

### And...engage:

```coffeescript
view_model = new ContactViewModel(new Backbone.Model({name: 'Bob', email: 'bob@bob.com', date: new Date()}))
ko.applyBindings(view_model)

# ...

# and cleanup after yourself when you are done.
kb.release(view_model)
```

And now when you type in the input boxes, the values are properly transferred to the model and the dates are even localized!

Of course, this is just a simple example, but hopefully you get the picture.



Building, Running and Testing the library
-----------------------

###Installing:

1. install node.js: http://nodejs.org
2. install node packages: 'npm install'

###Commands:

Look at: https://github.com/kmalakoff/easy-bake