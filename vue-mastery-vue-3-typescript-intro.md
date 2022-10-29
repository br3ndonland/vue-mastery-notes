# Intro to Vue 3 + TypeScript

[Vue Mastery](https://www.vuemastery.com)

[Intro to Vue 3 + TypeScript](https://www.vuemastery.com/courses/vue3-typescript)

[Code on GitHub](https://github.com/Code-Pop/Real-World-Vue-3-TypeScript)

Instructor: Ben Hong

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
  - [Computed properties](#computed-properties)
  - [Methods](#methods)
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

- Script blocks need `lang="ts"`, like `<script lang="ts"></script>`.
- The [`defineComponent` helper method](https://vuejs.org/guide/typescript/overview.html#definecomponent) needs to be imported: `import { defineComponent } from "vue"`
- `export` objects need to be constructed with the `defineComponent` helper method, rather than just `export default {}`: `export default defineComponent({ ... })`
- See the [Real-World-Vue-3-TypeScript](https://github.com/Code-Pop/Real-World-Vue-3-TypeScript) repo for a practical example.
- Also see the [example in the Vue docs](https://vuejs.org/guide/typescript/overview.html#definecomponent):

  ```ts
  import { defineComponent } from "vue"

  export default defineComponent({
    // type inference enabled
    props: {
      name: String,
      msg: { type: String, required: true },
    },
    data() {
      return {
        count: 1,
      }
    },
    mounted() {
      this.name // type: string | undefined
      this.msg // type: string
      this.count // type: number
    },
  })
  ```

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
- Simple types are added to variables like `isComplete: boolean = false`. Note that type names are lowercase in this syntax. Also note that, for this type, the default value is `false`.
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

An `interface` is like a type for an object. Each object attribute can have a type. To make a field optional (allowing either the given type or `undefined`), add `?`, like `age?: number`. To set a default value, use `=` and then the value, like `activeAvenger: boolean = true`.

```ts
type ComicUniverse = "Marvel" | "DC"

interface Hero {
  name: string
  age?: number
  activeAvenger: boolean = true
  powers: string[]
  universe: ComicUniverse
}
```

See the [TypeScript docs "TypeScript in 5 minutes" tutorial](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html) and the [TypeScript docs on optional parameters](https://www.typescriptlang.org/docs/handbook/2/functions.html#optional-parameters) for more info.

## 6. Data with Custom Types

- There is a [VueDX](https://github.com/vuedx/languagetools) VSCode extension that can help with types.
- Types can be centralized in a single file, called a "[declaration file](https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html)," like `src/types.d.ts`.

## 7. Props with Types

Vue allows [prop types](https://vuejs.org/guide/components/props.html#prop-validation) to be specified and validated when using object syntax:

```js
export default {
  props: {
    event: {
      type: Object,
      required: true,
    },
  },
}
```

The type `Object` is not very specific. TypeScript allows us to define more specific and custom types. Vue supports [typing props with TypeScript](https://vuejs.org/guide/typescript/options-api.html#typing-component-props).

It's not quite as simple as just importing a type and replacing `Object` with the type. TypeScript types are specific to TypeScript. To use TypeScript types to specify Vue.js prop types, the `PropType` utility can be used.

```ts
import { defineComponent, PropType } from "vue"

interface EventItem {
  id: number
  category: string
  title: string
  description: string
  location: string
  date: string
  time: string
  organizer: string
}

export default defineComponent({
  props: {
    event: {
      type: Object as PropType<EventItem>,
      required: true,
    },
  },
})
```

The type within carets, like `<EventItem>`, is an example of [TypeScript generics](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html#generics). Generics are like variables for complex types.

A TypeScript function with generic types might look like this:

```ts
function createArray<T>(item: T): T[] {
  const newArray: T[] = []
  newArray.push(item)
  return newArray
}

const numberArray = createArray<number>(123)
```

The generic type basically tells TypeScript that the function will accept a type, and then make an array of values with the type that was passed in. `createArray<number>(123)` will create an array of numbers: `[1, 2, 3]`.

While not particularly readable, the use of `T` to indicate a generic type is a common convention in other languages. For example, [Python uses this convention for type checking with mypy](https://mypy.readthedocs.io/en/latest/generics.html).

## 8. Computed & Methods with Custom Types

### Computed properties

Computed properties are typed based on what is returned.

```ts
import { defineComponent } from "vue"

interface EventItem {
  id: number
  category: string
  title: string
  description: string
  location: string
  date: string
  time: string
  organizer: string
}

export default defineComponent({
  data() {
    return {
      events: [] as EventItem,
    }
  },
  computed: {
    secondEvent(): EventItem {
      return this.events[1]
    },
  },
})
```

### Methods

Methods are typed based on function arguments (inputs) and return types.

```ts
import { defineComponent } from "vue"

interface EventItem {
  id: number
  category: string
  title: string
  description: string
  location: string
  date: string
  time: string
  organizer: string
}

export default defineComponent({
  data() {
    return {
      events: [] as EventItem,
    }
  },
  computed: {
    secondEvent(): EventItem {
      return this.events[1]
    },
    methods: {
      addEvent(newEvent: EventItem) {
        this.events.push(newEvent)
      },
      secondEvent(): EventItem {
        return this.events[1]
      },
    },
  },
})
```

## 9. Next Steps

## 10. Bonus: Composition API with TypeScript

In this lesson, the instructor Ben Hong updates the demo app from the course to use [TypeScript with the Vue 3 Composition API](https://vuejs.org/guide/typescript/composition-api.html), instead of the original [Options API](https://vuejs.org/guide/typescript/options-api.html).

- The `data()` object moves into a `reactive()` object within the `setup()` object
- `toRefs(state)` is added in order to access state attributes
- `computed` moves into `setup()`
- `methods` move into `setup()`
- Moving these methods inside `setup()` and adding return types can improve type inference. The [Vue 3 docs](https://vuejs.org/guide/typescript/options-api.html) explain, "While Vue does support TypeScript usage with Options API, it is recommended to use Vue with TypeScript via Composition API as it offers simpler, more efficient and more robust type inference."

See the [code on GitHub](https://github.com/Code-Pop/Real-World-Vue-3-TypeScript/tree/10-end). For example, the `<script lang="ts">` section of `src/views/Todo.vue` looks like this:

```ts
import { computed, defineComponent, reactive, toRefs } from "vue"
import { TodoItem } from "../types"

export default defineComponent({
  setup() {
    const state = reactive({
      newTask: {
        label: "",
        type: "personal",
      } as TodoItem,
      taskItems: [] as TodoItem[],
      listFilter: "all",
    })
    const filteredTasks = computed(() => {
      if (state.listFilter === "complete") {
        return state.taskItems.filter(
          (item: TodoItem) => item.isComplete === true
        )
      } else if (state.listFilter === "incomplete") {
        return state.taskItems.filter(
          (item: TodoItem) => item.isComplete === false
        )
      } else {
        return state.taskItems
      }
    })
    const addTask = () => {
      state.taskItems.push({
        ...state.newTask,
        isComplete: false,
      })
    }
    return {
      ...toRefs(state),
      addTask,
      filteredTasks,
    }
  },
})
```

**COURSE COMPLETE!!! I RULE!!!**

<img src="img/vue-mastery-vue3-typescript-intro-complete.png" alt="Vue Mastery Vue 3 and TypeScript course completion page" width="600px" />
