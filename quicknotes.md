- must register component
- must name it something different
    - only in nav? or for everything
- what is the @ component thing?

- why isnt eslint working in vue?

- fix worldRatio in CastleDuelGame


- did you register the component correctly? For recursive components, make sure to provide the "name" option.
    -name optional?



1 its fine to call the component the same name if its not a route
  its also fine to not name it and vue will name it for you based on what you import it as
  when you import a component, vue turns it nto a component for you


@ just means up until the src folder


JSX just works with vue cli

> slots in jsx
```javascript
<div>
  { this.$slots.default }
</div>
```


```javascript
<div class="overlay" @click="handleClick" >
  <div class="content">
    <slot />
  </div>
</div >

// becomes

render() {
  return (
    <div class="overlay" on-click={this.handleClick} >
      <div class="content">
        { this.$slots.default }
      </div>
    </div >
  )
}

```

for some reason it doesnt like when you export {Compoennt}