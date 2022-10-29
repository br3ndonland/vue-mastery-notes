# Build a blog with Nuxt 3 and Nuxt Content 2

[Vue Mastery](https://www.vuemastery.com)

[Build a blog with Nuxt 3 and Nuxt Content 2](https://www.vuemastery.com/courses/build-a-blog-nuxt3-content)

[Code on GitHub](https://github.com/Code-Pop/build-a-blog-with-nuxt-3-and-nuxt-content-v2)

Instructor: Ben Hong

## Table of Contents <!-- omit in toc -->

- [1. Introduction](#1-introduction)
- [2. Project Overview and Setup](#2-project-overview-and-setup)
- [3. Creating Blog Posts with Markdown](#3-creating-blog-posts-with-markdown)
- [4. Using Vue Components in Markdown](#4-using-vue-components-in-markdown)
- [5. Build the Blog Post List Component](#5-build-the-blog-post-list-component)
- [6. Build the Blog Post Detail Page](#6-build-the-blog-post-detail-page)
- [7. Deploying Your Blog](#7-deploying-your-blog)
- [8. Final Thoughts](#8-final-thoughts)

## 1. Introduction

- [Nuxt Content](https://content.nuxtjs.org/) is a Nuxt plugin that reads Markdown, CSV, JSON, and YAML files.
- The [Nuxt 3 `useHead` method](https://v3.nuxtjs.org/api/composables/use-head) allows us to add content to the HTML `<head>`.

## 2. Project Overview and Setup

For this course, we will use the [code on GitHub](https://github.com/Code-Pop/build-a-blog-with-nuxt-3-and-nuxt-content-v2).

For new projects, [get started with Nuxt Content](https://content.nuxtjs.org/get-started) by scaffolding from a template:

```sh
PROJECT_NAME="enter your project name here"
pnpm dlx nuxi init $PROJECT_NAME -t content
cd $PROJECT_NAME && pnpm install --shamefully-hoist
pnpm run dev
```

[Nuxt Content configuration](https://content.nuxtjs.org/api/configuration) is defined in `nuxt.config.ts`. Note that `defineNuxtConfig` is now auto-imported. Remove import or alternatively use `import { defineNuxtConfig } from "nuxt/config"`.

```ts
// nuxt.config.ts: import `from "nuxt/config"`
import { defineNuxtConfig } from "nuxt/config"

export default defineNuxtConfig({
  modules: ["@nuxt/content"],
})
```

```ts
// nuxt.config.ts: auto-import
export default defineNuxtConfig({
  modules: ["@nuxt/content"],
})
```

## 3. Creating Blog Posts with Markdown

## 4. Using Vue Components in Markdown

## 5. Build the Blog Post List Component

## 6. Build the Blog Post Detail Page

## 7. Deploying Your Blog

## 8. Final Thoughts
