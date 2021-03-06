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
  - [Setting up the database](#setting-up-the-database)
  - [Setting up the message display component](#setting-up-the-message-display-component)
  - [Setting up a test for the message display component](#setting-up-a-test-for-the-message-display-component)
  - [Mocking API calls](#mocking-api-calls)
  - [Awaiting promise resolution](#awaiting-promise-resolution)
  - [Asserting `expect()`ed outcomes](#asserting-expected-outcomes)
  - [Getting the correct number of API calls by clearing mocks](#getting-the-correct-number-of-api-calls-by-clearing-mocks)
  - [Refactoring](#refactoring)
- [6. Stubbing Child Components](#6-stubbing-child-components)
  - [Stubbing](#stubbing)
  - [`mount()` vs `shallowMount()`](#mount-vs-shallowmount)
- [7. ~~Testing Vuex~~](#7-stesting-vuexs)
- [8. ~~Testing Vue Router~~](#8-stesting-vue-routers)
- [Production unit testing course](#production-unit-testing-course)
- [Upgrading Vue Test Utils](#upgrading-vue-test-utils)
  - [Removing `nextTick()` and `await`ing DOM methods instead](#removing-nexttick-and-awaiting-dom-methods-instead)
  - [Correcting the `isVisible()` deprecation warning](#correcting-the-isvisible-deprecation-warning)
  - [Avoiding use of strings for stubs](#avoiding-use-of-strings-for-stubs)

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
    await wrapper.find("button").trigger("click")
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
    await wrapper.find("button").trigger("click")
    const randomNumber = parseInt(wrapper.find("span").element.textContent)
    expect(randomNumber).toBeGreaterThanOrEqual(200)
    expect(randomNumber).toBeLessThanOrEqual(300)
  })
})
```

_Update:_ For vue-test-utils ≥[v1.0.0](https://github.com/vuejs/vue-test-utils/releases/tag/v1.0.0), the `vm.$nextTick()` from the lesson code can be removed, and DOM methods can be directly `await`ed instead.

```js
// before
wrapper.find("button").trigger("click")
await wrapper.vm.$nextTick()

// after
await wrapper.find("button").trigger("click")
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

### Setting up the database

We create a small JSON database in _db.json_ with `json-server`. Spin it up with `json-server --watch db.json`. We also add an API service layer in _services/axios.js_, to simplify API calls with Axios. This consolidates our API calls into one file.

```js
import axios from "axios"

export async function getMessage() {
  const r = await axios(`http://localhost:3000/message`)
  return r.data
}
```

### Setting up the message display component

_MessageDisplay.vue_ is a small notification container.

```vue
<template>
  <p v-if="error" data-testid="message-error">{{ error }}</p>
  <p v-else data-testid="message">{{ message.text }}</p>
</template>
<script>
import { getMessage } from "@/services/axios.js"
export default {
  data() {
    return {
      message: {},
      error: null,
    }
  },
  async created() {
    try {
      this.message = await getMessage()
    } catch (e) {
      this.error = `Error: ${e}`
      throw Error(e)
    }
  },
}
</script>
<style scoped></style>
```

### Setting up a test for the message display component

Again, we follow our unit testing steps:

1. **Create test suite** (block of tests): `describe()`
2. **Set up tests**: `test()`
3. **Mount component** with Vue Test Utils: `mount()`
4. **Set data** if necessary: `setData()`
5. **Assert** the expected result: `expect()`

We also break down each test into steps:

1. **Mock API call**
2. **Await promise resolution**
3. **Assert that API call happened once**: `expect()`
4. **Assert that component displays message**: `expect()`

### Mocking API calls

Relying on actual back-end API calls would couple our tests to the back-end, and prevent us from running tests in continuous integration (CI). To decouple our tests from the back-end, we **mock** the API calls with the [Jest Mock Functions API](https://jestjs.io/docs/en/mock-functions), particularly [`mockResolvedValueOnce()`](https://jestjs.io/docs/en/mock-function-api.html#mockfnmockresolvedvalueoncevalue).

In addition to mocking successful API calls, we also need to mock failed API calls. For this, we use [`mockRejectedValueOnce()`](https://jestjs.io/docs/en/mock-function-api.html#mockfnmockrejectedvalueoncevalue).

### Awaiting promise resolution

We're testing a method inside the component's `created()` lifecycle hook. Vue Test Utils can't `await` promises inside the `created()` hook, so we use [`flush-promises`](https://www.npmjs.com/package/flush-promises) to ensure promises are resolved prior to running assertions with `expect()`.

### Asserting `expect()`ed outcomes

We use the Jest `expect()` method to assert two expected outcomes:

1. The API call is made the correct number of times (1)
2. The message contains the expected content

To evaluate the content of the message, we have to parse the `textContent` of the HTML element:

```js
const errorContent = wrapper.find(`[data-testid="message-error"]`).element
  .textContent
```

Note the square brackets around the attribute to be found. As we saw in the previous lesson, HTML elements go outside the square brackets, attributes of those elements go inside the square brackets. Here, we're simply looking for the attribute and remaining agnostic to the element used for it, which gives the test added flexibility.

### Getting the correct number of API calls by clearing mocks

The second test will fail because it sees two API calls. The mock API call from the first test has not been cleared out by Jest. We need to tell Jest to `clearAllMocks()` by adding a method to the beginning of _MessageDisplay.spec.js_.

```js
jest.mock("@/services/axios")
beforeEach(() => {
  jest.clearAllMocks()
})
```

Note that this can also be [specified in the Jest config](https://jestjs.io/docs/en/configuration#clearmocks-boolean) (either in _package.json_ or _jest.config.js_).

### Refactoring

In the lesson, we used `expect().toEqual()` when asserting the error message. The `toEqual()` method requires strict/exact equality, which won't work if any text is added to the error message, such as from a template string. Using `toContain()` instead allows Jest to do a regex search inside the error string.

I wanted to consolidate the try/catch error handling into the API call in _axios.js_. I tried this, but throwing an error from `getMessage()` in _axios.js_, then calling `getMessage()` from within the `created()` hook in _MessageDisplay.vue_ results in a Jest error:

```
 FAIL  tests/unit/MessageDisplay.spec.js
  ● MessageDisplay › displays an error if getMessage fails

    TypeError: Cannot read property 'textContent' of undefined

      34 |     expect(getMessage).toHaveBeenCalledTimes(1)
      35 |     // 4. Assert that component displays message
    > 36 |     const errorContent = wrapper.find(`[data-testid="message-error"]`).element
         |                          ^
      37 |       .textContent
      38 |     expect(errorContent).toContain(mockError)
      39 |   })

      at Object.<anonymous> (tests/unit/MessageDisplay.spec.js:36:26)
```

This is odd, because Vue doesn't throw any errors. Vue returns a warning when errors are thrown in `created()` lifecycle hooks, but the element mounts and renders in the actual app. In the browser console, the element content shows up:

```js
>>> document.querySelector("[data-testid=message-error]").textContent
"Network Error: getMessage could not connect to the database."
```

Removing `.element` to match the browser console doesn't change the result.

There may be a different [Jest matcher](https://jestjs.io/docs/en/using-matchers) I could use, or some [DOM manipulation](https://jestjs.io/docs/en/tutorial-jquery) I could do.

I noticed that [others have had the same issue](https://github.com/Code-Pop/Unit-Testing/issues/1):

> I've even added a line to display `<MessageDisplay />` in the `AppHeader.vue` to make sure the `json-server` is up and running.

- Mounting the `AppHeader` instead doesn't fix the issue: `const wrapper = mount(AppHeader)`
- Using `shallowMount()` instead doesn't fix the issue: `const wrapper = shallowMount(MessageDisplay)`
- Importing `MessageDisplay` directly in _App.vue_, instead of in _AppHeader.vue_ doesn't fix the issue:

  ```vue
  <template>
    <div id="app">
      <app-header />
      <message-display />
      <div id="nav">
        <router-link to="/">Home</router-link> |
        <router-link to="/about">About</router-link>
      </div>
      <router-view />
    </div>
  </template>

  <script>
  import AppHeader from "@/components/AppHeader"
  import MessageDisplay from "@/components/MessageDisplay.vue"
  export default {
    components: {
      AppHeader,
      MessageDisplay,
    },
  }
  </script>

  <style></style>
  ```

- Doesn't appear due to some difference between `mockResolvedValueOnce` and `mockRejectedValueOnce`
- In _src/components/MessageDisplay.vue_, the error element is only displayed `v-if` Vue has an `error` in its `data()`. By default, we have set `error: null`.

  ```vue
  <template>
    <p v-if="error" data-testid="message-error">{{ error }}</p>
    <p v-else data-testid="message">{{ message.text }}</p>
  </template>

  <script>
  import { getMessage } from "@/services/axios.js"
  export default {
    data() {
      return {
        message: {},
        error: null,
      }
    },
    async created() {
      try {
        this.message = await getMessage()
      } catch (err) {
        this.error = "Oops! Something went wrong."
      }
    },
  }
  </script>
  ```

- As we did in _AppHeader.spec.js_, we can `setData()` when mounting the component, but this doesn't fix the issue.
- **The error seems to have something to do with mounting _MessageDisplay.vue_. I could never figure out what was causing the error, and it mysteriously resolved in both [my repo](https://github.com/br3ndonland/vue-mastery-testing-app) as well as the Vue Mastery repo, so I just moved on.**

## 6. Stubbing Child Components

### Stubbing

A **stub** is a substitute version of a component for testing. Advantages:

- Isolate what you're testing
- Test one thing at a time
- Pinpoint the specific code that's broken

This concept is similar to [Jest manual mocks](https://jestjs.io/docs/en/manual-mocks), which allow you to mock entire components or Node modules.

Stubs should be used sparingly. Overuse of stubs increases the maintenance cost of your tests, because you depend on manually written stubs.

### `mount()` vs `shallowMount()`

In addition to `mount()`, Jest also offers a `shallowMount()` method, which only mounts the component being tested without the rest of the component hierarchy. Use of `shallowMount()` can have the same disadvantages as overuse of stubs, and is not always supported in other libraries like [Vue Testing Library](https://testing-library.com/docs/vue-testing-library/intro). Vue Testing Library [favors](https://testing-library.com/docs/guiding-principles) testing DOM nodes over isolated components. For more about `mount()` vs `shallowMount()`, listen to the [Enjoy the Vue podcast episode 9](https://enjoythevue.io/episodes/9/).

## 7. ~~Testing Vuex~~

~~To be released on May 12~~

## 8. ~~Testing Vue Router~~

~~To be released on May 19~~

## Production unit testing course

**This course was originally going to have lessons on testing Vuex and Vue Router, but the lessons were moved to a separate upcoming course.**

## Upgrading Vue Test Utils

Vue Test Utils 1.0 is [here](https://github.com/vuejs/vue-test-utils/releases). There were a couple of updates to make to the tests after upgrading Vue Test Utils.

### Removing `nextTick()` and `await`ing DOM methods instead

```js
// before
wrapper.trigger("click")
await wrapper.vm.$nextTick()

// after
await wrapper.trigger("click")
```

### Correcting the `isVisible()` deprecation warning

Warning:

```
[vue-test-utils]: isVisible is deprecated and will be removed in the next major version.
Consider a custom matcher such as those provided in jest-dom: https://github.com/testing-library/jest-dom#tobevisible.
When using with findComponent, access the DOM element with findComponent(Comp).element.
```

Solution:

- Import [jest-dom](https://github.com/testing-library/jest-dom): `import { toBeVisible } from "@testing-library/jest-dom/matchers"`
- [Extend `expect()`](https://jestjs.io/docs/en/expect#expectextendmatchers) with `toBeVisible()` from jest-dom: `expect.extend({ toBeVisible })`
- Update Jest tests to use `toBeVisible()` instead of `isVisible()`:

  ```js
  import { mount } from "@vue/test-utils"
  import { toBeVisible } from "@testing-library/jest-dom/matchers"
  import AppHeader from "@/components/AppHeader"

  expect.extend({ toBeVisible })

  describe("AppHeader", () => {
    it("shows the logout button if user is logged in", async () => {
      const wrapper = mount(AppHeader)
      await wrapper.setData({ loggedIn: true })
      // expect(wrapper.find("button").isVisible()).toBe(true)
      expect(wrapper.find("button").element).toBeVisible()
    })
    it("doesn't show logout button if user is not logged in", async () => {
      const wrapper = mount(AppHeader)
      // loggedIn is false, so we don't have to use setData()
      // expect(wrapper.find("button").isVisible()).toBe(false)
      expect(wrapper.find("button").element).not.toBeVisible()
    })
  })
  ```

- See [vue-test-utils#1532](https://github.com/vuejs/vue-test-utils/issues/1532) for more details.

### Avoiding use of strings for stubs

Vue Test Utils now throws the following warning:

```
[vue-test-utils]: Using a string for stubs is deprecated and will be removed in the next major version.
```

Confusingly, the solution is not in the [Vue Test Utils docs on stubbing components](https://vue-test-utils.vuejs.org/guides/#stubbing-components), but actually in the [Vue Test Utils docs on the `mount()` API](https://vue-test-utils.vuejs.org/api/#mount). The solution, based on the [example in the docs](https://vue-test-utils.vuejs.org/api/#mount) and on [what Vue Test Utils currently does](https://github.com/vuejs/vue-test-utils/blob/v1.0.3/packages/create-instance/create-component-stubs.js#L161), is to create a new object for the stub, and add the HTML string to the template key:

```js
// tests/unit/MessageContainer.spec.js
import MessageContainer from "@/components/MessageContainer"
import { mount } from "@vue/test-utils"

describe("MessageContainer", () => {
  it("Wraps MessageDisplay component", () => {
    /* old:
    const wrapper = mount(MessageContainer, {
      stubs: {
        MessageDisplay: '<p data-testid="message">Hello from the db!</p>',
      },
    })
    */
    const stubMessage = "Hello from the db!"
    const wrapper = mount(MessageContainer, {
      stubs: {
        MessageDisplay: {
          template: `<p data-testid="message">${stubMessage}</p>`,
        },
      },
    })
    const message = wrapper.find('[data-testid="message"]').element.textContent
    expect(message).toEqual(stubMessage)
  })
})
```
