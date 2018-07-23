# Vue CLI

- Table of contents
  - [Inline SVG](#Inline-SVG)


## [Inline SVG](https://cli.vuejs.org/guide/webpack.html#adding-a-new-loader)

To use inline SVG install vue-svg-loader, create a vue.config.js and add the override rule. Then you can use the svg as you would a component.

```bash
npm install --save-dev vue-svg-loader
yarn add --dev vue-svg-loader
```

```javascript
<template>
  <nav id="menu">
    <a href="...">
      <SomeIcon class="icon" />
      Some page
    </a>
  </nav>
</template>

<script>
import SomeIcon from './assets/some-icon.svg';

export default {
  name: 'menu',
  components: {
    SomeIcon,
  },
};
</script>

```

```javascript
// vue.config.js
module.exports = {
  chainWebpack: config => {
    const svgRule = config.module.rule('svg')

    // clear all existing loaders.
    // if you don't do this, the loader below will be appended to
    // existing loaders of the rule.
    svgRule.uses.clear()

    // add replacement loader(s)
    svgRule
      .use('vue-svg-loader')
        .loader('vue-svg-loader')
  }
}
```