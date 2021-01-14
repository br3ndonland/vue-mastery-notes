# Token-Based Authentication

[Vue Mastery](https://www.vuemastery.com)

[Token-Based Authentication](https://www.vuemastery.com/courses/token-based-authentication/intro-to-authentication)

## Table of Contents <!-- omit in toc -->

- [1. Intro to Authentication](#1-intro-to-authentication)
- [2. Project Structure](#2-project-structure)
- [3. User Registration](#3-user-registration)
- [4. User Login](#4-user-login)
- [5. User Logout](#5-user-logout)
- [6. Handling Errors](#6-handling-errors)
- [7. Automatic Login](#7-automatic-login)

## 1. Intro to Authentication

- We will implement authentication using JSON Web Tokens (JWT). See the [JWT docs](https://jwt.io/introduction/).
- A JWT has three parts:
  1. **Header**
  2. **Payload**
  3. **Signature**
- The three parts are separated by dots, like `xxxxx.yyyyy.zzzzz`.

## 2. Project Structure

- This project focuses on the front-end aspects of JWT. For the sake of example, a simple back-end server running on Node.js and Express is provided.
- `npm run start` will start the back-end server and build the front-end Vue.js app.
- The back-end server has four API endpoints:
  1. `/`
  2. `/register`
  3. `/login`
  4. `/dashboard`
- We will make the dashboard a **protected route** that is only visible after login.
- After login, Vuex will:
  1. Store `userData` in state
  2. Store `userData` in local storage
  3. Add token to Axios header to make authenticated API calls to the back-end.
- Logging out will reverse these steps:
  1. Clear `userData` from Vuex state
  2. Clear `userData` from local storage
  3. Remove token from Axios header

## 3. User Registration

- We create a registration component, _RegisterUser.vue_, in which users can register with name, email, and password `<input>` form fields. The inputs are dispatched to Vuex.
- Vuex `POST`s the credentials to the back-end server using the `/register` API endpoint.
- On the back-end, the [Auth0 `jsonwebtoken` package](https://github.com/auth0/node-jsonwebtoken) is used to generate a JWT.

  ```js
  const jwt = require("jsonwebtoken")

  app.post("/register", (req, res) => {
    if (req.body) {
      const user = {
        name: req.body.name,
        email: req.body.email,
        password: req.body.password,
        // You'll want to encrypt the password in a live app
      }

      const data = JSON.stringify(user, null, 2)

      fs.writeFile("db/user.json", data, (err) => {
        if (err) {
          console.log(err)
        } else {
          console.log("Added user to user.json")
        }
      })
      // The secret key should be an environment variable in a live app
      const token = jwt.sign({ user }, "the_secret_key")
      res.json({
        token,
        email: user.email,
        name: user.name,
      })
    } else {
      res.sendStatus(401)
    }
  })
  ```

- Vuex then receives the token from the back-end, stores it in local storage, and sets it as an Axios default header.

  ```js
  import Vue from "vue"
  import Vuex from "vuex"
  import axios from "axios"

  Vue.use(Vuex)

  export default new Vuex.Store({
    state: {
      user: null
    },
    mutations: {
      SET_USER_DATA(state, userData) {
        localStorage.setItem("user", JSON.stringify(userData))
        axios.defaults.headers.common[
          "Authorization"
        ] = `Bearer ${userData.token}`
        state.user = userData
      },
    actions: {
      register({ commit }, credentials) {
        return axios
          .post("//localhost:3000/register", credentials)
          .then(({ data }) => {
            commit("SET_USER_DATA", data)
          })
      },
    }
  })
  ```

- After successful registration, _RegisterUser.vue_ will redirect new users to the dashboard with `this.$router.push({ name: "dashboard" })`.

## 4. User Login

- Add a component for login, _LoginUser.vue_, with router links.
- Handle login with Vuex, in a similar manner to registration.

  ```js
  import Vue from "vue"
  import Vuex from "vuex"
  import axios from "axios"

  Vue.use(Vuex)

  export default new Vuex.Store({
    state: {
      user: null
    },
    mutations: {
      SET_USER_DATA(state, userData) {
        localStorage.setItem("user", JSON.stringify(userData))
        axios.defaults.headers.common[
          "Authorization"
        ] = `Bearer ${userData.token}`
        state.user = userData
      },
    actions: {
      register({ commit }, credentials) {
        return axios
          .post("//localhost:3000/register", credentials)
          .then(({ data }) => {
            commit("SET_USER_DATA", data)
          })
      },
      login({ commit }, credentials) {
        return axios
          .post("//localhost:3000/login", credentials)
          .then(({ data }) => {
            commit("SET_USER_DATA", data)
          })
      },
    }
  })
  ```

- The back-end code for handling login is also quite similar to the code for registration.

  ```js
  app.post("/login", (req, res) => {
    var userDB = fs.readFileSync("./db/user.json")
    var userInfo = JSON.parse(userDB)
    if (
      req.body &&
      req.body.email === userInfo.email &&
      req.body.password === userInfo.password
    ) {
      // The secret key should be an environment variable in a live app
      const token = jwt.sign({ userInfo }, "the_secret_key")
      res.json({
        token,
        email: userInfo.email,
        name: userInfo.name,
      })
    } else {
      res.sendStatus(401)
    }
  })
  ```

## 5. User Logout

- Add a logout button
- Clear user data and force a refresh with the [`location.reload()` web API](https://developer.mozilla.org/en-US/docs/Web/API/Location/reload).

  ```js
  import Vue from "vue"
  import Vuex from "vuex"
  import axios from "axios"

  Vue.use(Vuex)

  export default new Vuex.Store({
    state: {
      user: null
    },
    mutations: {
      SET_USER_DATA(state, userData) {
        localStorage.setItem("user", JSON.stringify(userData))
        axios.defaults.headers.common[
          "Authorization"
        ] = `Bearer ${userData.token}`
        state.user = userData
      },
      LOGOUT() {
        localStorage.removeItem("user")
        location.reload()
    },
    actions: {
      register({ commit }, credentials) {
        return axios
          .post("//localhost:3000/register", credentials)
          .then(({ data }) => {
            commit("SET_USER_DATA", data)
          })
      },
      login({ commit }, credentials) {
        return axios
          .post("//localhost:3000/login", credentials)
          .then(({ data }) => {
            commit("SET_USER_DATA", data)
          })
      },
      logout({ commit }) {
        commit("LOGOUT")
      },
    }
  })
  ```

- Redirect with Vue Router. Initally, the instructor uses [route `Meta` fields](https://router.vuejs.org/guide/advanced/meta.html) to add custom attributes to the routes.

  ```js
  {
    path: "/dashboard",
    name: "dashboard",
    component: Dashboard,
    meta: { requiresAuth: true }
  },
  ```

- Next, they add route guards with `router.beforeEach`.

  ```js
  router.beforeEach((to, from, next) => {
    const loggedIn = localStorage.getItem("user")
    if (to.matched.some((record) => record.meta.requiresAuth)) {
      if (!loggedIn) {
        next("/")
        return
      }
      next()
    }
    next()
  })
  ```

- Another way to do it is to specify the public routes in an array, rather than individually specifying restricted routes. If a user attempts to navigate to a restricted route, they are redirected to the authentication page instead.

  ```js
  router.beforeEach((to, from, next) => {
    const publicPages = ["/authenticate", "/"]
    const authRequired = !publicPages.includes(to.path)
    const loggedIn = localStorage.getItem("user")
    if (authRequired && !loggedIn) {
      return next("/authenticate")
    }
    next()
  })
  ```

- Finally, the dashboard route is only shown if the user is logged in.

  ```html
  <router-link v-if="loggedIn" to="/dashboard"> Dashboard </router-link>
  ```

## 6. Handling Errors

- _What if a user attempts to register with an email that is already registered?_
- _What if a user attempts to register with an invalid password?_
- _What if a user attempts to log in with invalid credentials?_
- These errors are mostly handled by the server. If the server encounters an error, it should return an error code like HTTP `400` Bad request or `401` unauthorized.
- On the front-end, errors from the server can be caught and console logged or displayed in the UI.

## 7. Automatic Login
