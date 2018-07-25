# Vue CLI

- Table of contents
  - [Inline SVG](#Inline-SVG)
  - [Deploying to heroku](#Deploying-to-heroku)



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





#### [Deploying to heroku](https://medium.com/netscape/deploying-a-vue-js-2-x-app-to-heroku-in-5-steps-tutorial-a69845ace489)
1. `$ heroku create <YOUR-PROJECT-NAME-HERE>`
2. `$ heroku config:set NODE_ENV=production --app <YOUR-PROJECT-NAME-HERE>`
3. Create an express server
  - add script to package.json: `"start": "node server",`
  - test server: `$ npm run build && npm start`
4. `$ heroku git:remote --app <YOUR-PROJECT-NAME-HERE>`
5. `$ git push heroku master`
