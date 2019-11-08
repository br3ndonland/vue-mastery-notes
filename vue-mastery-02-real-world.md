# Real-world Vue.js

[Vue Mastery](https://www.vuemastery.com)

[Vue Mastery Example Event App](https://github.com/code-pop/real-world-vue)

Instructor: Adam Jahr

## Table of Contents <!-- omit in toc -->

- [1. Intro to Real World Vue](#1-Intro-to-Real-World-Vue)
- [2. Vue CLI 3 - Creating our Project](#2-Vue-CLI-3---Creating-our-Project)
- [3. Optimizing your Editor](#3-Optimizing-your-Editor)
- [4. Vue Router Basics](#4-Vue-Router-Basics)
  - [Lesson](#Lesson)
  - [Installing the event app](#Installing-the-event-app)
  - [Running the event app](#Running-the-event-app)
- [5. Dynamic Routing & History Mode](#5-Dynamic-Routing--History-Mode)
- [6. Single File Vue Components](#6-Single-File-Vue-Components)
- [7. Global Components](#7-Global-Components)
- [8. Slots](#8-Slots)
- [9. API calls with Axios](#9-API-calls-with-Axios)

## 1. Intro to Real World Vue

- [Lesson link](https://www.vuemastery.com/courses/vm02-real-world-vue-js/real-world-intro)

## 2. Vue CLI 3 - Creating our Project

- [Lesson link](https://www.vuemastery.com/courses/vm02-real-world-vue-js/vue-cli)
- [Vue CLI](https://cli.vuejs.org/)
  - Command line interface to configure projects.
  - Really great features. Configures Webpack to bundle, optimize, and minify JS. Configures Babel.
  - HMR: Hot Module Replacement. Live updates whenever projects are saved.
- We configure a project with Vue CLI. They have Prettier built in. Note that creation of a project will include an initial Git commit. I hadn't configured Git on this machine yet, so I went ahead and did that.

  ```sh
  ~
  ❯ git config --global user.name 'Brendon Smith'
  ~
  ❯ git config --global user.email bsmith@invicro.com
  ```

  ```sh
  ~/dev
  ❯ npm i -g @vue/cli
  ~/dev
  ❯ mkdir vue-mastery
    ~/dev
  ❯ cd vue-mastery
  ~/dev/vue-mastery
  ❯ vue create vm02-real-world-vue
  ```

  ```text
  Vue CLI v3.5.1
  ? Please pick a preset: Manually select features
  ? Check the features needed for your project: Babel, Router, Vuex, Linter
  ? Use history mode for router? (Requires proper server setup for index fallback in production) No
  ? Pick a linter / formatter config: Prettier
  ? Pick additional lint features: (Press <space> to select, <a> to toggle all, <i> to invert selection) Lint on save
  ? Where do you prefer placing config for Babel, PostCSS, ESLint, etc.? In dedicated config files
  ? Save this as a preset for future projects? (y/N)
  ```

  ```sh
  ~/dev/vue-mastery
  ❯ cd vm02-real-world-vue
  ~/dev/vue-mastery/vm02-real-world-vue
  ❯ npm run serve
  ```

- Future clones of the repo should run `npm i` instead of `vue create`:

  ```sh
  ~/dev/vue-mastery
  ❯ cd ~/dev/vue-mastery/vm02-real-world-vue
  ~/dev/vue-mastery/vm02-real-world-vue
  ❯ npm i
  ~/dev/vue-mastery/vm02-real-world-vue
  ❯ npm run serve
  ```

- `npm run` is an alias for the [`npm run-script`](https://docs.npmjs.com/cli/run-script.html) command, used to run package scripts. In this case, we are running the [`serve` module](https://www.npmjs.com/package/serve).
- Browse to http://localhost:8080/ to see the app.
- Vue UI:
  - We also use the Vue UI, which is a user interface version of the CLI.
    - Launch Vue UI from terminal with `vue ui`. It will open in the browser and provide terminal output as well.
    - In addition to the capabilities of the Vue CLI, it also has analytics, and a list of plugins like Vuetify.
    - Projects can be imported into VueUI with the `import` feature.
- We walk through the project file structure.

  - _public/_: files that won't be processed by Webpack.
    - _index.html_ is a template into which Vue injects the app.
  - _src/_: application-specific code

    - _assets/_: images, fonts, etc
    - _components/_: Vue app components
    - _views/_: app pages
    - _App.vue_ is the root component in which the other components are nested.
    - _main.js_ imports the other parts of the app, then uses the `Vue` class to render the app and mount it to the DOM.

      ```js
      import Vue from "vue"
      import App from "./App.vue"
      import router from "./router"
      import store from "./store"

      Vue.config.productionTip = false

      new Vue({
        router,
        store,
        render: h => h(App)
      }).$mount("#app")
      ```

    - _router.js_ is for Vue router
    - _store.js_ is for Vuex.

- Build

  - The _public/index.html_ file has a comment that says `<!-- built files will be auto injected -->`.
  - We then run `npm run build`, and see a _dist/_ directory with files optimized for deployment and distribution.
  - We can then see _dist/index.html_, into which the script files have been injected.

## 3. Optimizing your Editor

- [Lesson link](https://www.vuemastery.com/courses/vm02-real-world-vue-js/optimizing-your-editor)
- Key concepts

  - VSCode
  - Vetur
  - Configuring ESLint+Prettier:

    - We selected "dedicated config files" when creating the project, which gave us the _.eslintrc.js_ instead of putting it inside the _package.json_.
    - Need to add some extra config to _.eslintrc.js_. Add `'plugin:prettier/recommended'`:

      ```js
      module.exports = {
        root: true,
        env: {
          node: true
        },
        extends: [
          "plugin:vue/essential",
          "plugin:prettier/recommended",
          "@vue/prettier"
        ],
        rules: {
          "no-console": process.env.NODE_ENV === "production" ? "error" : "off",
          "no-debugger": process.env.NODE_ENV === "production" ? "error" : "off"
        },
        parserOptions: {
          parser: "babel-eslint"
        }
      }
      ```

    - We then add a Prettier config file, with a slightly different syntax than usual. We save it as a _.js_ file, and because Prettier is installed as an npm module, we add the config inside `module.exports`.

      ```js
      module.exports = {
        semi: false
      }
      ```

    - I didn't set up the ESLint auto-fixing, because I have the Prettier extension installed.
    - After installing Sarah Drasner's Vue snippets extension, need to turn off Vetur's snippets:

      ```json
      // settings.json
      {
        ...
        "vetur.completion.useScaffoldSnippets": false,
        ...
      }
      ```

  - At this point, I started adding Git commits to the repo. I tried out Keybase Git, which was great.

    ```
    ~
    ❯ run_keybase
    ~
    ❯ keybase git create vm02-real-world-vue
    Repo created! You can clone it with:
      git clone keybase://private/br3ndonland/vm02-real-world-vue
    Or add it as a remote to an existing repo with:
      git remote add origin keybase://private/br3ndonland/vm02-real-world-vue
    ~
    ❯ cd dev/vue-mastery/vm02-real-world-vue
    ~/dev/vue-mastery/vm02-real-world-vue master
    ❯ git remote add origin keybase://private/br3ndonland/vm02-real-world-vue
    ~/dev/vue-mastery/vm02-real-world-vue master*
    ❯ git add --all
    ~/dev/vue-mastery/vm02-real-world-vue master*
    ❯ git commit
    [master 50fce56] Update .eslintrc.js for Prettier plugin
    1 file changed, 6 insertions(+), 2 deletions(-)
    ~/dev/vue-mastery/vm02-real-world-vue master
    ❯ git push --set-upstream origin master
    Initializing Keybase... done.
    Syncing with Keybase... done.
    Syncing encrypted data to Keybase: (0.00%) 0.00/4.97 KB.(100.00%) 4.97/4.97 KB... done.
    Counting objects: 130.54 KB... done.
    Preparing and encrypting objects: (0.07%) 0.09/130.54 KB(100.00%) 130.54/130.54 KB... done.
    Counting refs: 41 bytes... done.
    Preparing and encrypting refs: (100.00%) 41/41 bytes... done.
    To keybase://private/br3ndonland/vm02-real-world-vue
    * [new branch]      master -> master
    Branch 'master' set up to track remote branch 'master' from 'origin'.
    ```

  - I also added the Husky pre-commit hook recommended on the [Prettier homepage](https://prettier.io/).

    ```sh
    ~/dev/vue-mastery/vm02-real-world-vue master* 9s
    ❯ npm install pretty-quick husky --save-dev
    ```

    ```json
    {
      "name": "vm02-real-world-vue",
      "version": "0.1.0",
      "private": true,
      "scripts": {
        "serve": "vue-cli-service serve",
        "build": "vue-cli-service build",
        "lint": "vue-cli-service lint"
      },
      "dependencies": {
        "vue": "^2.6.6",
        "vue-router": "^3.0.1",
        "vuex": "^3.0.1"
      },
      "devDependencies": {
        "@vue/cli-plugin-babel": "^3.5.0",
        "@vue/cli-plugin-eslint": "^3.5.0",
        "@vue/cli-service": "^3.5.0",
        "@vue/eslint-config-prettier": "^4.0.1",
        "babel-eslint": "^10.0.1",
        "eslint": "^5.8.0",
        "eslint-plugin-vue": "^5.0.0",
        "husky": "^1.3.1",
        "pretty-quick": "^1.10.0",
        "vue-template-compiler": "^2.5.21"
      },
      "husky": {
        "hooks": {
          "pre-commit": "pretty-quick --staged"
        }
      }
    }
    ```

    ```
    ~/dev/vue-mastery/vm02-real-world-vue master* 9s
    ❯ git status
    On branch master
    Your branch is up to date with 'origin/master'.

    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

            modified:   package-lock.json
            modified:   package.json

    no changes added to commit (use "git add" and/or "git commit -a")

    ~/dev/vue-mastery/vm02-real-world-vue master*
    ❯ git add --all

    ~/dev/vue-mastery/vm02-real-world-vue master*
    ❯ git commit
    husky > pre-commit (node v11.11.0)
    🔍  Finding changed files since git revision 50fce56.
    🎯  Found 2 changed files.
    ✅  Everything is awesome!
    [master 7b428d3] Add Husky pre-commit hook for Prettier
      2 files changed, 200 insertions(+)
    ~/dev/vue-mastery/vm02-real-world-vue master ⇡ 19s
    ❯ git push
    Initializing Keybase... done.
    Syncing with Keybase... done.
    Preparing and encrypting: (100.00%) 4/4 objects... done.
    Indexing hashes: (100.00%) 4/4 objects... done.
    Indexing CRCs: (100.00%) 4/4 objects... done.
    Indexing offsets: (100.00%) 4/4 objects... done.
    To keybase://private/br3ndonland/vm02-real-world-vue
      50fce56..7b428d3  master -> master
    ```

  - [Prettier CLI](https://prettier.io/docs/en/cli.html)
    - Checking files: `prettier -c "src/**/*.js" "src/**/*.vue"`
    - Fixing files: `prettier --write "src/**/*.js" "src/**/*.vue"`

## 4. Vue Router Basics

### Lesson

- [Lesson link](https://www.vuemastery.com/courses/vm02-real-world-vue-js/vue-router)
- Key concepts

  - **Server-side vs client-side routing**: client-side renders differences without reloading the page

    <img src="img/vue-mastery-02-04-routing.jpg" alt="Server-side routing" width="60%">
    <img src="img/vue-mastery-02-04-routing02.jpg" alt="Client-side routing" width="60%">

  - **Single Page Applications (SPA)**: only one page is loaded, and navigation is done client-side.
  - **Named routes**:

    - Named routes use route names from _router.js_ instead of hardcoding paths. If the URL needs to be changed later, we only need to change _router.js_.
    - _router.js_:

      ```js
      import Vue from "vue"
      import Router from "vue-router"
      import Home from "./views/Home.vue"

      Vue.use(Router)

      export default new Router({
        routes: [
          {
            path: "/",
            name: "home",
            component: Home
          },
          {
            path: "/about",
            name: "about",
            component: About
          }
        ]
      })
      ```

    - _App.vue_:

      Hardcoded:

      ```html
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
      ```

      Named routes:

      ```html
      <router-link :to="{ name: 'home' }">Home</router-link> |
      <router-link :to="{ name: 'about' }">About</router-link>
      ```

  - **Redirect**:

    - If we want to change our about page from _/about_ to _/about-us_, we should use a redirect to point _/about_ to _/about-us_, in case people are already linking to _/about_.
    - _router.js_:

      ```js
      import Vue from "vue"
      import Router from "vue-router"
      import Home from "./views/Home.vue"

      Vue.use(Router)

      export default new Router({
        routes: [
          {
            path: "/",
            name: "home",
            component: Home
          },
          {
            path: "/about-us",
            name: "about",
            component: About
          },
          {
            path: "/about",
            redirect: { name: "about" }
          }
        ]
      })
      ```

  - **Alias**: Adding a duplicate path to the same content with `alias: '/about'`. This is not as common, and having duplicate content is not optimal for SEO.

  - Vue Router practices

    - Vue Router is used for client-side routing.
    - Components are basically pages.
    - When using Vue Router, it's best to put components (pages) for Vue Router in _/views_. Calling it _/pages_ would also be fine.
    - _main.js_ imports _router.js_. `router, store` is the same as saying `router: router, store: store`. This is ES6.

      ```js
      new Vue({
        router,
        store,
        render: h => h(App)
      }).$mount("#app")
      ```

### Installing the event app

- [Vue Mastery Example Event App](https://github.com/Code-Pop/real-world-vue): cloned as `vm02-app`.
- We will remove old components, create 3 main views of the app, and update _router.js_ to use our views.
- I cloned the repo and pushed it to Keybase.

  ```
  ~/dev/vue-mastery/vm02-real-world-vue master
  ❯ keybase git create vm02-app
  Repo created! You can clone it with:
    git clone keybase://private/br3ndonland/vm02-app
  Or add it as a remote to an existing repo with:
    git remote add origin keybase://private/br3ndonland/vm02-app

  ~/dev/vm02-real-world-vue master
  ❯ cd ../

  ~/dev/vue-mastery
  ❯ git clone git@github.com:code-pop/real-world-vue.git vm02-app
  Cloning into 'vm02-app'...
  remote: Enumerating objects: 34, done.
  remote: Counting objects: 100% (34/34), done.
  remote: Compressing objects: 100% (32/32), done.
  remote: Total 633 (delta 14), reused 20 (delta 0), pack-reused 599
  Receiving objects: 100% (633/633), 211.37 KiB | 4.14 MiB/s, done.
  Resolving deltas: 100% (364/364), done.

  ~/dev/vue-mastery
  ❯ cd vm02-app

  ~/dev/vue-mastery/vm02-app master 27s
  ❯ git remote get-url origin
  git@github.com:code-pop/real-world-vue.git

  ~/dev/vue-mastery/vm02-app master
  ❯ git remote set-url origin keybase://private/br3ndonland/vm02-app

  ~/dev/vue-mastery/vm02-app master
  ❯ git push --tags
  ```

- I then installed the app with npm

  ```sh
  ~/dev/vue-mastery/vm02-app master
  ❯ npm i
  ```

- The repo uses Git tags to mark milestones. I hadn't done much with tags before, other than tagging release versions. If pushing the code, it is important to include tags with `git push --tags`.
- We check out to different Git tags in each lesson. To make these notes persistent, I stored them in the `vm02-real-world-vue` repo, and kept `vm02-app` separate.
- I checked out to this lesson's finished code with

  ```sh
  ~/dev/vue-mastery/vm02-app master
  ❯ git checkout lesson4-routing-finish
  ```

  ```text
  Note: checking out 'lesson4-routing-finish'.

  You are in 'detached HEAD' state. You can look around, make experimental
  changes and commit them, and you can discard any commits you make in this
  state without impacting any branches by performing another checkout.

  If you want to create a new branch to retain commits you create, you may
  do so (now or later) by using -b with the checkout command again. Example:

    git checkout -b <new-branch-name>

  HEAD is now at f7cb384 Added routes and basic templates
  ```

### Running the event app

```sh
~/dev/vue-mastery/vm02-app
❯ json-server --watch db.json --port 8081
```

```sh
~/dev/vue-mastery/vm02-app
❯ npm run serve
```

## 5. Dynamic Routing & History Mode

- [Lesson link](https://www.vuemastery.com/courses/vm02-real-world-vue-js/dynamic-routing-history-mode)
- Key concepts

  - **Dynamic routes**:

    - Contain dynamic information, like a username. In this example, `:username` is a **dynamic segment**. The **`$route` object** represents the state of the currently active route. See the [route object API documentation](https://router.vuejs.org/api/#the-route-object).
    - Rather than using the `$route` object directly, we can set `props: true` in _router.js_, then export the `username` prop from _User.vue_. We can then use _User.vue_ as a child component, passing in `username` as a prop.

    _router.js_

    ```js
    ...
    import User from './views/User.vue'
    Vue.use(Router)

    export default new Router({
      routes: [
        ...
        {
          path: '/user/:username',
          name: 'user',
          component: User,
          props: true
        }
      ]
    })
    ```

    _/pages/User.vue_

    ```html
    <template>
      <div class="user">
        <h1>This is a page for {{ username }}</h1>
      </div>
    </template>
    <script>
      export default {
        props: ["username"]
      }
    </script>
    ```

  - **The hash**

    - The hash in the URL, like `localhost:8080/#/about-us`, is the default mode for Vue Router. It uses the URL hash to simulate a full URL, so the page isn't reloaded when the URL changes. We can add configuration to _router.js_ to remove the hash.
    - Enabling **HTML5 history mode** removes the hash. Add `mode: "history"` to _router.js_.
    - In history mode, every URL returns _index.html_. Vue Router knows what's up, and loads the about-us page.
    - See the [Vue Router HTML5 history mode docs](https://router.vuejs.org/guide/essentials/history-mode.html) for example server configurations and rationale.

      > Side Question: You might be wondering, “why isn’t this the default functionality?”
      > Side Answer: the browser `history.pushState` API is only supported in IE10+, while the current version of Vue provides support for IE9+. #BlameIE

  - Handling 404s: add a _/views/FileNotFound.vue_ component that loads if none of the existing paths match.
  - Here's how _router.js_ might look after removing the hash and handling 404s.

    ```js
    export default new Router({
      mode: "history",
      routes: [...{ path: "*", component: NotFoundComponent }]
    })
    ```

- Challenge

  - `git checkout lesson5-dynamic-routing-finish`
    > - Our EventShow.vue has a dynamic segment, /event/:id, using props.
    > - Let’s move our event show link into the EventList, where it will normally live.
    > - Our route.js uses history mode.

## 6. Single File Vue Components

- [Lesson link](https://www.vuemastery.com/courses/vm02-real-world-vue-js/single-file-vue-components)
- Also see [Vue Mastery Intro to Vue.js lesson 08](https://www.vuemastery.com/courses/intro-to-vue-js/components).
- See the [Vue.js Style Guide](https://vuejs.org/v2/style-guide/).
- Key concepts
  - **Vue component anatomy**: three main sections in each _.vue_ file. The `lang` attribute can be used to specify language.
    - `<template>`: structure, or skeleton, of the app
    - `<script>`: brain, or logic, of app.
      - TypeScript can be used with `<script lang="ts">`.
      - `export default { components: {} }` is used to **locally register components.**
    - `<style>`: appearance. Using `<style scoped>` will scope the styles in the component so they only apply to that component.
  - **Nested components**:
    - Nested in parent/child relationship like Russian nesting dolls.
    - In this example, _EventCard.vue_ is nested within _EventList.vue_.
  - **Global vs scoped styles**:
    - Global styles go in _App.vue_, inside the `<style>` tag as one way to do it.
    - Scoped styles go within each component, and apply only to that component.
  - [Vue VSCode snippets extension](https://marketplace.visualstudio.com/items?itemName=sdras.vue-vscode-snippets)
    - `vbase`: scaffolds a base component.
    - `vimport`
    - `vimport-c` to add a `components` section inside the `export` section.
- Challenge

  - `git checkout lesson6-sfc-finish`
    > - Add global app-level styles
    > - Flesh out our EventCard component
    > - Create a NavBar component
  - When attempting to start the webserver with `npm run serve`, I was running into file system watcher errors, like `ENOSPC: System limit for number of file watchers reached.` The solution was to start a new VSCode workspace with a smaller number of files.

## 7. Global Components

- [Lesson link](https://www.vuemastery.com/courses/vm02-real-world-vue-js/global-components)
- Key concepts

  - Local registration: child components are directly imported into parent components. For example, we `export` from _EventCard.vue_, and `import` _EventCard.vue_ into _EventList.vue_. This can get repetitive for commonly used components.
  - **Global registration**:
    - Each component in the application has access to the component.
    - If we wanted to register a single icon component in _main.js_: `Vue.component("BaseIcon", BaseIcon)`.
  - **Automatic global registration**:

    - It's not efficient or scaleable to individually register each component.
    - See the [Vue.js docs on automatic global registration of base components](https://vuejs.org/v2/guide/components-registration.html#Automatic-Global-Registration-of-Base-Components). The docs provide some boilerplate:

      ```js
      import Vue from "vue"
      import upperFirst from "lodash/upperFirst"
      import camelCase from "lodash/camelCase"
      import App from "./App.vue"
      import router from "./router"
      import store from "./store"

      Vue.config.productionTip = false

      const requireComponent = require.context(
        "./components",
        false,
        /Base[A-Z]\w+\.(vue|js)$/
      )

      requireComponent.keys().forEach(fileName => {
        const componentConfig = requireComponent(fileName)

        const componentName = upperFirst(
          camelCase(fileName.replace(/^\.\/(.*)\.\w+$/, "$1"))
        )

        Vue.component(componentName, componentConfig.default || componentConfig)
      })

      new Vue({
        router,
        store,
        render: h => h(App)
      }).$mount("#app")
      ```

    - Note the standardization of case. There are three different types of case described here:
      - **camelCase**
      - **kebab-case**
      - **PascalCase**
      - The [Vue.js style guide](https://vuejs.org/v2/style-guide/) explains that [filenames of single-file components should either be always PascalCase or always kebab-case](https://vuejs.org/v2/style-guide/#Single-file-component-filename-casing-strongly-recommended). PascalCase with each word capitalized is most common.
    - As an example, we use _public/feather-sprite.svg_ in _BaseIcon.vue_. We add a name prop with the `vprop` snippet.

- Challenge
  - `git checkout lesson7-global-finish`

## 8. Slots

- [Lesson link](https://www.vuemastery.com/courses/vm02-real-world-vue-js/slots)
- Key concepts

  - **Slots**
    - Slots allow code to be dynamically templated into components. For example, instead of creating a separate button for each activity, create a generic button and pass the text into it in a slot.
    - Slots can access parent properties like data and computed properties.
    - **Scoped slots**: the template code passed into the slots has access to the child component's data. This is covered in the advanced components course.
  - **Default slot content**
    - Text added inside the `<slot>` tag will be added by default, and replaced by any text added by the code slotted in.
  - **Named slots**

    - Help to pass template from parent to child, much like using `#id`.
    - If there are only two slots, and one is named, the other name can be left out.
    - In this example, we use a media box (like a user card from social media).
    - _MediaBox.vue_ (child)

      ```html
      <slot name="heading"></slot> <slot name="paragraph"></slot>
      ```

    - _Parent.vue_

      ```html
      <template>
        <MediaBox>
          <h2 slot="heading">Adam Jahr</h2>
          <p slot="paragraph">My words.</p>
        </MediaBox>
      </template>
      ```

    - A nested `<template>` can be used to slot-in multiple elements. _Parent.vue_ would look like this instead:

      ```html
      <template>
        <MediaBox>
          <h2 slot="heading">Adam Jahr</h2>
          <template slot="paragraph">
            <p>My words.</p>
            <BaseIcon name="book">
          </template>
        </MediaBox>
      </template>
      ```

- Challenge
  - `git checkout lesson8-slots-finish`

## 9. API calls with Axios

- [Lesson link](https://www.vuemastery.com/courses/vm02-real-world-vue-js/API-calls-with-Axios)
- Git tag for `vm02-app`: `lesson9-axios-start`
- Key concepts

  - **Deploying**
    - Run `npm run build` to build our application into _dist/_. These files would be deployed onto a server.
  - **[Axios](https://github.com/axios/axios)**

    - Commonly used client API library
    - `GET`, `POST`, `PUT`, `DELETE` requests

      - Basic `GET` request

        ```js
        axios
          .get("https://example.com/events")
          .then(response => {
            console.log(response.data)
          })
          .catch(error => {
            console.log(error)
          })
        ```

        Let's refactor that with Async/Await, using the [Axios examples](https://github.com/axios/axios#example) and the [example from Wes Bos](https://gist.github.com/wesbos/1866f918824936ffb73d8fd0b02879b4):

        ```js
        const example = () => {
          try {
            const response = await axios.get("https://example.com/events")
            console.log(response.data)
          } catch (e) {
            throw Error(e)
          }
        }
        ```

        Axios calls are async by default.

    - Add authentication to each request
    - Set timeouts if requests take too long
    - Configure defaults for every request
    - Intercept requests to create middleware
    - Handle errors and cancel requests properly
    - Gregg had all the Axios bullet points on a slide, and said he understood if people didn't like someone reading bullet points to them. I'm used to it, coming from academia, but it can be "a little painful."

  - **API calls**

    - Normally an API would be served by a framework (Django, Express) or a service (Firebase, ~~Graph Cool~~ (now Prisma)).
    - We use the json-server npm package as a mock API to serve _db.json_.

      ```text
      ~/dev/vue-mastery/vm02-app tags/lesson9-axios-finish*
      ❯ npm install -g json-server
      /home/bsmith/.linuxbrew/bin/json-server -> /home/bsmith/.linuxbrew/lib/node_modules/json-server/lib/cli/bin.js
      + json-server@0.14.2
      added 236 packages from 126 contributors in 6.404s
      ~/dev/vue-mastery/vm02-app tags/lesson9-axios-finish*
      ❯ json-server --watch db.json

        \{^_^}/ hi!

        Loading db.json
        Done

        Resources
        http://localhost:3000/events

        Home
        http://localhost:3000

        Type s + enter at any time to create a snapshot of the database
        Watching...
      ```

  - **Component Lifecycle**
    - Didn't get much on lifecycle in this lesson.
  - **Creating services**
    - Separation of concerns: move helper functions to separate JavaScript files
    - Keeps code modular
    - We move the Axios code into _EventService.js_. The core Axios request [creates an instance with `axios.create()`](https://github.com/axios/axios#creating-an-instance).
    - We use a ternary conditional operator in `EventShow.vue` to only display attendees if they are available: `event.attendees ? event.attendees.length : 0`

- Challenge: `git checkout lesson9-axios-finish`

  - Start the application (if you haven't installed yet, run `npm i`)

    ```sh
    ~/dev/vue-mastery/vm02-app tags/lesson9-axios-finish*
    ❯ json-server --watch db.json --port 8081
    ```

    ```sh
    ~/dev/vue-mastery/vm02-app tags/lesson9-axios-finish*
    ❯ npm run serve
    ```

  - Browse to http://localhost:8080 and open dev tools. Note the `[HMR] Waiting for update signal from WDS...` log message in the console, which is just a notice from webpack about Hot Module Replacement.
  - Reconfigure _EventService.js_ to point to port 8081, and refactor with template strings:

    ```js
    import axios from "axios"

    const apiClient = axios.create({
      baseURL: `http://localhost:8081`,
      withCredentials: false, // This is the default
      headers: {
        Accept: "application/json",
        "Content-Type": "application/json"
      }
    })

    export default {
      getEvents() {
        return apiClient.get(`/events`)
      },
      getEvent(id) {
        return apiClient.get(`/events/${id}`)
      }
    }
    ```

  - Refactor _EventList.vue_ with async/await:

    ```js
    import EventCard from "@/components/EventCard.vue"
    import EventService from "@/services/EventService.js"

    export default {
      components: {
        EventCard
      },
      data() {
        return {
          events: []
        }
      },
      async created() {
        try {
          let response = await EventService.getEvents()
          this.events = response.data
        } catch (e) {
          throw Error(e)
        }
      }
    }
    ```

- After refactoring, I created a new Git branch from the `lesson9-axios-finish` tag. Git [re-tagging](https://www.git-scm.com/docs/git-tag#_on_re_tagging) is not recommended.

  ```sh
  ~/dev/vue-mastery/vm02-app tags/lesson9-axios-finish-updated
  ❯ git checkout -b real-world-vue

  ~/dev/vue-mastery/vm02-app real-world-vue
  ❯ git push --set-upstream origin real-world-vue

  ~/dev/vue-mastery/vm02-app real-world-vue
  ❯ git push --tags
  ```

  ```
  Initializing Keybase... done.
  Syncing with Keybase... done.
  Preparing and encrypting: (100.00%) 23/23 objects... done.
  Indexing hashes: (95.65%) 22/23 objects... done.
  Indexing CRCs: (56.52%) 13/23 objects... done.
  Indexing offsets: (95.65%) 22/23 objects... done.
  Syncing encrypted data to Keybase: (100.00%) 63.61/63.61 KB... done.
  Syncing encrypted data to Keybase: (100.00%) 22.72/22.72 KB... done.
  To keybase://private/br3ndonland/vm02-app
  * [new tag]         lesson9-axios-finish-updated -> lesson9-axios-finish-updated
  ```

**COURSE COMPLETE!!! I RULE!!!**

<img src="img/vue-mastery-02-complete.png" alt="Vue Mastery Real-World Vue.js course completion page" width="600px" />
