# vue-mastery

Compiled notes and exercises to reach mastery in Vue.
Using Nuxt starter template.


## Build Setup

``` bash
# install dependencies
$ npm install # Or yarn install

# serve with hot reload at localhost:3000
$ npm run dev

# build for production and launch server
$ npm run build
$ npm start

# generate static project
$ npm run generate
```

For detailed explanation on how things work, checkout the [Nuxt.js docs](https://github.com/nuxt/nuxt.js).



# Notes

## [Directives](https://vuejs.org/v2/guide/syntax.html#Directives)
| | | |
| ------:| ------:|  ------:|
| v-for  | v-for="(item, i) in items" |  iterating through a list |
| v-model | v-model="app" | two way data binding |
| v-bind   or :| :class="item.class"  | can bind class or style |
| v-on or @ | @click="myFunc" v-on="click:a, keyup: b, keydown: c, keyenter: d" | events |
| v-if  | v-if="bool === 'true'" | conditionally render ele, |
| v-else-if | v-else-if | conditionally render elem, can be chained |
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


> Useful way to debug
```html
<pre> {{ $data }} </pre>
```



## [Computed Properties](https://vuejs.org/v2/guide/computed.html#Computed-Properties)
For any complex logic, you should use a _computed property_.

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


example: hover state
### Aside
When we use reactive premises for building applications, this means it's very easy to update state in reaction to events.

Vue takes the data object, walks throught its properties and converts them to getters/setters. Vue cannot detect property addition or deletion so we create this object to keep track.

## [Templates](https://vuejs.org/v2/guide/syntax.html#ad)

Vue.js uses HTML-based template syntax to bind the Vue instance to the DOM, very useful for components.

Templates are optional, you can also write render functions with optional JSX support.

##[Components Basics](https://vuejs.org/v2/guide/components.html#ad)

Components are reusable Vue instances with a name. In other words, it is a collection of elements that are encapsulated into a group that can be accessed through one single element.

Since components are reusable Vue instances, they accept the same options as `new Vue`, such as `data`, `computed`, `watch`, `methods`, and `lifecycle hooks`. The only exceptions are a few root-specific options like `el`.

Can use props, templates.
A component’s `data` option must be a function, so that each instance can maintain an independent copy of the returned data object. If Vue didn’t have this rule, clicking on one button would affect the data of all other instances.

##[Props](https://vuejs.org/v2/guide/components-props.html#ad)

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


[Camel cased props](https://vuejs.org/v2/guide/components-props.html#Prop-Casing-camelCase-vs-kebab-case) will be converted to kebob case
```javascript
props: ['helloWorld']
```
```html
<div :hello-world="foo"></div>
```

All props form a [one-way-down binding](https://vuejs.org/v2/guide/components-props.html#One-Way-Data-Flow) between the child property and the parent one: when the parent property updates, it will flow down to the child, but not the other way around. This prevents child components from accidentally mutating the parent’s state, which can make your app’s data flow harder to understand.


