# Mastering Vuex

[Vue Mastery](https://www.vuemastery.com)

[Vuex](https://vuex.vuejs.org/)

## Table of Contents <!-- omit in toc -->

- [1. Intro to Vuex](#1-intro-to-vuex)
- [2. Mastering Vuex Orientation](#2-mastering-vuex-orientation)
- [3. State & Getters](#3-state--getters)
- [4. Mutations & Actions Pt.1](#4-mutations--actions-pt1)
- [5. Mutations & Actions Pt.2](#5-mutations--actions-pt2)
- [6. Modules](#6-modules)
- [7. Success & Error Notifications](#7-success--error-notifications)

## 1. Intro to Vuex

[Lesson link](https://www.vuemastery.com/courses/mastering-vuex/intro-to-vuex)

Key concepts

- Managing state in an application full of components can be difficult.
- Facebook discovered this the hard way, which is why they created the flux design pattern.
- **State is the data your components depend on and render.** In large applications, each component can have its own state. If other components depend on that state, we need to communicate any state changes to the other components. Rather than controlling data changes with events and props, we can have one single source of truth for state. **Vuex state is reactive**, as the Vue instance's `data` property is.
- **Vuex state management pattern**

  - Just as the Vue instance has computed properties that can access data, the Vuex store has **getters** which can access our state.
  - **Mutations track state changes.**
  - **Actions commit mutations, which update state.**
  - Mutation names are typically written in capital letters with underscores, like `ACTION_NAME`, a convention started by flux.
  - Using the Vue dev tools, we can do "time travel debugging," rolling back mutations to revert the state to previous values.

  - Vue instance:

    ```js
    const app = new Vue({
      data: {},
      methods: {},
      computed: {},
    })
    ```

  - Vuex store:

    ```js
    const store = new Vuex.Store({
      state: {},
      mutations: {},
      actions: {},
      getters: {},
    })
    ```

- **Vuex state management workflow**

  <img src="img/vue-mastery-03-01-workflow.gif" alt="Vuex state management pattern example workflow" width="600px"/>

  Example store

  ```js
  const store = new Vuex.Store({
    state: {
      isLoading: false,
      todos: [],
    },
    mutations: {
      SET_LOADING_STATUS(state) {
        state.isLoading = !state.isLoading
      },
      SET_TODOS(state, todos) {
        state.todos = todos
      },
    },
    actions: {
      fetchTodos(context) {
        context.commit("SET_LOADING_STATUS")
        axios.get("/api/todos").then((response) => {
          context.commit("SET_LOADING_STATUS")
          context.commit("SET_TODOS", response.data.todos)
        })
      },
    },
  })
  ```

  To kick this off, the `fetchTodos` action is dispatched from a component with `this.$store.dispatch('fetchTodos')`.

## 2. Mastering Vuex Orientation

[Lesson link](https://www.vuemastery.com/courses/mastering-vuex/mastering-vuex-orientation)

Key concepts

- We'll be adding Vuex to the app we built in the real world Vue course.
- Adam reviews the setup of the app.

```sh
~/dev/vue-mastery/vm02-app
❯ json-server --watch db.json --port 8081
```

```sh
~/dev/vue-mastery/vm02-app
❯ npm run serve
```

## 3. State & Getters

[Lesson link](https://www.vuemastery.com/courses/mastering-vuex/vuex-state-getters)

Key concepts

- Accessing state from the Vuex store from within components:

  - Adding `store` to the Vue constructor in _main.js_ injects the store into all components. Any component can accesss the store with `$store`.

    ```js
    new Vue({
      router,
      store,
      render: (h) => h(App),
    }).$mount("#app")
    ```

  - Then, in _state.js_, we can start to add state.

    ```js
    import Vue from "vue"
    import Vuex from "vuex"

    Vue.use(Vuex)

    export default new Vuex.Store({
      state: {},
      mutations: {},
      actions: {},
    })
    ```

- Create computed properties from state

  - We can create computed properties to efficiently reuse data from the store. Vuex provides the `import { mapState } from 'vuex'` helper to efficiently generate computed properties for us.
  - Use ES6 spread operator to map local computed properties. See my [udacity-google-mws lesson notes](https://github.com/br3ndonland/udacity-google-mws/blob/master/lessons/2-ajax-es6-offline/es6/es6-1-syntax.md#114-spread-operator) where I learned this.
    ```js
    const books = [
      "Don Quixote",
      "The Hobbit",
      "Alice in Wonderland",
      "Tale of Two Cities",
    ]
    console.log(...books)
    ```
    ```
    Don Quixote The Hobbit Alice in Wonderland Tale of Two Cities
    ```

- Getters for derived state
  - _When should I use computed properties vs getters? Seem to accomplish the same thing._ -> Depends on where you are in the application. The Vue instance and components use computed properties, the Vuex store uses getters. Vuex store getters can be used elsewhere in the application.
- Mapping state and getters to computed properties
- Dynamic Getters
  - Use the `mapGetters` helper with `import { mapGetters } from 'vuex'`
- Checking out the lesson code

  ```sh
  ~
  ❯ cd dev/vue-mastery/vm02-app

  ~/dev/vue-mastery/vm02-app mastering-vuex
  ❯ gm lesson11-vuex-finish

  ~/dev/vue-mastery/vm02-app mastering-vuex
  ❯ prettier --write "**/*.vue" "**/*.js"
  ```

## 4. Mutations & Actions Pt.1

[Lesson link](https://www.vuemastery.com/courses/mastering-vuex/vuex-mutations-actions-1)

Key concepts, as introduced in [1. Intro to Vuex](#1-Intro-to-Vuex):

- Mutations update, or mutate, state.
- Actions commit mutations, which update state.
- **Mutations should always go within actions.** This improves scaleability.
- **Mutations are synchronous. Actions are asynchronous.**
- Mutation names are written in uppercase, like `MUTATION_NAME`.
- Pseudocode example: asking a friend to get bread. The request is like a mutation, the action is actually getting the bread.

  ```js
  export default new Vuex.Store({
    mutations: {
      PICK_UP_BREAD(state, bread) {
        state.bread = bread
      },
    },
    actions: {
      async pleasePickUpBread({ commit }) {
        if (possible) {
          let bread = await fetch("/bread")
          commit("PICK_UP_BREAD", bread)
        }
      },
    },
  })
  ```

Lesson code

- Merge

  ```sh
  ~/dev/vue-mastery/vm02-app mastering-vuex
  ❯ gm 'lesson12-mutations&actions1-finish'
  # fix merge conflicts with VSCode

  ~/dev/vue-mastery/vm02-app mastering-vuex
  ❯ prettier --write "**/*.vue" "**/*.js"

  ~/dev/vue-mastery/vm02-app mastering-vuex
  ❯ gaa

  ~/dev/vue-mastery/vm02-app mastering-vuex
  ❯ gci

  ```

- Install new dependency `vuejs-datepicker`
- Add to _EventCreate.vue_
- Add mutation and action to _store.js_
- Route to new event
- View the state in the Vue.js DevTools, Vuex tab.

## 5. Mutations & Actions Pt.2

[Lesson link](https://www.vuemastery.com/courses/mastering-vuex/vuex-mutations-actions-2)

Key concepts

- We use Vuex to retrieve and display events with the _EventList.vue_ component.
  <img src="img/vue-mastery-03-05-fetchEvents.png" alt="Vuex state mutations and actions workflow" width="600px"/>
- We implement [json-server pagination](https://github.com/typicode/json-server#paginate) with `_page` and `_limit`.
- _Problem: The component isn’t reloading_: This is similar to the key-change method I implemented in the iPACS. From _ForceRerender.js_:
  - The ForceRerender mixin uses key-changing to trigger component re-rendering.
  - The ForceRerender mixin is imported into a parent component. Mixins are typically .js files, so curly braces are needed for imports:
    ```js
    import { ForceRerender } from "common/mixins/ForceRerender.js"
    ```
  - In the parent component, when the child component is added to the `<template>`, it is bound to a key, componentKey, set to the integer 0:
    ```html
    <child-component :key="componentKey"></child-component>
    ```
  - The forceRerender method increments the key, triggering component re-render.
  - When the forceRerender method is called, it increments the key, and the child component is re-rendered.
  - This method is more specific than simply using the `created()` lifecycle hook, or `vm.$forceUpdate()` (https://vuejs.org/v2/api/#vm-forceUpdate) because it can be triggered by a specific event, such as closing a modal. `vm.$forceUpdate()` re-renders the entire Vue instance.
  - Links
    - https://michaelnthiessen.com/force-re-render/
    - https://redmine.invicro.com/issues/8010
- When paginating, we need to stop at the last page, and not allow clicking through to blank pages. We do this by determining the total pages from the HTTP headers, with `commit('SET_EVENTS_TOTAL', parseInt(response.headers['x-total-count']))` in _store.js_ `fetchEvents()`, and then determining whether or not the page has a next page in _EventList.vue_ `hasNextPage()`.
- Problem: we're loading data twice:
  - We already have the events loaded by _EventList.vue_. We don't need to fetch the events again when viewing a specific event with _EventShow.vue_. Instead, we can find the event in the events array.
    ```js
    export default {
      ...
      getters: {
        getEventById: state => id => {
          return state.events.find(event => event.id === id)
        }
      }
    }
    ```
  - This is a similar strategy to the one I used for editing contacts in _src/icro/forms/contacts/contacts-add.vue_ `getContactToEdit(selectedContactToEdit)`.
  - We then use the getter in `fetchEvent()`.
    ```js
    export default {
      methods: {
        async fetchEvent({ commit, getters, dispatch }, id) {
          const event = getters.getEventById(id)
          if (event) {
            commit("SET_EVENT", event)
          } else {
            try {
              let response = await EventService.getEvent(id)
              commit("SET_EVENT", response.data)
            } catch (e) {
              const notification = {
                type: "error",
                message: `There was a problem fetching the event: ${e.message}`,
              }
              dispatch("notification/add", notification, { root: true })
            }
          }
        },
      },
    }
    ```
- I, of course, refactored the actions in _store.js_ with async/await.

  ```js
  export default new Vuex.Store({
    ...
    mutations: {
      ADD_EVENT(state, event) {
        state.events.push(event)
      },
      SET_EVENTS(state, events) {
        state.events = events
      },
      SET_EVENTS_TOTAL(state, eventsTotal) {
        state.eventsTotal = eventsTotal
      },
      SET_EVENT(state, event) {
        state.event = event
      }
    },
    actions: {
      async createEvent({ commit }, event) {
        await EventService.postEvent(event)
        commit('ADD_EVENT', event)
      },
      async fetchEvent({ commit, getters }, id) {
        const event = getters.getEventById(id)
        if (event) {
          commit('SET_EVENT', event)
        } else {
          try {
            let response = await EventService.getEvent(id)
            commit('SET_EVENT', response.data)
          } catch (e) {
            throw Error(e)
          }
        }
      },
      async fetchEvents({ commit }, { perPage, page }) {
        try {
          let response = await EventService.getEvents(perPage, page)
          commit('SET_EVENTS_TOTAL', parseInt(response.headers['x-total-count']))
          commit('SET_EVENTS', response.data)
        } catch (e) {
          throw Error(e)
        }
      }
    },
    ...
  })
  ```

## 6. Modules

[Lesson link](https://www.vuemastery.com/courses/mastering-vuex/vuex-modules/)

[Vuex | Modules](https://vuex.vuejs.org/guide/modules.html)

Key concepts

- **Vuex modules** inside a _src/store_ directory. The main store file goes in _src/store/store.js_, and the modules go in _src/store/modules_.
- **Imports and [namespacing](https://vuex.vuejs.org/guide/modules.html#namespacing)**

  - Vue Mastery course instructors Gregg and Adam import the Vuex modules with into _store.js_ `import *`, which imports all public items into the user namespace. Using `import *` is frowned upon in Python but I guess it's okay here.

    ```js
    import * as user from "@/store/modules/user.js"
    import * as event from "@/store/modules/event.js"
    ```

  - The second option is to export one object. I decided to refactor with the second syntax option. `export const namespaced = true` becomes `namespaced: true`.

    <img src="img/vue-mastery-03-06-vuex-module-syntax.png" alt="Vuex module syntax options" width="600px"/>

  - Actions, mutations, and getters are always registered under the global namespace, and therefore called without their module name. This is because multiple actions and mutations may need to use the same name. [Namespacing](https://vuex.vuejs.org/guide/modules.html#namespacing) ensures that all mutations, actions, and getters are namespaced under the module of interest.
    - We namespace Vuex modules by adding `namespaced: true`.
    - We namespace actions in components by adding the module name before the action name, such as `event/createEvent`.
    - We namespace getters in the same manner, with
      ```js
      export default {
        computed: {
          getEventById() {
            return this.$store.getters['event/getEventById']
          }
        },
        // Which could be shortened to:
        computed: mapGetters('event', ['getEventById'])
      },
      ```

## 7. Success & Error Notifications

[Lesson link](https://www.vuemastery.com/courses/mastering-vuex/success-error-notifications)

Key concepts

- Now, we need to display success and error notifications to the user, in addition to console logging.
- Normally, these notifications would come from a component library, like Bootstrap or Vuetify, but it's helpful to get some practice implementing them manually.
- Test out the fetch events failure message by stopping the `json-server`.

**COURSE COMPLETE!!! I RULE!!!**

<img src="img/vue-mastery-03-complete.png" alt="Vue Mastery Mastering Vuex course completion page" width="600px" />
