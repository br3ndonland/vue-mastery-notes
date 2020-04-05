# Testing

[Vue Mastery](https://www.vuemastery.com)

[Unit testing course](https://www.vuemastery.com/courses/unit-testing/what-to-test)

## Table of Contents <!-- omit in toc -->

- [1. What to test](#1-what-to-test)
  - [Benefits of unit testing](#benefits-of-unit-testing)
  - [The component contract](#the-component-contract)
  - [What to test and not test](#what-to-test-and-not-test)
- [2. Writing a Unit Test with Jest](#2-writing-a-unit-test-with-jest)
- [3. Testing Props & User Interaction](#3-testing-props--user-interaction)
- [4. Testing Emitted Events](#4-testing-emitted-events)
- [5. Testing API Calls](#5-testing-api-calls)
- [6. Stubbing Child Components](#6-stubbing-child-components)
- [7. Testing Vuex](#7-testing-vuex)
- [8. Testing Vue Router](#8-testing-vue-router)

## 1. What to test

### Benefits of unit testing

- **Increased confidence:** effects of changes to code are easier to evaluate.
- **Quality code:** when components are written to be testable, they are more isolated and reusable.
- **Better documentation:** tests help describe how the source code works.

### The component contract

- The units of a Vue.js app are components.
- Each component has a "contract" with the rest of the app.

### What to test and not test

<img src="img/vm-testing-01-01.png" alt="What to test" width="600" />

**Test:**

- **Inputs:** data passed in to components
  - Props
  - User interaction
  - [Lifecycle hooks](https://vuejs.org/v2/api/#Options-Lifecycle-Hooks)
  - Vuex data
  - Route params
- **Outputs:**
  - DOM rendering of component
  - External function calls
  - Emitted events
  - Route changes
  - Vuex updates
  - Changes passed to child components

**Don't test:**

- Implementation details
- Framework itself: **if the component has prop types and defaults specified, those will be checked by the Vue core library itself, and don't need further unit tests.**
- Third-party libraries (should have their own tests already)

## 2. Writing a Unit Test with Jest

## 3. Testing Props & User Interaction

## 4. Testing Emitted Events

## 5. Testing API Calls

_To be released on April 7_

## 6. Stubbing Child Components

_To be released on May 5_

## 7. Testing Vuex

_To be released on May 12_

## 8. Testing Vue Router

_To be released on May 19_
