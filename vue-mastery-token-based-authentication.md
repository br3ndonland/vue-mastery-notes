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

## 4. User Login

## 5. User Logout

## 6. Handling Errors

## 7. Automatic Login
