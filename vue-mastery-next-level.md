# Next-Level Vue

[Vue Mastery](https://www.vuemastery.com)

## Table of Contents <!-- omit in toc -->

- [01. Next-Level Vue: Orientation](#01-next-level-vue-orientation)
- [02. Progress Bar: Axios Interceptors](#02-progress-bar-axios-interceptors)
- [03. In-Component Route Guards](#03-in-component-route-guards)
- [04. Global and Per-Route Guards](#04-global-and-per-route-guards)
- [05. Completing our Progress Bar](#05-completing-our-progress-bar)
- [06. Error Handling](#06-error-handling)
- [07. Reusable Form Components: BaseInput](#07-reusable-form-components-baseinput)
- [08. Reusable Form Components: BaseSelect](#08-reusable-form-components-baseselect)
- [09. Reusable Form Components: BaseButton](#09-reusable-form-components-basebutton)
- [10. Form Validation with Vuelidate](#10-form-validation-with-vuelidate)
- [11. Form Validation with Vuelidate Pt. 2](#11-form-validation-with-vuelidate-pt-2)
- [12. Mixins](#12-mixins)
- [13. Filters](#13-filters)

Starting code: [tag `progress-bar-start`](https://github.com/Code-Pop/real-world-vue/releases/tag/progress-bar-start).

## 01. Next-Level Vue: Orientation

## 02. Progress Bar: Axios Interceptors

- Axios interceptors can intercept requests and responses. It's like middleware.
- Code: [tag `progress-bar-axios-interceptors-finish`](https://github.com/Code-Pop/real-world-vue/releases/tag/progress-bar-axios-interceptors-finish)
- After adding NProgress, we can see a progress bar each time we navigate to another page.
- Caveats:
  - Not optimal for multiple API calls at the same time
  - Templates get rendered before the API call returns data (so pages look empty).

## 03. In-Component Route Guards

- In this lesson, we use new navigation hooks called [route guards](https://router.vuejs.org/guide/advanced/navigation-guards.html#in-component-guards).
- Code: [tag progressbar-in-component-route-guard-finish](https://github.com/Code-Pop/real-world-vue/releases/tag/progressbar-in-component-route-guard-finish)

## 04. Global and Per-Route Guards

- Code: [tag progress-bar-global-guard](https://github.com/Code-Pop/real-world-vue/releases/tag/progress-bar-global-guard)
- [Global Guard Docs](https://router.vuejs.org/guide/advanced/navigation-guards.html#global-guards)
- [Per-Route Guard Docs](https://router.vuejs.org/guide/advanced/navigation-guards.html#per-route-guard)
- Guards are like hooks that run whenever navigation is triggered.
- To add per-route guards, we add the route methods directly to the array of routes in _router.js_.
- To accommodate global guards, the router is set to a `const` instead of directly exported. After the `router.beforeEach()` and `router.afterEach()` global guards, we then `export default router`.
- We also remove the Vuex dependency of the _EventShow.vue_ component.
  - Specifying prop types is the first step.
  - In some simple cases, it's easier to test and maintain if we keep state management local to the component.
  - See [Markus Oberlehner | Blog 20180527: Should I store this data in Vuex?](https://markus.oberlehner.net/blog/should-i-store-this-data-in-vuex/).

## 05. Completing our Progress Bar

- Code: [tag progress-bar-finished](https://github.com/Code-Pop/real-world-vue/releases/tag/progress-bar-finished)
- Here, we also implement in-component route guards.

## 06. Error Handling

- Catch-all 404 page:
  - Create _src/views/NotFound.vue_ component
  - Add _NotFound.vue_ to _router.js_
  - Add catch-all route to _router.js_
- Network issue:
  - Create _src/views/NetworkIssue.vue_
- Code:[tag error-handling-finish](https://github.com/Code-Pop/real-world-vue/releases/tag/error-handling-finish)

## 07. Reusable Form Components: BaseInput

- We added code for automatic global component registration in the [real-world vue course, lesson 7](https://www.vuemastery.com/courses/real-world-vue-js/global-components).
- Code: [tag BaseInput-finish](https://github.com/Code-Pop/real-world-vue/releases/tag/BaseInput-finish)

## 08. Reusable Form Components: BaseSelect

- We start using [vue-multiselect](https://vue-multiselect.js.org/) here.
- Code: [tag baseSelect-finish](https://github.com/Code-Pop/real-world-vue/releases/tag/baseSelect-finish)

## 09. Reusable Form Components: BaseButton

- Code: [tag baseButton-finish](https://github.com/Code-Pop/real-world-vue/releases/tag/baseButton-finish)

## 10. Form Validation with Vuelidate

- We start using [Vuelidate](https://vuelidate.netlify.com/) in this lesson.
  - We can use `@blur` to only validate when the user changes focus. Many websites could benefit from using this event handler. Usually, the form starts yelling at you as soon as you type, which is very annoying.
  - As soon as a user starts typing, `$touch` is activated
  - After `$touch` but before validation, the field is considered `$dirty`
  - `$dirty` + `$invalid` = `$error`
- We also add the usual `@submit.prevent="submit"` in the template, then add the `submit()` method to run the validation.
- Code: [tag form_validation1_finish](https://github.com/Code-Pop/real-world-vue/releases/tag/form_validation1_finish)

## 11. Form Validation with Vuelidate Pt. 2

- We add a `validations: {}` object to the exports in _vm02-app/src/views/EventCreate.vue_. As explained in the last lesson, we use the `$error` state to conditionally display a `div` containing the error message.
- _vm02-app/src/components/BaseSelect.vue:_
  - In the parent component _src/views/EventCreate.vue_, we added the `@blur` listener to the _src/components/BaseSelect.vue_ child component.
  - We then need to give the `<select>` element in _src/components/BaseSelect.vue_ the ability to inherit the event listener.
  - As in lesson 9 on the BaseButton, we add `v-on="$listeners"` to the child component so it inherits the event listeners added in the parent component scope.
- Resolving event listener conflict in _vm02-app/src/components/BaseInput.vue_:
  - If we use `v-on="$listeners"` to inherit from the parent, it conflicts with the `v-model="event.title"` event already added (remember that `v-model` is syntactic sugar for `v-bind` and `@input`).
  - Add a `listeners()` computed, use the [rest operator](https://github.com/br3ndonland/udacity-google-mws/blob/master/lessons/2-ajax-es6-offline/es6/es6-1-syntax.md#115-rest-parameter) to mix in listeners with `...this.$listeners`, then update value on input with `input: this.updateValue`. The method lower down in the call stack takes precedence, so the value displayed will come from `input: this.updateValue`.
- We also work with [Vue.js Datepicker](https://www.npmjs.com/package/vuejs-datepicker).
- We disable the submit button when errors are seen with `:disabled="$v.$anyError"` on `BaseButton`, and add a global error message.
- Code: [tag vuelidateP2-finish](https://github.com/Code-Pop/real-world-vue/releases/tag/vuelidateP2-finish)

## 12. Mixins

- I already learned how to make mixins for the iPACS.
- Mixin data is secondary to component data. If both have the same object or methods, the component takes precedence.
- For global mixins (mixed in to every component), add `Vue.mixin()` to the _main.js_ file. Be careful with this, because every component in your application will contain this mixin.
- Code: [tag mixins_finish](https://github.com/Code-Pop/real-world-vue/releases/tag/mixins_finish)

## 13. Filters

- I hadn't implemented filters yet. These could be a useful alternative to the computed properties I use to display names and such to the user.
- Filters are stored in separate files like mixins. If you `export default`, the filter can be imported with any name you like.

  ```js
  export default value => {
    const date = new Date(value)
    return date.toLocaleString(["en-US"], {
      month: "short",
      day: "2-digit",
      year: "numeric"
    })
  }
  ```

- Data can be piped into the filter. From _EventCard.vue_:
  ```html
  <span class="eyebrow">@{{ event.time }} on {{ event.date | date }}</span>
  ```
- Code: [tag filters_FINISH](https://github.com/Code-Pop/real-world-vue/releases/tag/filters_FINISH)

**COURSE COMPLETE!!! I RULE!!!**

<img src="img/vm-next-level-complete.png" alt="Vue Mastery Next-Level Vue course completion page" width="600px" />
