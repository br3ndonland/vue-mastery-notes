# Vue.js tips

## Table of Contents <!-- omit in toc -->

- [Learning resources](#learning-resources)
- [Component anatomy](#component-anatomy)
  - [Template](#template)
  - [Script](#script)
  - [Example](#example)
- [Features](#features)
  - [Automatic global registration of base components](#automatic-global-registration-of-base-components)
  - [@ shortcut](#-shortcut)
  - [Two-way computed properties](#two-way-computed-properties)
- [Style guide](#style-guide)
  - [Priority A rules: essential](#priority-a-rules-essential)
  - [Priority B rules: strongly recommended](#priority-b-rules-strongly-recommended)
  - [DOM templates vs. render functions](#dom-templates-vs-render-functions)
  - [Vuex](#vuex)
- [Linting and autoformatting](#linting-and-autoformatting)
- [Tooling](#tooling)

## Learning resources

- [Vue Mastery](https://www.vuemastery.com)
- [Vue.js docs](https://vuejs.org/v2/guide/)
- [Vue.js style guide](https://vuejs.org/v2/style-guide)

## Component anatomy

**The three sections of a Vue component represent the three code languages that web browsers run**:

1. HTML (structure, this is the `<template>` section of a Vue component)
2. JavaScript (function, this is the `<script>` section of a Vue component)
3. CSS (style, this is the `<style>` section of a Vue component)

Also see [Vue.js docs - components basics](https://vuejs.org/v2/guide/components.html).

### Template

HTML goes here.

### Script

JavaScript goes here.

#### Imports

#### Exports

The following order of exports is suggested based on [`eslint-plugin-vue`](https://eslint.vuejs.org/rules/):

- `name`
- `components`
- `mixins`
- `props`
- `data()`
- `computed`
- `watch`
- `created()`
- `methods`

#### Style

CSS and SCSS go here.

### Example

```vue
<template>
  <div>
    <!-- <other-component></other-component> -->
  </div>
</template>

<script>
// import OtherComponent from "other-component.vue"
// import { Mixin } from "mixins/Mixin.js"
export default {
  name: "component-name",
  components: {
    // "other-component": OtherComponent
  },
  mixins: [],
  props: {
    prop1: Number,
    prop2: {
      type: String,
      required: true,
      default: `Hello, World!`,
    },
  },
  data() {
    return {
      key1: value1,
      key2: value2,
    }
  },
  computed: {},
  watch: {},
  created() {},
  methods: {},
}
</script>

<style scoped></style>
```

## Features

### Automatic global registration of base components

[Component Registration - automatic global registration of base components](https://vuejs.org/v2/guide/components-registration.html#Automatic-Global-Registration-of-Base-Components):

- Place components in _/components_
- Start the name with `Base`, like `BaseButton`
- You can use the component everywhere without having to import it.

### @ shortcut

`@` is an alias to `/src`. Helpful so you don't have to constantly type `../../` (for example, `../../components` could become `@/components`). Also, if you rearrange your components, the relative links won't be broken.

### Two-way computed properties

Instead of separate computed properties and watchers, [two-way computed properties](https://vuex.vuejs.org/guide/forms.html#two-way-computed-property) combine getters and setters, enabling use of v-model while pulling data from Vuex instead of storing locally.

```vue
<template>
  <div>
    <input v-model="message" />
  </div>
</template>

<script>
export default {
  name: "component-name",
  components: {},
  mixins: [],
  props: {},
  },
  data() {
    return {}
  },
  computed: {
    message: {
      get() {
        return this.$store.state.obj.message
      },
      set(value) {
        this.$store.commit("updateMessage", value)
      },
    },
  },
  watch: {},
  created() {},
  methods: {},
}
</script>

<style scoped></style>
```

## Style guide

Adhere to best practices described in the [Vue.js Style Guide](https://vuejs.org/v2/style-guide).

### Priority A rules: essential

- Always include [prop definitions](https://vuejs.org/v2/style-guide/#Prop-definitions-essential), specifying at least the prop type.

### Priority B rules: strongly recommended

- [Single-file component name casing](https://vuejs.org/v2/style-guide/#Single-file-component-filename-casing-strongly-recommended) should be consistently either PascalCase or kebab-case.
- [Order of words in component names](https://vuejs.org/v2/style-guide/#Order-of-words-in-component-names-strongly-recommended) should go from general to specific, in PascalCase. For example, if there is a parent component _DataTable.vue_, a child component might be named _DataTableItem.vue_.

### DOM templates vs. render functions

DOM templates mount the Vue instance to a DOM element [`el`](https://vuejs.org/v2/api/#el).

```js
new Vue({
  el: "#app",
  router,
  store,
  components: { App },
  template: "<App/>",
})
```

Server-side rendered (SSR) apps like Nuxt.js can't use DOM templates, because they don't have access to the DOM at the time of rendering. DOM templates are also slower, because they don't take full advantage of Vue's virtual DOM (vDOM). When relying on the vDOM, just the JavaScript is changed, and then Vue will update the DOM.

The alternative is to use a render function (generated by default for Vue CLI and Nuxt.js projects):

```js
new Vue({
  router,
  store,
  render: (h) => h(App),
}).$mount("#app")
```

For more info, see:

- [Vue.js Developers | Blog 20170917: Why you should avoid Vue.js DOM templates](https://vuejsdevelopers.com/2017/09/17/vue-js-avoid-dom-templates/)
- [Vue.js Developers | Blog 20171205: What’s the deal with Vue’s virtual DOM?](https://vuejsdevelopers.com/2017/02/21/vue-js-virtual-dom/)

### Vuex

- The convention is to write mutations in [flux](https://github.com/facebook/flux)-style (capital letters with underscores, like `ACTION_NAME`). Vuex actions should be written in typical `camelCase`.
- Some teams [use constants for mutation types](https://vuex.vuejs.org/guide/mutations.html#using-constants-for-mutation-types). I prefer not to. Having a separate mutation types file means each new mutation added to a Vuex module has to also be added to the mutation types file. Team members frequently forget to do this.

## Linting and autoformatting

For JavaScript linting and autoformatting, use [ESLint](https://eslint.org/), [TSLint](https://palantir.github.io/tslint/), or [Prettier](https://prettier.io/).

**Prettier is preferred** because:

- It requires less configuration
- It can install globally without a _package.json_ (can be installed system-wide, as opposed to in a specific repo)
- It can format more code languages, such as CSS, Markdown, and YAML.

Prettier extensions are available for [VSCode](https://github.com/prettier/prettier-vscode), [Vim](https://github.com/prettier/vim-prettier), [Emacs](https://github.com/prettier/prettier-emacs), [Atom](https://github.com/prettier/prettier-atom), and [Sublime Text](https://packagecontrol.io/packages/JsPrettier).

_.eslintrc_

```json
{
  "env": {
    "browser": true,
    "es2020": true
  },
  "extends": [
    "eslint:recommended",
    "plugin:vue/recommended",
    "plugin:prettier/recommended"
  ],
  "plugins": ["vue"],
  "rules": {
    "indent": ["warn", 2],
    "linebreak-style": ["error", "unix"],
    "quotes": ["warn", "double", { "allowTemplateLiterals": true }],
    "semi": ["warn", "never"],
    "vue/html-closing-bracket-newline": 0,
    "vue/html-indent": 0,
    "vue/html-self-closing": 0,
    "vue/max-attributes-per-line": 0,
    "vue/multiline-html-element-content-newline": 0,
    "vue/singleline-html-element-content-newline": 0
  }
}
```

_.prettierrc_

```json
{
  "semi": false
}
```

_tslint.json_

```json
{
  "extends": ["tslint-plugin-prettier", "tslint-config-prettier"],
  "rules": {
    "prettier": true
  },
  "defaultSeverity": "warning",
  "linterOptions": {
    "exclude": ["node_modules/**"]
  }
}
```

## Tooling

Vue.js-specific tooling is available for various text editors.

- [Microsoft Visual Studio Code](https://code.visualstudio.com/) (VSCode) has the [Vetur](https://vuejs.github.io/vetur/) extension. Suggested VSCode settings:

  ```json
  {
    "editor.formatOnSave": true,
    "editor.insertSpaces": true,
    "editor.tabSize": 2,
    "eslint.autoFixOnSave": false,
    "eslint.enable": true,
    "eslint.validate": ["javascript", "javascriptreact", "vue"],
    "files.trimTrailingWhitespace": true,
    "[html]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascript]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[json]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[jsonc]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[markdown]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "vetur.format.defaultFormatter.css": "prettier",
    "vetur.format.defaultFormatter.html": "prettier",
    "vetur.format.defaultFormatter.js": "prettier",
    "vetur.format.defaultFormatter.ts": "prettier",
    "vetur.format.options.tabSize": 2,
    "vetur.format.options.useTabs": false,
    "[vue]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[yaml]": {
      "editor.defaultFormatter": "esbenp.prettier-vscode"
    }
  }
  ```

- Vim has [vim-vue](https://github.com/posva/vim-vue) and [vim-vue-plugin](https://github.com/leafOfTree/vim-vue-plugin).
- Emacs has the [vue-mode](https://github.com/AdamNiederer/vue-mode) extension.
