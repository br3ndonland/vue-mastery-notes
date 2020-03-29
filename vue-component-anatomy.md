# Vue.js guide

## Learning resources

- [Vue Mastery](https://www.vuemastery.com)
- [Vue.js docs](https://vuejs.org/v2/guide/)
- [Vue.js style guide](https://vuejs.org/v2/style-guide)

## Component anatomy

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

```html
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

## Best practices

Adhere to best practices described in the [Vue.js Style Guide](https://vuejs.org/v2/style-guide).

### Component names

- [Component names should always be multi-word](https://vuejs.org/v2/style-guide/#Multi-word-component-names-essential).
- [Order of words in component names](https://vuejs.org/v2/style-guide/#Order-of-words-in-component-names-strongly-recommended) should go from general to specific.
- [Single-file component filename casing](https://vuejs.org/v2/style-guide/#Single-file-component-filename-casing-strongly-recommended) should always be either kebab-case or PascalCase.

### Name case

- Use `camelCase` for all functions, methods, and actions.
- Use `UPPERCASE_UNDERSCORES` for Vuex mutations only, not for Vuex actions.

### Props

- Always use object notation for [prop definitions](https://vuejs.org/v2/style-guide/#Prop-definitions-essential), specifying at least the prop type.

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
