# Vue Notes

- Table of Contents
  - [Directives](#Directives)
  - [Computed Properties](#Computed-Properties)
  - [Watchers](#Watchers)
  - [Lifecycle Hooks](#Lifecycle-Hooks)
  - [Templates](#Templates)
  - [Components Basics](#Components-Basics)
  - [Props](#Props)
    - [Camel cased props](#Camel-cased-props)
  - [Custom Events](#Custom-Events)
  - [Slots](#Slots)
  - [Filters](#Filters)
  - [Mixins](#Mixins)
    - [Merging with mixins](#Merging-with-mixins)
    - [Global Mixins](#Global-Mixins)
  - [Custom Directives](#Custom-Directives)
  - [Hook Functions](#Hook-Functions)
  - [Vuex](#Vuex)
    - [One-Way Data Flow](#One-Way-Data-Flow)
    - [The Vuex Architecture](#The-Vuex-Architecture)
    - [mapActions](#mapActions)
  - [Transitions](#Transitions)
    - [named transition](#named-transition)
    - [unnamed transition](#unnamed-transition)
    - [Transition Classes](#Transition-Classes)
    - [Hide show animation](#Hide-show-animation)
    - [List Transitions](#List-Transitions)
  - [The Key Attribute](#The-Key-Attribute)
  - [Resources](#Resources)

- Jump To
  - [Vue Packages](vue-packages.md)
  - [README](../README.md)

# Notes

## [Directives](https://vuejs.org/v2/guide/syntax.html#Directives)
| | | |
| ------:| ------:|  ------:|
| v-for  | v-for="(item, i) in items" |  iterating through a list |
| v-model | v-model="app" | two way data binding |
| v-bind   or :| :class="item.class"  | can bind class or style |
| v-on or @ | @click="myFunc" v-on="click:a, keyup: b, keydown: c, keyenter: d" | events |
| v-if  | v-if="condition" | conditionally render ele, |
| v-else-if | v-else-if="condition" | conditionally render elem, can be chained |
| v-else  | v-else | otherwise render this elem |
| v-show | | conditionally toggle element |
|  v-once | v-once="foo" | renders something once |
| v-html | not to be used on user-generated content, XSS attacks | for strings that have html elements that need to be rendered|
| v-text | v-text="foo"| similar to mustach templates |
| v-cloak | v-cloak | can be used to hide un-compiled mustache bindings until the Vue instance is ready|
| v-pre | v-pre | used for displaying raw mustache tags |

<br>

| Modifiers |  | |
| ------:| ------:|  ------:|
| v-model.trim |  | strips whitespace from user inputs |
| v-model.number |  | changes strings to number inputs|
| v-model.lazy |  | won't auto update automatically |
| @mousemove.stop | | e.stopPropogation() |
| @mousemove.prevent | | e.preventDefault() |
| @submit.prevent | | will not reload page on submission |
| @click.once | | click can only be triggered once |
| @click.native | | listen to native events in the DOM |


**v-model**: Two way data binding between Vue instance and virtual DOM for forms and inputs
**v-bind**: One way data binding from Vue instance to virtual DOM to bind element attributes to a piece of state

> Useful way to debug
```html
  <pre> {{ $data }} </pre>
```

> v-  if, else-if and else
```html
  <div v-if="loading">
    Loading...
  </div>

  <div v-else-if="processing">
    Processing
  </div>

  <div v-else>
    Content here
  </div>
```

> V-Bind and Dynamic CSS Classes

```html
  <!-- template -->
  <div :class="['a', 'b', 'c']">
  <!-- html -->
  <div:class="one two three">

  <!-- template -->
  <div :class="{ a: true, b: false, c: true }">
  <!-- html -->
  <div :class="a c">

  <!-- which allows you conditionally add a class like so -->
  <div :class"{ loading: isLoading === true }">
  <div :class"{ selected: objRef1 === objRef2 }">

  <!-- you can have static classes and dynamic classes -->
  <div class="todo" :class="{ selected: todo === currentTodo }" >
```


## [Computed Properties](https://vuejs.org/v2/guide/computed.html#Computed-Properties)
For any complex logic, you should use a _computed property_. Computed is an object with methods that returns the thing you want computed. Accessible in templates by name.

```html
  <div id="example">
    <p>Original message: "{{ message }}"</p>
    <p>Computed reversed message: "{{ reversedMessage }}"</p>
  </div>
```

```javascript
  var vm = new Vue({
    el: '#example',
    data: {
      message: 'Hello'
    },
    computed: {
      // a computed getter
      reversedMessage: function () {
        // `this` points to the vm instance
        return this.message.split('').reverse().join('')
      }
    }
  })
```

If you instead wrote the revsedMessage function as a *method* instead of as *computed*, and invoked `{{ reversedMessage() }}`, it would achieve the exact same result. However, there is a difference between Computed Properties and Methods. _Computed Properties will be cached and will only change when its dependencies change_. A computed property will only re-evaluate when some of its dependencies have changed. This means as long as `message` has not changed, multiple access to the `reversedMessage` computed property will immediately return the previously computed result without having to run the function again. In comparison, a method invocation will _always_ run the function whenever a re-render happens or an update occurs. In cases where you do not need caching, then use a method instead.



## [Watchers](https://vuejs.org/v2/guide/computed.html#Watchers)
There are times when a custom watcher is necessary. This is most useful when you want to perform asynchronous or expensive operations in response to changing data.

Vue's approach to reactive programming. Reactive programming is programming with asynchronous data streams. A stream is a sequence of ongoing events ordered in time that offer some hooks iwth which to observe it.

Each component has a watcher instance. The properties touched by the watcher during the render are registered as dependencies. When the setter is triggered, it lets the watcher know, and causes the component to re-render.

The Vue instance is the middleman between the DOM and the business logic. Watch updates the DOM only if it is required. Performing calculations in JS is really performant but accessing the DOM is not. So we have a Virtual DOM which is like a copy, but parsed in JavaScript.

It will only update the DOM for things that need to be changed, which is faster.

Watchers are good for asyncronous updates, and updates/transitions with data changes.

Watchers must have the same name as the data property.


Watchers make the developer's life easier because you can watch a piece of state for changes and the watch handler will automatically run without having to tell Vue to do it explicitly each time.


example: hover state

[Deep Option](https://vuejs.org/v2/api/#Options-Data): If watching an object, you may need to pass the option `deep: true`, so that Vue will watch for changes within the object and run the handler. If you do not apply the `deep` option, and mutate a nested object, the watch handler will not run.

Vue will also watch for changes within the object recursively.

These operations are detected by default:
```javascript
  // Assignment
  this.message = 'hello world!'

  // Adding or removing an item in an array
  this.array.push({})
  this.array.splice(5)

  // Sorting an array
  this.array.sort()
```

All other operations will not envoke the watch handler:
```javascript
// Assignment to an attribute or a nested object
this.obj.a = 'hello world!'
this.obj.a.b = 'foo'

// Changes made to items in an array
this.array[0] = 'bar'
this.array[0].stars = 5
```

### Aside
When we use reactive premises for building applications, this means it's very easy to update state in reaction to events.

Vue takes the data object, walks throught its properties and converts them to getters/setters. Vue cannot detect property addition or deletion so we create this object to keep track.

## [Lifecycle Hooks](https://vuejs.org/v2/api/#Options-Lifecycle-Hooks)
### [beforeCreate](https://vuejs.org/v2/api/#beforeCreate)
### [created](https://vuejs.org/v2/api/#created)
### [beforeMount](https://vuejs.org/v2/api/#beforeMount)
### [mounted](https://vuejs.org/v2/api/#mounted)
### [beforeUpdate](https://vuejs.org/v2/api/#beforeUpdate)
### [updated](https://vuejs.org/v2/api/#updated)
### [activated](https://vuejs.org/v2/api/#activated)
### [deactivated](https://vuejs.org/v2/api/#deactivated)
### [beforeDestroy](https://vuejs.org/v2/api/#beforeDestroy)
### [destroyed](https://vuejs.org/v2/api/#destroyed)
### [errorCapture](https://vuejs.org/v2/api/#errorCaptured)


```javascript
  new Vue({
    data() {
      return {
        color: ''
      }
    }
    created() {
      this.loadContent()
    },
    methods: {
      loadContent() {
        this.color = 'blue'
      }
    }
  })
```

> Lifecycle Diagram

<img src="https://vuejs.org/images/lifecycle.png" style="max-width:450px;display:block;margin-left:auto;margin-right:auto;">


## [Templates](https://vuejs.org/v2/guide/syntax.html#ad)

Vue.js uses HTML-based template syntax to bind the Vue instance to the DOM, very useful for components.

Templates are optional, you can also write render functions with optional JSX support.

When using data or computed properties, you can access by the property name. If using with directives, should be surrounded in quotes, if it is an innerHTML then put inside mustaches.

## [Components Basics](https://vuejs.org/v2/guide/components.html#ad)

Components are the building blocks to compose an app, and a central concept in Vue.

Register a component with `Vue.component('Component', {})`

Components are reusable Vue instances with a name. In other words, it is a collection of elements that are encapsulated into a group that can be accessed through one single element.

Since components are reusable Vue instances, they accept the same options as `new Vue`, such as `data`, `computed`, `watch`, `methods`, and `lifecycle hooks`. The only exceptions are a few root-specific options like `el`.

Can use props, templates.
A component’s `data` option must be a function, so that each instance can maintain an independent copy of the returned data object. If Vue didn’t have this rule, clicking on one button would affect the data of all other instances.

## [Props](https://vuejs.org/v2/guide/components-props.html#ad)

Props are a way for the child components to get data from its parent.

From the parent template, bind the parent's data to the child components props by passing it like so:
`<child-component :my-prop="myProp" />`

Note the use of kebab case in v-bind. `:my-prop` is what the prop will be named inside the child component (you can name it whatever you want). The `"myProp"` in `:my-prop="myProp"` must match the name of the piece of state you wish to pass.

Then in the child component, set the `props` option.
`props: ['myProp']`

Now `this.myProp` is attached to the component instance and can be used within the component. In the template, you would access it with `{{myProp}}`

Types and Validation

```Javascript
  // lazy
  props: ['title', 'likes', 'isPublished', 'commentIds', 'author']

  // better
  props: {
    title: String,
    likes: Number,
    isPublished: Boolean,
    commentIds: Array,
    author: Object
  }

  // amazing
  props: {
    title: {
      type: String,
      required: true,
      default: "Ready Player One"
    },
    likes: {
      type: Number,
      required: true,
      default: 0
    },
    isPublished: {
      type: Boolean,
      required: true,
      default: true
    },
    commentIds: {
      type: Array,
      required: true,
      default: () => []
    },
    author: {
      type: Object,
      required: true,
      default: () => {
        name: '',
        id: ''
      }
    }
  }
```

This not only documents your component, but will also warn users in the browser’s JavaScript console if they pass the wrong type.

Note: Objects and arrays need their defaults to be returned from a function.

You don't necessarily need to pass the data in props to the child, either, you have the option of using vue instance data or a static value as you see fit.

We use `v-bind` or `:` to dynamically bin props to data on the parent.

Each component instance has its own isolated scope, which is why data must be a function. Without isolated scope, component state will update all instances of that component.

### [Camel cased props](https://vuejs.org/v2/guide/components-props.html#Prop-Casing-camelCase-vs-kebab-case) will be converted to kebob case
```javascript
  props: ['helloWorld']
```
```html
  <div :hello-world="foo"></div>
```

All props form a [one-way-down binding](https://vuejs.org/v2/guide/components-props.html#One-Way-Data-Flow) between the child property and the parent one: when the parent property updates, it will flow down to the child, but not the other way around. This prevents child components from accidentally mutating the parent’s state, which can make your app’s data flow harder to understand.

If you mutate a prop directly, it won't tell the parent that something changed. So when the parent re-renders, it will be overwritten to what it originally was.


## [Custom Events](https://vuejs.org/v2/guide/components-custom-events.html#Event-Names)

Vue has its own event system for custom components called "custom events", to use `@click`, you must use `@click.native`, otherwise the handler will not run.

You can also pass state from child to parent using "custom events". Very similar to the concept of lifting state up in React.

>Custom events are completely separate from the browser event system. The special methods--$on and $emit--are not aliases to the standard addEventListener and dispatchEvent. That explains why we need the .native modifier on components to listen to browser events such as 'click'.

```javascript
// child component
template: `<div @click="foo">Hello world!</div>`

methods: {
  foo() {
    this.emit('bar', 'baz')
  }
}

// parent
template: `<div @bar="barHandler"> </div>`

methods: {
  barHandler(x) {
    console.log('x:', x) // outputs 'x: baz'
  }
}
```

> Aside
Custom events can only be triggered up one level, so to trigger an event in a grandparent, it must first go through the parent

[vm.$on(event, callback)](https://vuejs.org/v2/api/#vm-on)
Listen for a custom event on the current vm.

You can watch for an event from within the same component with:
```javascript
  created() {
    this.$on('play', (...args) => {
      console.log('$on args', args)
    })
  }
```

## [Slots](https://vuejs.org/v2/guide/components-slots.html#ad)
Vue implements a content distribution API that’s modeled after the current Web Components spec draft, using the <slot> element to serve as distribution outlets for content.


## [Filters](https://vuejs.org/v2/guide/filters.html#ad)
The first thing to understand about filters is that they aren't replacements for methods, computed values, or watchers, because fiters don't tranform the data, just the output that the user sees.

Filters sounds like it would be good to filter a lot of data, but filters are rerun on every single update, so better to use computed, for values like these that should be cached.

## [Mixins](https://vuejs.org/v2/guide/mixins.html#ad)
Mixins are a flexible way to distribute reusable functionalities for Vue components. A mixin object can contain any component options. When a component uses a mixin, all options in the mixin will be “mixed” into the component’s own options.


It's a common situation: you have two components that are pretty similar, they share the same basic functionality, but there's enough that's different about each of them that you come to a crossroads: do I split this component into two different components? or do I keep one component, but create enough variance with props that I can alter each one?

A mixin allows you to encapsulate one piece of functionality so that you can use it in different components throughout the application.

If written correctly, they are pure, they don't modify or change things outside of the function's scope, so you will reliably always receive the same value with the same inputs on multiple executions.

### [Merging with mixins](https://vuejs.org/v2/guide/mixins.html#Option-Merging)
When a mixin and the component itself contain overlapping options, they will be “merged” using appropriate strategies.

For example, data objects undergo a shallow merge (one property deep), with the component’s data taking priority in cases of conflicts.

By default, mixins will be applied first, and the component will be applied second so that we can override it as necessary.

The component has the last say.

Mixins have all the lifecycle methods available to them, just like the components do.

### [Global Mixins](https://vuejs.org/v2/guide/mixins.html#Global-Mixin)
You can also apply a mixin globally. Use with caution! Once you apply a mixin globally, it will affect every Vue instance created afterwards. When used properly, this can be used to inject processing logic for custom options.

Global mixins are literally applied to every single component. One use I can think of that makes sense is something like a plugin, where you may need to gain access to everything.

But still, the use case for them is extremely limited and they should be used with great caution.

Almost never ever use these.

## [Custom Directives](https://vuejs.org/v2/guide/custom-directive.html#ad)
In addition to the default set of directives shipped in core (v-model and v-show), Vue also allows you to register your own custom directives. Note that in Vue 2.0, the primary form of code reuse and abstraction is components, however there may be cases where you need some low-level DOM access on plain elements, and this is where custom directives would still be useful. An example would be focusing on an input element.


`v-example` - this will instantiate a directive, but doesn't accept any arguments. Without passing a value, this would not be very flexible, but you could still hang some piece of functioanlity off of the DOM element.

`v-example="value" - this will pass a value into the directive, and the directive figures out what to do based off of that value.
```html
  <div v-if="stateExample">I will show up if stateExample is true</div>
```

`v-example="'string'"` - this will let you use a string as an expression
```html
  <p v-html="'<strong>this is an example of a string in some text</strong>'"></p>
```

`v-example:arg="value"` - this allows us to pass in an argument to the directive. In the example below, we're binding to a class, and we'd style it with an object, stored separately.

```html
  <div v-bind:class="someClassObject"></div>
```

`v-example:arg.modifier="value"` - this allows us to use a modifier. The example below allows us to call preventDefault() on the click event.

```html
  <button v-on:submit.prevent></button>
```

### [Hook Functions](https://vuejs.org/v2/guide/custom-directive.html#Hook-Functions)
A directive definition object can provide several hook functions (all optional):

* `unbind`: _Unmoumted from the DOM_. Called only once, when the directive is unbound from the element.

* `bind`: _Insertion into the DOM_. Called only once, when the directive is first bound to the element. This is where you can do one-time setup work.

* `inserted`: _Element is inserted_. Called when the bound element has been inserted into its parent node (this only guarantees parent node presence, not necessarily in-document).

* `update`: _Element is updated_. Called after the containing component’s VNode has updated, but possibly before its children have updated. The directive’s value may or may not have changed, but you can skip unnecessary updates by comparing the binding’s current and old values (see below on hook arguments).

* `componentUpdated`: _Children are updated_. Called after the containing component’s VNode and the VNodes of its children have updated.

## [Vuex](https://vuex.vuejs.org/)

* Centralized store for shared data and logic, even shared methods or async.

* Unidirectional data flow

* Ensures that the state can only be mutated in a predictable fashion

* Influenced by Flux, Redux and The Elm Architecture

* Similar to Redux

* You can still use Redux if you like but this is Vue's version

* Integrates with Vue's official devtools extension to provide advanced features such as zero-config time-travel debugging and state snapshot export / import.

> Why use Vuex?
In a complex SPA, passing state between many components, and especially deeply nested or sibling components, can get complicated quickly. Having one centralized place to access your data can help you stay organized.

Multiple views may depend on the same piece of state.

Actions from different views may need to mutate the same piece of state.

> When to use Vuex?
Multiple instances of children/siblings communicating and sharing logic accross the application.

Note: Not a replacement for single component methods. Not everything belongs in the Vuex Store.

#### ./store/index.js
```javascript
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)

  export const store = new Vuex.Store({
    state: {
      key: 'value'
    }
  })
```

#### main.js
```javascript
  import Vue from 'vue'
  import App from './App.vue'

  import { store } from './store'

  new Vue({
    el: '#app',
    store,
    template: '<App />',
    components: { App }
  })
```

## One-Way Data Flow
<img src="https://vuex.vuejs.org/flow.png" style="width:100%;max-width:450px;display:block;margin-left:auto;margin-right:auto;">

### The Vuex Architecture
<img src="https://vuex.vuejs.org/vuex.png" style="display:block;margin-left:auto;margin-right:auto;">


* [Getters](https://vuex.vuejs.org/api/#getters) will make values able to show statically in our templates. In other words, getters can read the value, but not mutate the state.

* [Mutations](https://vuex.vuejs.org/api/#mutations) will allow us to update the state, but they will always be synchronous. Mutations are the only way to change data in the state in the store.

* [Actions](https://vuex.vuejs.org/api/#actions) will allow us to update the state asynchronously, but will use an existing mutation. This can be very helpful if you need to perform a few different mutations at once in a particular order, or reach out to a server.

We separate actions and mutations because we don't want to get an ordering problem.

```javascript
  actions: {
    asyncIncrement ({ commit }) {
      setTimeout(() => {
        commit('increment')
      }, 1000)
    }
  }

  methods: {
    asyncIncrement() {
      this.$store.dispatch('asyncIncrement')
    }
  }
```

On the component itself, we would use `computed` for `getters` (this makes sense because the value is already computed for us), and `methods` with `commit` to access the `mutations` with `dispatch for the `actions.

In the Vue instance or a component:

```javascript
  computed: {
    value() {
      return this.$store.getters.tripleCounter;
    }
  },
  methods: {
    increment() {
      this.$store.commit('increment', 2)
    },
    asyncIncrement() {
      this.$store.dispatch('asyncIncrement', 2)
    }
  }
```

### [mapActions](https://vuex.vuejs.org/api/#mapactions)
You can use a spread operator, useful when you have to work with a lot of getters/mutations/actions.

```javascript
  import { mapActions } from 'vuex'

  export default {
    // ...
    methods: {
      ...mapActions([
        // map this.increment() to this.$store.commit('increment')
        'increment',
        'decrement',
        'asyncIncrement'
      ])
    }
  }
```
This allows us to still make our own computed properties if we wish.

## [Transitions](https://vuejs.org/v2/guide/transitions.html#ad)

Transition is a special component that will not appear in the DOM, much like the template tag.

Works with components and html elements.

It is easiest to just name the transition as seen below. You can ommit the name but the css syntax is more verbose. Named transitions are also reusable.

### named transition
```html
  <transition name="fade">
    <p v-if="show">hello</p>
  </transition>
```

```css
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
  }
  .fade-enter, .fade-leave-to {
    opacity: 0;
  }
```

### unnamed transition
```html
  <transition>
    <p v-if="show">hello</p>
  </transition>
```

```css
  p.v-enter-active, p.v-fade-leave-active {
    transition: opacity .5s;
  }
  p.v-fade-enter, p.v-fade-leave-to {
    opacity: 0;
  }
```

### [Transition Classes](https://vuejs.org/v2/guide/transitions.html#Transition-Classes)
There are two phases, entering phase and leaving phase.

There are six classes applied for enter/leave transitions.

1. v-enter: Starting state for enter. Added before element is inserted, removed one frame after element is inserted.

2. v-enter-active: Active state for enter. Applied during the entire entering phase. Added before element is inserted, removed when transition/animation finishes. This class can be used to define the duration, delay and easing curve for the entering transition.

3. v-enter-to: Only available in versions 2.1.8+. Ending state for enter. Added one frame after element is inserted (at the same time v-enter is removed), removed when transition/animation finishes.

4. v-leave: Starting state for leave. Added immediately when a leaving transition is triggered, removed after one frame.

5. v-leave-active: Active state for leave. Applied during the entire leaving phase. Added immediately when leave transition is triggered, removed when the transition/animation finishes. This class can be used to define the duration, delay and easing curve for the leaving transition.

6. v-leave-to: Only available in versions 2.1.8+. Ending state for leave. Added one frame after a leaving transition is triggered (at the same time v-leave is removed), removed when the transition/animation finishes.

<img src="https://vuejs.org/images/transition.png" style="width:100%;max-width:450px;display:block;margin-left:auto;margin-right:auto;">


### Hide show animation
```css
  .hand-enter-active,
  .hand-leave-active {
    transition: opacity .5s;
  }

  .hand-enter,
  .hand-leave-to {
    opacity: 0;
  }

  .hand-enter-active .wrapper,
  .hand-leave-active .wrapper {
    transition: transform .8s cubic-bezier(.08, .74, .34, 1);
    transform-origin: bottom center;
  }

  .hand-enter .wrapper,
  .hand-leave-to .wrapper {
    transform: rotateX(90deg);
  }

  .hand-enter-active .card,
  .hand-leave-active .card {
    transition: margin .8s cubic-bezier(.08, .74, .34, 1);
  }

  .hand-enter .card,
  .hand-leave-to .card {
    margin: 0 -100px;
  }
```

[How to create Vue.js Transitions Medium Article](https://medium.com/vue-mastery/how-to-create-vue-js-transitions-6487dffd0baa)




### [List Transitions](https://vuejs.org/v2/guide/transitions.html#List-Transitions)
For a list of items rendered simultaneously we can use the `<transition-group>` component.

```javascript
  <transition-group name="myitemsname" class="myclassname">
    <div v-for="item of items" />
  </transition-group>
```

Unlinke the transition element, the transition group apears in the DOM as a `<span>` by default. This can be changed with the tag prop.

```javascript
  <transition-group tag="ul" name="myitemsname" class="myclassname">
    <div v-for="item of items" />
  </transition-group>
```

List transitions have the six CSS classes listed above and has an additional class applied to moving items `v-move`. Vue will use the CSS transform property on the items to make them move.


## [The Key Attribute](https://vuejs.org/v2/guide/list.html#key)
When Vue is updating a list of elements rendered with v-for, by default it uses an “in-place patch” strategy. If the order of the data items has changed, instead of moving the DOM elements to match the order of the items, Vue will patch each element in-place and make sure it reflects what should be rendered at that particular index.

To give Vue a hint so that it can track each node’s identity, and thus reuse and reorder existing elements, you need to provide a unique key attribute for each item.

It is recommended to provide a key with v-for whenever possible, unless the iterated DOM content is simple, or you are intentionally relying on the default behavior for performance gains.

Since it’s a generic mechanism for Vue to identify nodes, the key also has other uses that are not specifically tied to v-for

## Resources
[vue docs](https://vuejs.org/v2/api/)
[vue repo](https://github.com/vuejs)
[nuxt docs](https://nuxtjs.org/)
[vuex docs](https://vuex.vuejs.org/)
[vuex repo](https://github.com/vuejs/vuex)
[css-tricks guide](https://css-tricks.com/guides/vue/)
[awesome vue](https://github.com/vuejs/awesome-vue)
[vue newsletter](https://news.vuejs.org/)
[monterail blog](https://www.monterail.com/blog)
[vue tips](http://vuetips.com/)
[the majesty of vue](https://leanpub.com/vuejs2)
[Sarah Dras intro-to-vue](https://github.com/sdras/intro-to-vue)