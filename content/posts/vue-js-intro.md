---
title: "A Vue of things"
date: 2019-09-08T19:52:00+08:00
draft: true
---

I previously worked on a website application using [Bootstrap](https://getbootstrap.com) and [jQuery](https://jquery.com). So naturally I gravitate towards these frameworks whenever I do web coding. But it turns out that I never considered [Vue.js](https://vuejs.org) before, and I always thought of Vue.js as something like Bootstrap (I'm wrong, by the way). Vue.js is something more like jQuery in addition to other equally impressive frameworks like [React](https://reactjs.org) and [Angular](https://angular.io). I actually wanted to learn React on my own, but since I have to use Vue this time around, I decided to learn it instead.

All web frameworks have one thing in mind: binding method to DOM events. In jQuery, this is done using a dot syntax:

```javascript
$('#element_id').on('event', function() {
  /* ... */}
);
```

To me, this is quite intuitive because I've been working using jQuery for some time already, thus when I first saw how Vue.js did this, I did not really fancy it.

```javascript
var app = new Vue({
  el: '#element_id',
  data: {  
    message: 'An example'
  },
  methods: {
    do_it: function () {
      /* ... */
    }
  }
})
```

It expresses very much the same thing that the above jQuery snippet does. However, there are fundamental differences that undergird the difference in syntax. Vue.js uses a Model-View-ViewModel architecture, which evolved from the traditional Model-View-Controller (MVC) architecture that was first pioneered by Rails.

![The Model-View-ViewModel or MVVM architecture](/vue-js-intro-mvvm.png)

In the MVVM architecture,

* The view interacts with the user. It is the UI.
* The view-model contains all of the application's data.
* The model retains the persistence of data.

There is a binding of data between the view and view-model with a *binder*. The binder exposes data to the view as properties. Views then interact with the data by calling methods that are exposed by the binder. The view-model replaces the controller. The reason why the controller can now be replaced is because of the ability of JavaScript/HTML to do asynchronous updates. This allows the application to be reactive because it can update the user on components, rather than sending out a server request to the backend everytime there is a change. Of course, the backend can still serve up a database running on the server, and this would still constitute the 'model'.

In this post, I will only cover the very fundamental basics of Vue. The neat thing about Vue is that it is a progressive framework, which means that there's just enough to bootstrap an application, but it comes with batteries included so that we can build a full-fledged web application if necessary.

## Basics

Usually, each web page contains one Vue application instance. When this application is instantiated, it starts the Vue lifecycle. What happens during the life cycle is that Vue creates observers for the data in the view-model, renders the HTML and makes a virtual copy of the DOM and then mounted onto the DOM, starts an event loop to monitor the DOM, before finally entering the 'destructive phase', wherein all the observers, event listeners, and child components are destroyed and the entire app is then terminated.

```javascript
var vm = new Vue({
  // options
})
```

The fourfold process is:

1. Create observers
2. Mount the DOM
3. Create the event loop (updates are hooks)
4. Destruction

### Binding DOM events

We ensure interactivity by binding JavaScript methods to DOM events, such as a user clicking on a button. This is the same in jQuery. In Vue, the syntax uses a `v-on` directive (more on what directives are later).

```html
<button v-on:click="doSomething">Click here</button>

<script type="text/javascript">
  var vm = new Vue({
    el: "#app",
    data: {},
    methods: {
      doSomething: function () {
        return "Something was done";
      }
    }
  })
</script>
```

1. `doSomething` can be the name of an anonymous function that has been assigned to a variable.
2. Or, `doSomething` can actually be inline JavaScript code.

#### `methods` versus `computed`

This segment was adapted from Bert's reply found in a Stack Overflow [question](https://stackoverflow.com/questions/44350862/method-vs-computed-in-vue). You can also check out the [documentation](https://vuejs.org/v2/guide/computed.html) on this.

`methods` is different from `computed`. A `method` is just a function that is bound to the Vue instance. It will be evaluated only when we explicitly call it. Like all JavaScript functions, it accepts parameters and will be re-evaluated each time it is called. On the other hand, a `computed` method (or rather, a `computed` property), is quite a different beast. A computed value is a derived value that is automatically updated whenever one of the underlying values used to calculate it is updated.

* A `method` method can be called. It is useful wherever any normal JavaScript method is useful.
* A `computed` method cannot be called. Instead it is automatically "called" whenever there is a change in some data composing it. We reference a computed property just as we would a data property.

This example was taken from *Vue.js in Action*.

```javascript
var vm = new Vue({
  data: {
    length: 8,
    width: 10
  },
  computed: {
    area: function() {
      return this.length * this.width;
    }
  }
})
```

The computed property, `area`, will have an initial value of 15. Any subsequent change to `length` or `width` will then (reactively) trigger a series of updates to the application. This is how reactive programming looks like.

> **A  note on directives**
>
> A directive is prefixed with a `-v` to indicate that they are special attributes provided by Vue. We used a few directives, such as `v-on` in the previous example. They apply special reactive behaviour to the rendered DOM.
>
> There are many different directives, and Vue also provides us with the option of creating our own directives. But it is sufficient to learn a few fundamental ones only. The "Getting Started" [guide](https://vuejs.org/v2/guide/) for Vue covers them.

### `v-show`, `v-if`, `v-else`

These are simple logic. I will gloss over them.

### `v-model`

The `v-model` directive creates a two-way data binding between inputs and the template. This ensures that data is always in sync throughout the entire app. To understand the `v-model` directive, we first have to consider the `v-bind` directive, upon which the `v-model` is written.

#### The `v-bind` directive
The `v-bind` directive is used to dynamically bind one or more attributes, or a component prop, to an expression. The directive arises out of the need to assign values to elements, change their style, or assign classes.

For example, let's say we code the following:

```html
<div id="app">
  <a href="{{ url }}">Hyperlink</a>
  <a href="url">Hyperlink</a>
</div>

<script type="text/javascript">
  var vm = new Vue({
    el: "#app",
    data: {
      url: "http://widargoiwan.github.io"
    }
  });
</script>
```

We will face a problem because the moustache syntax fails to put the correct data into the `href` attribute. To fix this we have to do the following:

```html
<div id="app">
  <a v-bind:href="url">Hyperlink</a>
</div>
```

To assign values to HTML attributes, we need to bind it with the directive v-bind
