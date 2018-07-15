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

| Directives |  | |
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



## Computed Properties
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
