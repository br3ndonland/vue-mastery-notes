# Vue 3 Essentials

[Vue Mastery](https://www.vuemastery.com)

[Vue 3 Essentials](https://www.vuemastery.com/courses/vue-3-essentials)

## Table of Contents <!-- omit in toc -->

- [1. Why the Composition API](#1-why-the-composition-api)
  - [Readability suffers as components grow](#readability-suffers-as-components-grow)
  - [Code reuse patterns have drawbacks](#code-reuse-patterns-have-drawbacks)
  - [The composition API allows components to be organized by logical concerns](#the-composition-api-allows-components-to-be-organized-by-logical-concerns)
- [2. Setup & Reactive References](#2-setup--reactive-references)
  - [Setup](#setup)
  - [Reactive references](#reactive-references)
- [3. Methods](#3-methods)
- [4. Computed Properties](#4-computed-properties)
- [5. The Reactive Syntax](#5-the-reactive-syntax)
- [6. Modularizing](#6-modularizing)
- [7. Lifecycle Hooks](#7-lifecycle-hooks)
- [8. Watch](#8-watch)
- [9. Sharing State](#9-sharing-state)

## 1. Why the Composition API

### Readability suffers as components grow

Say you have searching and sorting features in a component. The search and sort features will be split up across sections of the component (data, computed, methods, etc). As the component gets larger, it's difficult to keep track of all the features for searching or sorting.

### Code reuse patterns have drawbacks

Previous options for code reuse:

- Mixins
- Mixin factories
- Scoped slots

### The composition API allows components to be organized by logical concerns

Code for a particular feature, like searching or sorting in the example above, can be collected together inside a new **`setup()` method**:

  <img src="img/vm-vue3-01-06-composition-functions.jpg" alt="Vue 3 composition functions" width="600px" />

You don't have to include all the code directly inside a massive `setup()` method. The `setup()` method can accept **composition functions** from other files.

  <img src="img/vm-vue3-01-11-composition-functions.jpg" alt="Vue 3 composition functions" width="600px" />

## 2. Setup & Reactive References

### Setup

- `setup()` is additive, it doesn't replace previous methods.
- Executes before:
  - Components
  - Props
  - Data
  - Methods
  - Computed Properties
  - Lifecycle methods
- We can use a `props` argument to provide props to `setup()`.
- Doesn't have access to `this`. We use the `context` argument instead.
- There's a [Vue 2 plugin for the composition API](https://github.com/vuejs/composition-api) (not recommended for production).

### Reactive references

- Reactive data are wrapped in `ref()`, which is sort of a stand-in for `data()` in the `setup()` function.
- We also need to `return` the reactive object.

## 3. Methods

- Methods can be declared as functions inside of `setup()`.
- We don't have access to `this`, so instead we access values inside of reactive references by using the object name.
- When Vue finds a `ref()` in the `template`, it automatically exposes the inner value, so there's no need to modify `{{ capacity }}`.
- The Vue component below is a simple counter, which could show the total capacity at an event.

  ```html
  <template>
    <div>
      <p>Capacity: {{ capacity }}</p>
      <button @click="increaseCapacity()">Increase Capacity</button>
    </div>
  </template>
  <script>
    import { ref } from "vue"
    export default {
      setup() {
        const capacity = ref(3)
        function increaseCapacity() {
          capacity.value++
        }
        return { capacity, increaseCapacity }
      }
    }
  </script>
  ```

## 4. Computed Properties

## 5. The Reactive Syntax

## 6. Modularizing

## 7. Lifecycle Hooks

## 8. Watch

## 9. Sharing State
