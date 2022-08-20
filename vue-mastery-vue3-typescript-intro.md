# Intro to Vue 3 + TypeScript

[Vue Mastery](https://www.vuemastery.com)

[Intro to Vue 3 + TypeScript](https://www.vuemastery.com/courses/vue3-typescript)

## Table of Contents <!-- omit in toc -->

- [1. Why Vue \& TypeScript](#1-why-vue--typescript)
- [2. Setting Up Vue 3 \& TypeScript](#2-setting-up-vue-3--typescript)
- [3. Creating Components with TypeScript](#3-creating-components-with-typescript)
  - [Overview](#overview)
  - [Path aliases with Vue.js and TypeScript](#path-aliases-with-vuejs-and-typescript)
- [4. Type Fundamentals](#4-type-fundamentals)
- [5. Defining Custom Types](#5-defining-custom-types)
  - [type](#type)
  - [interface](#interface)
- [6. Data with Custom Types](#6-data-with-custom-types)
- [7. Props with Types](#7-props-with-types)
- [8. Computed \& Methods with Custom Types](#8-computed--methods-with-custom-types)
- [9. Next Steps](#9-next-steps)
- [10. Bonus: Composition API with TypeScript](#10-bonus-composition-api-with-typescript)

## 1. Why Vue & TypeScript

- TypeScript prevents type errors and improves IDE support. From the [Vue.js docs](https://vuejs.org/guide/typescript/overview.html): "A type system like TypeScript can detect many common errors via static analysis at build time. This reduces the chance of runtime errors in production, and also allows us to more confidently refactor code in large-scale applications. TypeScript also improves developer ergonomics via type-based auto-completion in IDEs."
- TypeScript has a cost.
  - Learning curve
  - Compilation time (@br3ndonland note: see [denoland/deno#5432](https://github.com/denoland/deno/issues/5432))
- TypeScript is not actually as scary as you might think.
  - [Vue.js props](https://vuejs.org/guide/components/props.html) with object syntax are similar to TypeScript
  - Vue.js prop validators also have a similar effect
- TypeScript can be added incrementally. Specific components and parts of the application can be TypeScript

## 2. Setting Up Vue 3 & TypeScript

- New project: `vue create`, select TypeScript in the list of features
- Existing project: `vue add typescript`
- TypeScript files will have a `.ts` extension. They look similar to JavaScript files.
- `shims-vue.d.ts` is a helper file for TypeScript to understand Vue.js types.
- `tsconfig.json` can be used to configure the TypeScript compiler.

## 3. Creating Components with TypeScript

### Overview

- Script blocks will now look like `<script lang="ts"></script>`
- A helper function import is needed: `import { defineComponent } from "vue"`
- `defineComponent` will then be used to construct `export` objects: `export default defineComponent({ ... })`
- See the [Real-World-Vue-3-TypeScript](https://github.com/Code-Pop/Real-World-Vue-3-TypeScript) repo for a practical example.

### Path aliases with Vue.js and TypeScript

Note that TypeScript must be configured to handle aliases.

- The `@` symbol is an alias to `/src` (see [vue tips](vue-tips.md)).
- As explained in the [Vue.js docs](https://vuejs.org/guide/typescript/overview.html#configuring-tsconfig-json), the alias must be configured in the [`compilerOptions.paths` section of `tsconfig.json`](https://www.typescriptlang.org/tsconfig#paths).
- It might look like this:
  ```json
  {
    "compilerOptions": {
      "baseUrl": ".",
      "paths": {
        "@/*": ["src/*"]
      }
    }
  }
  ```
- There might be some additional [Vite configuration](https://vitejs.dev/config/shared-options.html#resolve-alias) needed in `vite.config.js` if using Vite:
  ```js
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  }
  ```

## 4. Type Fundamentals

- JavaScript has many [built-in types (global objects)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects)
- TypeScript adds types:
  - Tuple (like an array, but with predefined length)
  - Enum (set "friendly names" for values)
- Simple types are added to variables like `isComplete: boolean = false`. Note that type names are lowercase in this syntax.
- Complex types like arrays need to define not only the data type, but which data types it should contain. An array of strings might be defined like `let shoppingList: string[] = ["eggs", "kale", "sweet potatoes"]`.
- Functions have types in a couple of places:
  - Function parameters (arguments)
  - Function return types
  ```ts
  let generateFullName = (firstName: string, lastName: string): string => {
    return `${firstName} ${lastName}`
  }
  ```

## 5. Defining Custom Types

### type

`type` can be used to define types. The union operator (`|`) is like "or" in JavaScript.

```ts
type ButtonType = "primary" | "secondary" | "success" | "danger"
```

### interface

An `interface` is like a type for an object.

```ts
type ComicUniverse = "Marvel" | "DC"

interface Hero {
  name: string
  age: number
  activeAvenger: boolean
  powers: string[]
  universe: ComicUniverse
}
```

See the [TypeScript docs](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html) for more info.

## 6. Data with Custom Types

- There is a [VueDX](https://github.com/vuedx/languagetools) VSCode extension that can help with types.
- Types can be centralized in a single file, like `src/types.ts`.

## 7. Props with Types

## 8. Computed & Methods with Custom Types

## 9. Next Steps

## 10. Bonus: Composition API with TypeScript