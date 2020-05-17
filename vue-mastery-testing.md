# Testing

[Vue Mastery](https://www.vuemastery.com)

[Unit testing course](https://www.vuemastery.com/courses/unit-testing/what-to-test)

## Table of Contents <!-- omit in toc -->

- [1. What to test](#1-what-to-test)
  - [Benefits of unit testing](#benefits-of-unit-testing)
  - [The component contract](#the-component-contract)
  - [What to test and not test](#what-to-test-and-not-test)
- [2. Writing a Unit Test with Jest](#2-writing-a-unit-test-with-jest)
  - [Vue CLI project creation](#vue-cli-project-creation)
  - [Unit testing steps](#unit-testing-steps)
- [3. Testing Props & User Interaction](#3-testing-props--user-interaction)
  - [Random number component](#random-number-component)
  - [Random number component tests](#random-number-component-tests)
- [4. Testing Emitted Events](#4-testing-emitted-events)
  - [Breaking down the test into steps](#breaking-down-the-test-into-steps)
  - [Finding the text input element and setting the value](#finding-the-text-input-element-and-setting-the-value)
  - [Simulating form submission](#simulating-form-submission)
  - [Testing emitted event and payload](#testing-emitted-event-and-payload)
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

### Vue CLI project creation

We create a project with Vue CLI. Note that Vue CLI will create a directory with the name given, and initialize the Git repository.

```sh
~
❯ cd ~/dev/vue-mastery

~/dev/vue-mastery
❯ vue create vue-mastery-testing-app
```

```
Vue CLI v4.3.0
? Please pick a preset: Manually select features
? Check the features needed for your project: Babel, Router, Vuex, Linter, Unit
? Use history mode for router? (Requires proper server setup for index fallback in production) Yes
? Pick a linter / formatter config: Prettier
? Pick additional lint features: Lint on save, Lint and fix on commit
? Pick a unit testing solution: Jest
? Where do you prefer placing config for Babel, ESLint, etc.? In dedicated config files
? Save this as a preset for future projects? (y/N) N
```

I then created the GitHub remote with the [GitHub CLI](https://cli.github.com/). My remote is at [br3ndonland/vue-mastery-testing-app](https://github.com/br3ndonland/vue-mastery-testing-app).

```sh
~/dev/vue-mastery
❯ cd vue-mastery-testing-app

~/dev/vue-mastery/vue-mastery-testing-app master
❯ gh repo create br3ndonland/vue-mastery-testing-app --public --enable-issues=false --enable-wiki=false -d "Vue CLI 4 app for Vue Mastery testing course"
✓ Created repository br3ndonland/vue-mastery-testing-app on GitHub
✓ Added remote https://github.com/br3ndonland/vue-mastery-testing-app.git

~/dev/vue-mastery/vue-mastery-testing-app master
❯ git remote set-url origin git@github.com:br3ndonland/vue-mastery-testing-app.git

~/dev/vue-mastery/vue-mastery-testing-app master
❯ git pso master
```

### Unit testing steps

1. **Create test suite** (block of tests): `describe()`
2. **Set up tests**: `test()`
3. **Mount component** with Vue Test Utils: `mount()`
4. **Set data** if necessary: `setData()`
5. **Assert** the expected result: `expect()`

We create a new component, _AppHeader.vue_, and a test file, _AppHeader.spec.js_.

- The component should display a logout button in the header if the user is logged in.
- The input is `loggedIn`.
- The output is the `<button>`.
- We write tests using the [test method](https://jestjs.io/docs/en/api#testname-fn-timeout) for when users are logged in and logged out, according to the unit testing steps above.
- We assert the expected result with [expect](https://jestjs.io/docs/en/expect) and [matchers](https://jestjs.io/docs/en/using-matchers).
- We have to make the tests wait until the button is mounted to the DOM before they run, so we use async/await to [test asynchronous code](https://jestjs.io/docs/en/asynchronous).

Other things to note:

- Imports from Vue Test Utils use the JavaScript import syntax with curly braces, like `import { mount } from "@vue/test-utils"`, because we are importing specific method exports from Vue Test Utils. Vue component imports normally omit curly braces. See the [Vue Test Utils getting started docs](https://vue-test-utils.vuejs.org/guides/).
- After building _AppHeader.vue_, we import it into _App.vue_ and use it in the template. Although the title of _AppHeader.vue_ is `PascalCase`, we can apparently use the `kebab-case` variant directly in the template, `<app-header />`, without having to explicitly tell Vue.
- Lesson notes on `mount` vs `shallowMount`:
  > In the Vue Test Utils you will also find the method `shallowMount()`. If your component has children, `shallowMount()` will return a simple implementation of that component instead of a fully rendered version. This is important because the focus of a unit test is the component in isolation and not the children of that component.

We don't have a login functionality in the app right now, but we can try changing `loggedIn` with Vue devtools:

<img src="img/vm-testing-02-devtools.png" alt="Checking login button with Vue devtools" width="600px">

## 3. Testing Props & User Interaction

### Random number component

- We create a component that accepts `min` and `max` props as inputs, and generates a random number between `min` and `max` as the output.
- See _[src/components/RandomNumber.vue](https://github.com/br3ndonland/vue-mastery-testing-app/blob/master/src/components/RandomNumber.vue)_.

### Random number component tests

Tests to write:

1. **Check default value:** `randomNumber` default value should be 0
2. **Simulate interaction:** When generated by clicking the button, `randomNumber` should be between the `min` (`1`) and `max` (`10`).
3. **Change prop values:** If `min` and `max` are changed, and the button is clicked again, `randomNumber` should be between the new `min` and `max`.

As explained in the first lesson, we don't need to test prop types. **If the component has prop types and defaults specified, those will be checked by the Vue core library itself, and don't need further unit tests.**

_[tests/unit/RandomNumber.spec.js](https://github.com/br3ndonland/vue-mastery-testing-app/blob/master/tests/unit/RandomNumber.spec.js)_

```js
describe("RandomNumber", () => {
  test("Check default value: randomNumber default should be 0", () => {
    const wrapper = mount(RandomNumber)
    expect(wrapper.html()).toContain("<span>0</span>")
  })
  test("Simulate interaction: button -> 0 < randomNumber < 10", async () => {
    const wrapper = mount(RandomNumber)
    wrapper.find("button").trigger("click")
    await wrapper.vm.$nextTick()
    const randomNumber = parseInt(wrapper.find("span").element.textContent)
    expect(randomNumber).toBeGreaterThanOrEqual(1)
    expect(randomNumber).toBeLessThanOrEqual(10)
  })
  test("Change prop values: new min < randomNumber < new max", async () => {
    const wrapper = mount(RandomNumber, {
      propsData: {
        min: 200,
        max: 300,
      },
    })
    wrapper.find("button").trigger("click")
    await wrapper.vm.$nextTick()
    const randomNumber = parseInt(wrapper.find("span").element.textContent)
    expect(randomNumber).toBeGreaterThanOrEqual(200)
    expect(randomNumber).toBeLessThanOrEqual(300)
  })
})
```

In the third test where we change prop values, we change the props in a slightly different manner than we did in _AppHeader.spec.js_ during lesson 2. If we attempt to set the data using `wrapper.setData({ min: 200, max: 300 })` after mounting `RandomNumber`, the test passes with two warnings:

```
[Vue warn]: Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders. Instead, use a data or computed property based on the prop's value. Prop being mutated: "min"

[Vue warn]: Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders. Instead, use a data or computed property based on the prop's value. Prop being mutated: "max"
```

To avoid these warnings, set the props when mounting the component, using `propsData`.

## 4. Testing Emitted Events

### Breaking down the test into steps

We follow our unit testing steps:

1. **Create test suite** (block of tests): `describe()`
2. **Set up tests**: `test()`
3. **Mount component** with Vue Test Utils: `mount()`
4. **Set data** if necessary: `setData()`
5. **Assert** the expected result: `expect()`

When setting up tests (step 2), it helps to break down the test suite into steps based on how users will interact with the application:

1. **Find text input**
2. **Set value for text input**
3. **Simulate form submission**
4. **Assert event has been emitted**
5. **Assert payload is correct**

### Finding the text input element and setting the value

The simplest method for locating the text input element to test is to search for the input type directly: `const input = wrapper.find('input[type="text"]')`. However, in the future, more input elements could be added, or the input type could change.

To make our location method more specific, we can assign a test-specific attribute to the element. This decouples the test from the implementation details of the component.

### Simulating form submission

In order to decouple the test from the component, we don't want to make the test dependent on the submit button. Instead, we can use `wrapper.trigger("submit")` to simulate form submission without a submit button.

### Testing emitted event and payload

We start by simply checking that the event has been emitted from the component.

We then test the specific form value in the test submission. The form submission value is nested inside some JSON arrays, so we use `wrapper.emitted("formSubmitted")[0][0]` to locate the value.

## 5. Testing API Calls

## 6. Stubbing Child Components

_To be released on May 5_

## 7. Testing Vuex

_To be released on May 12_

## 8. Testing Vue Router

_To be released on May 19_
