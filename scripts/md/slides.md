title: About Me

* [github.com/rjackson](https://github.com/rjackson)
* [twitter.com/rwjblue](https://twitter.com/rwjblue)
* Frequent Ember.js Contributor
* Ember Release Management Team
* General Open Source Fanatic
* Senior Developer with [DockYard](http://dockyard.com)

---

title: Intro
subtitle: What is the Resolver
build_lists: true

* MAGIC
* Looks up the code in YOUR application.
* It is responsible for converting a "name" in your application into the actual class/function that Ember needs.

---

title: Different Types of Resolver

* The `Ember.DefaultResolver` is used with a stock Ember build. It looks up your classes & templates on the main namespace global.

* The `ember-jj-abrams-resolver` is used by Ember App Kit and Ember Appkit Rails to lookup classes & templates using ES6/AMD modules.

---

title: `Ember.DefaultResolver`
subtitle: How does it work for classes?


<pre class="prettyprint" data-lang="javascript">
function resolve(name, type, global) {
  var className = Ember.String.classify(name) + Ember.String.classify(type),
      factory = Ember.get(global, className);

  return factory;
}

resolve('application', 'route', App); //=> Looks up App.ApplicationRoute
resolve('post', 'controller', App);   //=> Looks up App.PostController

</pre>

---

title: `Ember.DefaultResolver`
subtitle: How does it work for templates?


<pre class="prettyprint" data-lang="javascript">
function resolveTemplate(templateName) {
  if (Ember.TEMPLATES[templateName]) {
    return Ember.TEMPLATES[templateName];
  }

  templateName = Ember.String.decamelize(templateName);
  if (Ember.TEMPLATES[templateName]) {
    return Ember.TEMPLATES[templateName];
  }
}

resolveTemplate('application'); //=> Looks up Ember.TEMPLATES['application']

</pre>

---

title: `ember-jj-abrams-resolver`
subtitle: How does it work?

<pre class="prettyprint" data-lang="javascript">
function resolve(prefix, type, name) {
  var moduleName = prefix + '/' + type + 's/' + name;
  
  return require(moduleName);
}

resolve('appkit', 'route', 'application'); //=> Looks up appkit/routes/application
resolve('appkit', 'post', 'controller');   //=> Looks up appkit/controllers/post

---

title: Customizing

<pre class="prettyprint" data-lang="javascript">
var MyResolver = Ember.DefaultResolver.extend({
  resolveRoute: function(){
    return Ember.Route.extend();
  }
});

window.App = Ember.Application.create({
  Resolver: MyResolver
});
</pre>

---

title: Where can I learn more?

* https://github.com/emberjs/ember.js/blob/master/packages/ember-application/lib/system/resolver.js
* https://github.com/stefanpenner/ember-jj-abrams-resolver/blob/master/packages/ember-resolver/lib/core.js
