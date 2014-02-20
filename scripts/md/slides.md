title: About Me

* [github.com/rjackson](https://github.com/rjackson)
* [twitter.com/rwjblue](https://twitter.com/rwjblue)
* Frequent Ember.js Contributor
* Ember Release Management Team
* General Open Source Fanatic
* Senior Developer with [DockYard](http://dockyard.com)

---

title: Intro

What is Ember.js?

* A framework for creating Ambitious web applications
* Favors convention over configuration
* Open source
* Community Driven

---

title: Intro
subtitle: Major Features

* Two-way Data Bindings
* First-class Router - URL first routing
* Cocoa Style MVC

---

title: Agenda
class: big
build_lists: true

Things we'll cover:

- Object Model Basics
- Templates
- Routes
- Controllers
- Actions

---

title: Object Model Basics
class: big

- Classes
- Computed Properties

---

title: Classes via Ember.Object.extend

<pre class="prettyprint" data-lang="javascript">
var Person = Ember.Object.extend({
  fullName: function(){
    return this.get('firstName') + ' ' + this.get('lastName');
  }
});

var rob = Person.create({
  firstName: 'Robert',
  lastName: 'Jackson'
});

rob.fullName(); // 'Robert Jackson'
</pre>

---

title: Classes via Ember.Object.extend Notes

* Generally all objects in Ember will extend from Ember.Object
* To create an instance of a class, use `.create`
* Pass an object/hash to `.create` to populate with data
* Use `get`/`set` to retrieve or set values.

---

title: Computed Properties

<pre class="prettyprint" data-lang="javascript">
var Person = Ember.Object.extend({
  fullName: function(){
    return this.get('firstName') + ' ' + this.get('lastName');
  }.property('firstName', 'lastName');
});

var rob = Person.create({
  firstName: 'Robert',
  lastName: 'Jackson'
});

rob.get('fullName'); // 'Robert Jackson'
</pre>

---

title: Computed Properties Notes

* Using `.property()` sets up caching of a function's return value
* Specify dependent keys to force invalidation upon changes
* Use `.get('someProperty')` for looking up

---

title: Templates

* Uses Handlebars.js
* Customized to allow two-way data binding

---

title: Templates

<pre class="prettyprint" data-lang="handlebars">
&lt;h2>{{title}}&lt;/h2>
<p>This text is always displayed.</p>

{{#if title}}
  <p>This text is only displayed if a title is present.</p>
{{/if}}

<div>{{input value=title}}</div>
</pre>

[JSBin Demo](http://emberjs.jsbin.com/yah/1/edit?html,javascript,live)

---

title: Templates Notes

* Use `{{` and `}}` to delineate handlebars helpers.
* `if`, `each`, `with`, `bind`, `view`, and MANY more helpers exist.
* Context of the template can change (notice in `#each` example)
* More to come later!

---

title: Basic Route

Route:

<pre class="prettyprint" data-lang="javascript">
App = Ember.Application.create();
App.ApplicationRoute = Ember.Route.extend({
  model: function(){
    return {firstName: 'Robert', lastName: 'Jackson'};
  }
});
</pre>

Template:

<pre class="prettyprint" data-lang="handlebars">
{{! application.hbs}}
&lt;h2>Hi, {{firstName}} {{lastName}}!!&lt;/h2>
</pre>

[JSBin Demo](http://emberjs.jsbin.com/pac/1/edit?html,javascript,live)

---

title: Basic Route Notes

* ApplicationRoute + Template are the starting point
* The `model` hook will be executed upon entering your route
* The return value of the `model` hook is used as the contexted
  of the template.

---

title: Index Route


Route:

<pre class="prettyprint" data-lang="javascript">
App = Ember.Application.create();
App.IndexRoute = Ember.Route.extend({
  model: function(){
    return ['red', 'yellow','blue'];
  }
});
</pre>

Template:

<pre class="prettyprint" data-lang="handlebars">
{{! application.hbs}}
{{outlet}}

{{! index.hbs}}
{{#each}} {{this}} {{/each}}
</pre>

[JSBin Demo](http://emberjs.jsbin.com/juj/1/edit?html,javascript,live)

---

title: Index Route Notes

* `{{outlet}}` indicates where nested resources get rendered
* `{{each}}` allows iterating over a collection
* `this` in a template can be changed (very similar to normal JavasScript)

---

title: Router

<pre class="prettyprint" data-lang="javascript">
App = Ember.Application.create();
App.Router.map(function(){
  this.resource('posts', function(){
    this.resource('post', {path: ':post_id'}, function(){
      this.route('show');
      this.route('edit');
    });
  });
});
</pre>

---

title: Router Notes

* `this.resource` is used for 'nouns'
* `this.route` is used for 'verbs'
* You can pass a `path` to the route/resource for more control over the path.
* Each `path` portion builds upon its parent. In the last example the path for
  `post.show` would be `/posts/1234/show`.

---

title: Using {{link-to}} With Routes

Using the following template with the previous slides router:

<pre class="prettyprint" data-lang="handlebars">
{{! application.hbs}}
{{#link-to 'posts'}}Posts{{/link-to}}

{{outlet}}
</pre>

<pre class="prettyprint" data-lang="handlebars">
{{! application.hbs}}
{{#link-to 'posts.show' 1}}Show Post #1{{/link-to}}

{{outlet}}
</pre>

---

title: Basic Routing Demo
subtitle: <a href="http://emberjs.jsbin.com/mib/1/edit">JSBin</a>
class: segue dark nobackground

---

title: Controller

<pre class="prettyprint" data-lang="javascript">
App = Ember.Application.create();
App.ApplicationRoute = Ember.Route.extend({
  model: function(){
    return {firstName: 'Robert', lastName: 'Jackson'};
  }
});

App.ApplicationController = Ember.ObjectController.extend({
  fullName: function(){
    return this.get('firstName') + ' ' + this.get('lastName');
  }.property('firstName', 'lastName')
});

</pre>

[JSBin Demo](http://emberjs.jsbin.com/pac/2/edit?html,javascript,live)
---

title: Controller Notes

* The router wraps each model in a controller and sets it as the template target.
* As with most things Ember will create a default for you.
* Functions as a decorator, allowing the model to be disconnected from view layer concerns.

---

title: Actions

<pre class="prettyprint" data-lang="javascript">
App = Ember.Application.create();
App.ApplicationRoute = Ember.Route.extend({
  model: function(){
    return Ember.Object.create({name: 'Robert', clickCount: 0});
  },
  actions: {
    incrementCount: function(model){
      model.incrementProperty('clickCount');
    }
  }
});
</pre>

[JSBin Demo](http://emberjs.jsbin.com/pac/3/edit?html,javascript,live)

---

title: Actions

<pre class="prettyprint" data-lang="javascript">
App = Ember.Application.create();
App.ApplicationRoute = Ember.Route.extend({
  model: function(){
    return Ember.Object.create({name: 'Robert', clickCount: 0});
  }
});

App.ApplicationController = Ember.ObjectController.extend({
  actions: {
    incrementCount: function(model){
      model.incrementProperty('clickCount');
    }
  }
});
</pre>

[JSBin Demo](http://emberjs.jsbin.com/pac/4/edit?html,javascript,live)
---

title: Action Notes

* Turns DOM events into events with semantic meaning to your application.
* Actions bubble from the template -> controller -> route -> parent route (all the way up to ApplicationRoute).
* Can accept any number of parameters.

---

title: CRUD'ish Demo
subtitle: <a href="http://emberjs.jsbin.com/mib/2/edit?html,javascript">JSBin</a>
class: segue dark nobackground

---

title: Where can I learn more?

* [Ember.js Guides](http://emberjs.com/guides/)
* [Code School - Warming up with Ember.js](https://www.codeschool.com/courses/warming-up-with-emberjs)
* [EmberWatch](http://emberwatch.com/)
* [Tilde - Introduction to Ember](http://www.tilde.io/events/introduction-to-ember-online/)
