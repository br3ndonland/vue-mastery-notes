# Intro to Vue.js

[Vue Mastery](https://www.vuemastery.com)

I really like how they have both videos and written notes. Great to cater to multiple learning styles.

## Table of Contents <!-- omit in toc -->

- [01. The Vue Instance](#01-The-Vue-Instance)
- [02. Attribute Binding](#02-Attribute-Binding)
- [03. Conditional Rendering](#03-Conditional-Rendering)
- [04. List Rendering](#04-List-Rendering)
- [05. Event Handling](#05-Event-Handling)
- [06. Class & Style Binding](#06-Class--Style-Binding)
- [07. Computed Properties](#07-Computed-Properties)
- [08. Components](#08-Components)
- [09. Emitting Events](#09-Emitting-Events)
- [10. Forms](#10-Forms)
- [11. Tabs](#11-Tabs)

## 01. The Vue Instance

- [Lesson link](https://www.vuemastery.com/courses/intro-to-vue-js/vue-instance)
- Setup
  - I installed the Vue dev tools extension for firefox and chrome.
  - Used CodePen for coding
- Key concepts
  - Vue instance is heart of application
  - Vue instance plugs into element in DOM, and the element can use an `{{ expression }}` to display the Vue instance's data.
  - Reactive: the instance's data is linked to every place the data are being referenced. Changing data updates all the corresponding places the data are referenced.
- Challenge

  - Got it right away. Just added a description to the data object, then added an HTML element to reference the data object.

[CodePen solution](https://codepen.io/br3ndonland/pen/BbYxpG)

JavaScript

```js
// Add a description to the data object with the value "A pair of warm, fuzzy socks". Then display the description using an expression in an p element, underneath the h1.
var app = new Vue({
  el: "#app",
  data: {
    product: "Compass",
    description: "A pair of warm, fuzzy socks"
  }
})
```

HTML

```html
<div id="app">
  <div class="product-image">
    <img src="" />
  </div>
  <div class="product-info">
    <h1>{{ product }}</h1>
    <p>{{ description }}</p>
  </div>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.10/vue.min.js"></script>
```

<details><summary>CSS</summary>

```css
html {
  background: #ddd;
}
```

</details>

## 02. Attribute Binding

- [Lesson link](https://www.vuemastery.com/courses/intro-to-vue-js/attribute-binding)
- Key concepts
  - `v-bind` dynamically binds an attribute to an expression. You can just remove the `v-bind` and use a colon.
- Challenge: got it right away.

[CodePen solution](https://codepen.io/br3ndonland/pen/YgevVM)

JavaScript

```js
//Add a link to your data object, and use v-bind to sync it up with an anchor tag in your HTML. Hint: you’ll be binding to the href attribute.
var app = new Vue({
  el: "#app",
  data: {
    product: "Socks",
    image:
      "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg",
    link: "https://brew.sh"
  }
})
```

HTML

```html
<div id="app">
  <div class="product">
    <div class="product-image">
      <img :src="image" />
    </div>
    <div class="product-info">
      <h1>{{ product }}</h1>
      <a :href="link" target="_blank">Homebrew</a>
    </div>
  </div>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.10/vue.min.js"></script>
```

<details><summary>CSS</summary>

```css
body {
  font-family: tahoma;
  color: #282828;
  margin: 0px;
}

.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  height: 60px;
  margin-bottom: 15px;
}

.product {
  display: flex;
}

img {
  border: 1px solid #d8d8d8;
  width: 70%;
  margin: 40px;
  box-shadow: 0px 0.5px 1px #d8d8d8;
}

.product-image {
  flex-basis: 700px;
}

.product-info {
  margin-top: 10px;
  flex-basis: 500px;
}

.color-box {
  width: 40px;
  height: 40px;
  margin-top: 5px;
}

.cart {
  margin-right: 25px;
  float: right;
  border: 1px solid #d8d8d8;
  padding: 5px 20px;
}

button {
  margin-top: 30px;
  border: none;
  background-color: #1e95ea;
  color: white;
  height: 40px;
  width: 100px;
  font-size: 14px;
}

.disabledButton {
  background-color: #d8d8d8;
}

.review-form {
  width: 30%;
  padding: 20px;
  border: 1px solid #d8d8d8;
}

input {
  width: 100%;
  height: 25px;
  margin-bottom: 20px;
}

textarea {
  width: 100%;
  height: 60px;
}
```

</details>

## 03. Conditional Rendering

- [Lesson link](https://www.vuemastery.com/courses/intro-to-vue-js/conditional-rendering)
- Key concepts
  - Using `v-if` to display items only if they are in stock
  - Using `v-show` to toggle visibility
- Challenge: got it right away. Again, very intuitive.

[CodePen solution](https://codepen.io/br3ndonland/pen/pYaZWx)

JavaScript

```js
//Add an onSale property to the data, and use it to conditionally render a span that says “On Sale!”
var app = new Vue({
  el: "#app",
  data: {
    product: "Socks",
    image:
      "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg",
    inStock: true,
    onSale: true
  }
})
```

HTML

```html
<div class="nav-bar"></div>
<div id="app">
  <div class="product">
    <div class="product-image">
      <img :src="image" />
    </div>
    <div class="product-info">
      <h1>{{ product }}</h1>
      <p v-if="inStock">In Stock</p>
      <p v-else>Out of Stock</p>
      <span v-if="onSale">On Sale!</span>
    </div>
  </div>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.10/vue.min.js"></script>
```

<details><summary>CSS</summary>

```css
body {
  font-family: tahoma;
  color: #282828;
  margin: 0px;
}

.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  height: 60px;
  margin-bottom: 15px;
}

.product {
  display: flex;
}

img {
  border: 1px solid #d8d8d8;
  width: 70%;
  margin: 40px;
  box-shadow: 0px 0.5px 1px #d8d8d8;
}

.product-image {
  flex-basis: 700px;
}

.product-info {
  margin-top: 10px;
  flex-basis: 500px;
}

.color-box {
  width: 40px;
  height: 40px;
  margin-top: 5px;
}

.cart {
  margin-right: 25px;
  float: right;
  border: 1px solid #d8d8d8;
  padding: 5px 20px;
}

button {
  margin-top: 30px;
  border: none;
  background-color: #1e95ea;
  color: white;
  height: 40px;
  width: 100px;
  font-size: 14px;
}

.disabledButton {
  background-color: #d8d8d8;
}

.review-form {
  width: 30%;
  padding: 20px;
  border: 1px solid #d8d8d8;
}

input {
  width: 100%;
  height: 25px;
  margin-bottom: 20px;
}

textarea {
  width: 100%;
  height: 60px;
}
```

</details>

## 04. List Rendering

- [Lesson link](https://www.vuemastery.com/courses/intro-to-vue-js/list-rendering)
- Key concepts
  - Looping over an array with `v-for`.
  - Looping over an array of objects with `v-for="variant in variants"`, and including `:key="variant.variantId"`.
- Challenge: got it right away.

[CodePen solution](https://codepen.io/br3ndonland/pen/drdqMz)

JavaScript

```js
// Add an array of sizes to the data and use v-for to display them in a list.
var app = new Vue({
  el: "#app",
  data: {
    product: "Socks",
    image:
      "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg",
    inStock: true,
    details: ["80% cotton", "20% polyester", "Gender-neutral"],
    sizes: ["small", "medium", "large"],
    variants: [
      {
        variantId: 2234,
        variantColor: "green"
      },
      {
        variantId: 2235,
        variantColor: "blue"
      }
    ]
  }
})
```

HTML

```html
<div class="nav-bar"></div>
<div id="app">
  <div class="product">
    <div class="product-image">
      <img :src="image" />
    </div>
    <div class="product-info">
      <h1>{{ product }}</h1>
      <p v-if="inStock">In Stock</p>
      <p v-else>Out of Stock</p>
      <ul>
        <li v-for="detail in details">{{ detail }}</li>
      </ul>
      <ul>
        Sizes
        <li v-for="size in sizes">{{ size }}</li>
      </ul>
      <div v-for="variant in variants" :key="variant.variantId">
        <p>{{ variant.variantColor }}</p>
      </div>
    </div>
  </div>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.10/vue.min.js"></script>
```

<details><summary>CSS</summary>

```css
body {
  font-family: tahoma;
  color: #282828;
  margin: 0px;
}

.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  height: 60px;
  margin-bottom: 15px;
}

.product {
  display: flex;
}

img {
  border: 1px solid #d8d8d8;
  width: 70%;
  margin: 40px;
  box-shadow: 0px 0.5px 1px #d8d8d8;
}

.product-image {
  flex-basis: 700px;
}

.product-info {
  margin-top: 10px;
  flex-basis: 500px;
}

.color-box {
  width: 40px;
  height: 40px;
  margin-top: 5px;
}

.cart {
  margin-right: 25px;
  float: right;
  border: 1px solid #d8d8d8;
  padding: 5px 20px;
}

button {
  margin-top: 30px;
  border: none;
  background-color: #1e95ea;
  color: white;
  height: 40px;
  width: 100px;
  font-size: 14px;
}

.disabledButton {
  background-color: #d8d8d8;
}

.review-form {
  width: 30%;
  padding: 20px;
  border: 1px solid #d8d8d8;
}

input {
  width: 100%;
  height: 25px;
  margin-bottom: 20px;
}

textarea {
  width: 100%;
  height: 60px;
}
```

</details>

## 05. Event Handling

- [Lesson link](https://www.vuemastery.com/courses/intro-to-vue-js/event-handling)
- Key concepts
  - Each Vue instance can have a `methods` property.
  - Event handling with `v-on`, such as `v-on:click`. Really nice after all the time I spent manually setting this stuff up for [udacity-fsnd-p5-map](https://github.com/br3ndonland/udacity-fsnd-p5-map) and [udacity-google-mws](https://github.com/br3ndonland/udacity-google-mws).
  - The shortcut `@` can replace `v-on`, such as `@mouseover` instead of `v-on:mouseover`.
    - `@click`
    - `@mouseover`
    - `@submit`
    - `@keyup.enter`: `enter` is a modifier that specifies the keyboard event.
  - When `v-on` detects the click event, it can trigger a function, such as `v-on:click="addToCart"` in this example.
- Challenge: rocked it.

[CodePen solution](https://codepen.io/br3ndonland/pen/GeQwpj)

JavaScript

```js
//Create a new button and method to decrement the value of `cart`.
var app = new Vue({
  el: "#app",
  data: {
    product: "Socks",
    image:
      "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg",
    inStock: true,
    details: ["80% cotton", "20% polyester", "Gender-neutral"],
    variants: [
      {
        variantId: 2234,
        variantColor: "green",
        variantImage:
          "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg"
      },
      {
        variantId: 2235,
        variantColor: "blue",
        variantImage:
          "https://www.vuemastery.com/images/challenges/vmSocks-blue-onWhite.jpg"
      }
    ],
    cart: 0
  },
  methods: {
    addToCart() {
      this.cart += 1
    },
    removeFromCart() {
      this.cart -= 1
    },
    updateProduct(variantImage) {
      this.image = variantImage
    }
  }
})
```

HTML

```html
<div class="nav-bar"></div>
<div id="app">
  <div class="product">
    <div class="product-image">
      <img :src="image" />
    </div>
    <div class="product-info">
      <h1>{{ product }}</h1>
      <p v-if="inStock">In Stock</p>
      <p v-else>Out of Stock</p>
      <ul>
        <li v-for="detail in details">{{ detail }}</li>
      </ul>
      <div v-for="variant in variants" :key="variant.variantId">
        <p @mouseover="updateProduct(variant.variantImage)">
          {{ variant.variantColor }}
        </p>
      </div>
      <button @click="addToCart">Add to cart</button>
      <button @click="removeFromCart">Remove from cart</button>
      <div class="cart">
        <p>Cart({{ cart }})</p>
      </div>
    </div>
  </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.10/vue.min.js"></script>
```

<details><summary>CSS</summary>

```css
body {
  font-family: tahoma;
  color: #282828;
  margin: 0px;
}

.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  height: 60px;
  margin-bottom: 15px;
}

.product {
  display: flex;
}

img {
  border: 1px solid #d8d8d8;
  width: 70%;
  margin: 40px;
  box-shadow: 0px 0.5px 1px #d8d8d8;
}

.product-image {
  flex-basis: 700px;
}

.product-info {
  margin-top: 10px;
  flex-basis: 500px;
}

.color-box {
  width: 40px;
  height: 40px;
  margin-top: 5px;
}

.cart {
  margin-right: 25px;
  float: right;
  border: 1px solid #d8d8d8;
  padding: 5px 20px;
}

button {
  margin-top: 30px;
  border: none;
  background-color: #1e95ea;
  color: white;
  height: 40px;
  width: 100px;
  font-size: 14px;
}

.disabledButton {
  background-color: #d8d8d8;
}

.review-form {
  width: 30%;
  padding: 20px;
  border: 1px solid #d8d8d8;
}

input {
  width: 100%;
  height: 25px;
  margin-bottom: 20px;
}

textarea {
  width: 100%;
  height: 60px;
}
```

</details>

## 06. Class & Style Binding

- [Lesson link](https://www.vuemastery.com/courses/intro-to-vue-js/class-&-style-binding)
- Key concepts

  - Setting variant properties of an item with arrays in the Vue app.
  - Updating color on mouseover
  - Class binding: disabling a button when product is out of stock, using `v-bind:` or the `:` shorthand, and by adding the `disabledButton` CSS class.

    ```html
    <button
      v-on:click="addToCart"
      :disabled="!inStock"
      :class="{ disabledButton: !inStock }"
    >
      Add to cart
    </button>
    ```

- Challenge:

  - Didn't quite get it at first. I just added a line to the CSS.

    ```css
    .disabledButton {
      background-color: #d8d8d8;
      text-decoration: line-through;
    }
    ```

  - I actually needed to strikeout the text above, not the button. This required modifying the `<p>` tag inside `<div class="product-info">`, then creating a CSS class to strikeout the text.

    ```html
    <p v-if="inStock">In Stock</p>
    <p v-else :class="{ outOfStock: !inStock }">Out of Stock</p>
    ```

    ```css
    .outOfStock {
      text-decoration: line-through;
    }
    ```

Full code:

[CodePen solution](https://codepen.io/br3ndonland/pen/rRdJPd)

JavaScript

```js
//When inStock is false, bind a class to the “Out of Stock” p tag that adds  text-decoration: line-through to that element.
var app = new Vue({
  el: "#app",
  data: {
    product: "Socks",
    image:
      "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg",
    inStock: false,
    details: ["80% cotton", "20% polyester", "Gender-neutral"],
    variants: [
      {
        variantId: 2234,
        variantColor: "green",
        variantImage:
          "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg"
      },
      {
        variantId: 2235,
        variantColor: "blue",
        variantImage:
          "https://www.vuemastery.com/images/challenges/vmSocks-blue-onWhite.jpg"
      }
    ],
    cart: 0
  },
  methods: {
    addToCart() {
      this.cart += 1
    },
    updateProduct(variantImage) {
      this.image = variantImage
    }
  }
})
```

HTML

```html
<div class="nav-bar"></div>
<div id="app">
  <div class="product">
    <div class="product-image">
      <img :src="image" />
    </div>
    <div class="product-info">
      <h1>{{ product }}</h1>
      <p v-if="inStock">In Stock</p>
      <p v-else :class="{ outOfStock: !inStock }">Out of Stock</p>
      <ul>
        <li v-for="detail in details">{{ detail }}</li>
      </ul>
      <div
        class="color-box"
        v-for="variant in variants"
        :key="variant.variantId"
        :style="{ backgroundColor: variant.variantColor }"
        @mouseover="updateProduct(variant.variantImage)"
      ></div>
      <button
        v-on:click="addToCart"
        :disabled="!inStock"
        :class="{ disabledButton: !inStock }"
      >
        Add to cart
      </button>
      <div class="cart">
        <p>Cart({{ cart }})</p>
      </div>
    </div>
  </div>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.10/vue.min.js"></script>
```

<details><summary>CSS</summary>

```css
body {
  font-family: tahoma;
  color: #282828;
  margin: 0px;
}

.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  height: 60px;
  margin-bottom: 15px;
}

.product {
  display: flex;
}

img {
  border: 1px solid #d8d8d8;
  width: 70%;
  margin: 40px;
  box-shadow: 0px 0.5px 1px #d8d8d8;
}

.product-image {
  flex-basis: 700px;
}

.product-info {
  margin-top: 10px;
  flex-basis: 500px;
}

.color-box {
  width: 40px;
  height: 40px;
  margin-top: 5px;
}

.cart {
  margin-right: 25px;
  float: right;
  border: 1px solid #d8d8d8;
  padding: 5px 20px;
}

button {
  margin-top: 30px;
  border: none;
  background-color: #1e95ea;
  color: white;
  height: 40px;
  width: 100px;
  font-size: 14px;
}

.disabledButton {
  background-color: #d8d8d8;
  text-decoration: line-through;
}

.review-form {
  width: 30%;
  padding: 20px;
  border: 1px solid #d8d8d8;
}

input {
  width: 100%;
  height: 25px;
  margin-bottom: 20px;
}

textarea {
  width: 100%;
  height: 60px;
}

.outOfStock {
  text-decoration: line-through;
}
```

</details>

## 07. Computed Properties

- [Lesson link](https://www.vuemastery.com/courses/intro-to-vue-js/computed-properties)
- Key concepts

  - Displaying multiple pieces of data, like `brand` and `product`, in one string. Rather than having two expressions, like `{{ brand }} {{ product }}`, we create a computed property and call it using an expression with the name of that property, like `{{ title }}`. It's like a calculator.
  - **Computed properties are cached.** The result is saved until its dependencies change. This makes computed properties more computationally efficient than methods, because methods need to be entirely re-run.
  - A computed property to update a product image based on the color that the mouse is hovering over would look something like this:

    ```js
    computed: {
      title() {
        return `${this.brand} ${this.product}`
      },
      image() {
        return this.variants[this.selectedVariant].variantImage
      },
      inStock() {
        return this.variants[this.selectedVariant].variantQuantity
      }
    }
    ```

- Challenge

  - Add a boolean data property `onSale`.
  - Create a computed property that prints out `brand` and `product` whenever `onSale` is true.
  - This challenge took me about 10 minutes, but I got it on my own, and almost exactly the same as the instructor's solution!

[CodePen solution](https://codepen.io/br3ndonland/pen/pYLVMj)

JavaScript

```js
//Add a new boolean data property `onSale` and create a computed property that takes `brand`, `product` and `onSale` and prints out a string whenever `onSale` is true.
var app = new Vue({
  el: "#app",
  data: {
    product: "Socks",
    brand: "Vue Factery",
    selectedVariant: 0,
    details: ["80% cotton", "20% polyester", "Gender-neutral"],
    variants: [
      {
        variantId: 2234,
        variantColor: "green",
        variantImage:
          "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg",
        variantQuantity: 10
      },
      {
        variantId: 2235,
        variantColor: "blue",
        variantImage:
          "https://www.vuemastery.com/images/challenges/vmSocks-blue-onWhite.jpg",
        variantQuantity: 0
      }
    ],
    cart: 0,
    onSale: true
  },
  methods: {
    addToCart() {
      this.cart += 1
    },
    updateProduct(index) {
      this.selectedVariant = index
      console.log(index)
    }
  },
  computed: {
    title() {
      return `${this.brand} ${this.product}`
    },
    image() {
      return this.variants[this.selectedVariant].variantImage
    },
    inStock() {
      return this.variants[this.selectedVariant].variantQuantity
    },
    saleInfo() {
      return `This item, ${this.brand} ${this.product}, is on sale!!!`
    }
  }
})
```

HTML

```html
<div class="nav-bar"></div>
<div id="app">
  <div class="product">
    <div class="product-image">
      <img :src="image" />
    </div>
    <div class="product-info">
      <h1>{{ product }}</h1>
      <p v-if="onSale">{{ saleInfo }}</p>
      <p v-if="inStock">In Stock</p>
      <p v-else>Out of Stock</p>
      <ul>
        <li v-for="detail in details">{{ detail }}</li>
      </ul>
      <div
        class="color-box"
        v-for="(variant, index) in variants"
        :key="variant.variantId"
        :style="{ backgroundColor: variant.variantColor }"
        @mouseover="updateProduct(index)"
      ></div>
      <button
        v-on:click="addToCart"
        :disabled="!inStock"
        :class="{ disabledButton: !inStock }"
      >
        Add to cart
      </button>
      <div class="cart">
        <p>Cart({{ cart }})</p>
      </div>
    </div>
  </div>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.10/vue.min.js"></script>
```

<details><summary>CSS</summary>

```css
body {
  font-family: tahoma;
  color: #282828;
  margin: 0px;
}

.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  height: 60px;
  margin-bottom: 15px;
}

.product {
  display: flex;
}

img {
  border: 1px solid #d8d8d8;
  width: 70%;
  margin: 40px;
  box-shadow: 0px 0.5px 1px #d8d8d8;
}

.product-image {
  flex-basis: 700px;
}

.product-info {
  margin-top: 10px;
  flex-basis: 500px;
}

.color-box {
  width: 40px;
  height: 40px;
  margin-top: 5px;
}

.cart {
  margin-right: 25px;
  float: right;
  border: 1px solid #d8d8d8;
  padding: 5px 20px;
}

button {
  margin-top: 30px;
  border: none;
  background-color: #1e95ea;
  color: white;
  height: 40px;
  width: 100px;
  font-size: 14px;
}

.disabledButton {
  background-color: #d8d8d8;
}

.review-form {
  width: 30%;
  padding: 20px;
  border: 1px solid #d8d8d8;
}

input {
  width: 100%;
  height: 25px;
  margin-bottom: 20px;
}

textarea {
  width: 100%;
  height: 60px;
}
```

</details>

## 08. Components

- [Lesson link](https://www.vuemastery.com/courses/intro-to-vue-js/components)
- Key concepts

  - **Components** are reusable blocks of code used to create a modular, maintainable codebase.
  - Components can be nested (components within components). For example, an item details component might have a "Buy it now" button component and an "Add to cart" component nested within it.
  - The `template` section sets up the HTML structure of the component. There are multiple ways to construct a template. Here, the template is inserted as a template literal within backticks.
  - Components are scoped, and can't access data outside the component. **Props** are used to pass data from parent to child components.
  - Building a **single-file component** (see Vue Mastery cheat sheet, component anatomy section):

    - Name
    - Options object `{}`:
      - Props
      - Template `div`
      - `data()` function (`data()` is a function, not an object, so that it can be different for each instance of the component)

    ```js
    Vue.component("product", {
      props: {
        message: {
          type: String,
          required: true,
          default: "Hi"
        }
      },
      template: `<div>{{ message }}</div>`,
      data() {
        return {
          // Data to return
        }
      },
      methods: {},
      computed: {}
    })
    ```

  - In addition to the `data() {}` function, components can have `methods: {}` and `computed: {}`.

- Challenge:

  - I didn't quite get this one at first. The directions were less clear than the other challenges, and it was Monday morning.
  - I guess I wasn't sure how much they wanted me to add to the prop.
  - I was also thinking they were going to have me pass `details` from the `product` component as a prop to the `product-details` component, but it doesn't look like they wanted us to go that far. It's more about waiting for data when writing child components. As the notes say:

    > In Vue, we use props to handle this kind of downward data sharing. Props are essentially variables that are waiting to be filled with the data its parent sends down into it.

  - The rest was very logical. We will have an array of details, so it's set to `type: Array`. We then use `v-for` to iterate over the array and display details.

[CodePen solution](https://codepen.io/br3ndonland/pen/XGYxYp)

JavaScript

```js
// Create a new component for product-details with a prop of details.
Vue.component("product-details", {
  props: {
    details: {
      type: Array,
      required: true
    }
  },
  template: `
    <ul>
      <li v-for="detail in details">{{ detail }}</li>
    </ul>
  `
})
Vue.component("product", {
  props: {
    premium: {
      type: Boolean,
      required: true
    }
  },
  template: `
  <div class="product">
    <div class="product-image">
      <img :src="image" />
    </div>
      <div class="product-info">
        <h1>{{ product }}</h1>
        <p v-if="inStock">In Stock</p>
        <p v-else>Out of Stock</p>
        <p>Shipping: {{ shipping }}</p>
        <ul>
          <li v-for="detail in details">{{ detail }}</li>
        </ul>
        <div class="color-box"
          v-for="(variant, index) in variants"
          :key="variant.variantId"
          :style="{ backgroundColor: variant.variantColor }"
          @mouseover="updateProduct(index)"
          >
        </div>
        <button v-on:click="addToCart"
          :disabled="!inStock"
          :class="{ disabledButton: !inStock }"
          >
        Add to cart
        </button>
        <div class="cart">
          <p>Cart({{ cart }})</p>
        </div>
      </div>
    </div>
  `,
  data() {
    return {
      product: "Socks",
      brand: "Vue Mastery",
      selectedVariant: 0,
      details: ["80% cotton", "20% polyester", "Gender-neutral"],
      variants: [
        {
          variantId: 2234,
          variantColor: "green",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg",
          variantQuantity: 10
        },
        {
          variantId: 2235,
          variantColor: "blue",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-blue-onWhite.jpg",
          variantQuantity: 0
        }
      ],
      cart: 0
    }
  },
  methods: {
    addToCart: function() {
      this.cart += 1
    },
    updateProduct: function(index) {
      this.selectedVariant = index
    }
  },
  computed: {
    title() {
      return `${this.brand} ${this.product}`
    },
    image() {
      return this.variants[this.selectedVariant].variantImage
    },
    inStock() {
      return this.variants[this.selectedVariant].variantQuantity
    },
    shipping() {
      if (this.premium) {
        return "Free"
      }
      return 2.99
    }
  }
})
var app = new Vue({
  el: "#app",
  data: {
    premium: true
  }
})
```

HTML

```html
<div class="nav-bar"></div>
<div id="app">
  <product :premium="premium"></product>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.10/vue.min.js"></script>
```

<details><summary>CSS</summary>

```css
body {
  font-family: tahoma;
  color: #282828;
  margin: 0px;
}

.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  height: 60px;
  margin-bottom: 15px;
}

.product {
  display: flex;
}

img {
  border: 1px solid #d8d8d8;
  width: 70%;
  margin: 40px;
  box-shadow: 0px 0.5px 1px #d8d8d8;
}

.product-image {
  flex-basis: 700px;
}

.product-info {
  margin-top: 10px;
  flex-basis: 500px;
}

.color-box {
  width: 40px;
  height: 40px;
  margin-top: 5px;
}

.cart {
  margin-right: 25px;
  float: right;
  border: 1px solid #d8d8d8;
  padding: 5px 20px;
}

button {
  margin-top: 30px;
  border: none;
  background-color: #1e95ea;
  color: white;
  height: 40px;
  width: 100px;
  font-size: 14px;
}

.disabledButton {
  background-color: #d8d8d8;
}

.review-form {
  width: 30%;
  padding: 20px;
  border: 1px solid #d8d8d8;
}

input {
  width: 100%;
  height: 25px;
  margin-bottom: 20px;
}

textarea {
  width: 100%;
  height: 60px;
}
```

</details>

## 09. Emitting Events

- [Lesson link](https://www.vuemastery.com/courses/intro-to-vue-js/communicating-events)
- Key concepts
  - In addition to passing data from parent to child components via props, we can also pass data back to parent components with emitters.
  - In this example, we move the `cart` to the root `app` instance, and pass `product` data up to it.
  - We use `this.$emit("title", options)` to pass data to parent elements, letting the parent know when the event has occurred.
  - We also change `cart` to an array, which is obviously more useful and practical.
  - We're using `v-on`/`@` event handling (see [05. Event Handling](#05-event-handling)) to listen for an event emission, and `v-bind` class binding to trigger methods on the parent elements (see [06. Class & Style Binding](#06-class--style-binding)).
- Challenge

  - After walking through the lesson, we now have an `addToCart()` method in the `product` component.
  - We need to add a `removeFromCart()` method, and a button that triggers the method.
  - I added the button to the `template` of the `product` component.
  - As with the "Add to cart" button, the "Remove from cart" button needs to trigger one of the `product` component's methods, which I named `removeProductFromCart` for clarity.
  - The `removeProductFromCart` method emits `remove-from-cart`, like

    ```js
    this.$emit(
      "remove-from-cart",
      this.variants[this.selectedVariant].variantId
    )
    ```

  - The `add-to-cart` and `remove-from-cart` events are event listeners on the `<product>` div in the HTML, and trigger functions in the parent `app`. I created a new function, and decided to try to `pop` the item out of the `cart: []` array. JavaScript array methods are really useful.

    ```js
    removeFromCart(id) { this.cart.pop(id) }
    ```

  - I tried it, and... **SUCCESS!!!** Yes!!! This challenge took me close to an hour, but I got it on my own!

[CodePen solution](https://codepen.io/br3ndonland/pen/gEKqWj)

JavaScript

```js
// Add a button that removes the product from the cart array by emitting an event with the id of the product to be removed.
Vue.component("product", {
  props: {
    premium: {
      type: Boolean,
      required: true
    }
  },
  template: `
    <div class="product">
      <div class="product-image">
        <img :src="image" />
      </div>
      <div class="product-info">
        <h1>{{ product }}</h1>
        <p v-if="inStock">In Stock</p>
        <p v-else>Out of Stock</p>
        <p>Shipping: {{ shipping }}</p>
        <ul>
          <li v-for="detail in details">{{ detail }}</li>
        </ul>
        <div class="color-box"
          v-for="(variant, index) in variants"
          :key="variant.variantId"
          :style="{ backgroundColor: variant.variantColor }"
          @mouseover="updateProduct(index)"
          >
        </div>
        <button v-on:click="addToCart"
          :disabled="!inStock"
          :class="{ disabledButton: !inStock }"
          >
        Add to cart
        </button>
        <button @click="removeProductFromCart"
        >
        Remove from cart
        </button>
      </div>
    </div>
  `,
  data() {
    return {
      product: "Socks",
      brand: "Vue Mastery",
      selectedVariant: 0,
      details: ["80% cotton", "20% polyester", "Gender-neutral"],
      variants: [
        {
          variantId: 2234,
          variantColor: "green",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg",
          variantQuantity: 10
        },
        {
          variantId: 2235,
          variantColor: "blue",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-blue-onWhite.jpg",
          variantQuantity: 0
        }
      ]
    }
  },
  methods: {
    addToCart() {
      this.$emit("add-to-cart", this.variants[this.selectedVariant].variantId)
    },
    removeProductFromCart() {
      this.$emit(
        "remove-from-cart",
        this.variants[this.selectedVariant].variantId
      )
    },
    updateProduct(index) {
      this.selectedVariant = index
    }
  },
  computed: {
    title() {
      return `${this.brand} ${this.product}`
    },
    image() {
      return this.variants[this.selectedVariant].variantImage
    },
    inStock() {
      return this.variants[this.selectedVariant].variantQuantity
    },
    shipping() {
      if (this.premium) {
        return "Free"
      }
      return 2.99
    }
  }
})
var app = new Vue({
  el: "#app",
  data: {
    premium: true,
    cart: []
  },
  methods: {
    updateCart(id) {
      this.cart.push(id)
    },
    removeFromCart(id) {
      this.cart.pop(id)
    }
  }
})
```

HTML

```html
<div class="nav-bar"></div>
<div id="app">
  <div class="cart">
    <p>Cart({{ cart.length }})</p>
  </div>
  <product
    :premium="premium"
    @add-to-cart="updateCart"
    @remove-from-cart="removeFromCart"
  ></product>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.10/vue.min.js"></script>
```

<details><summary>CSS</summary>

```css
body {
  font-family: tahoma;
  color: #282828;
  margin: 0px;
}

.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  height: 60px;
  margin-bottom: 15px;
}

.product {
  display: flex;
}

img {
  border: 1px solid #d8d8d8;
  width: 70%;
  margin: 40px;
  box-shadow: 0px 0.5px 1px #d8d8d8;
}

.product-image {
  flex-basis: 700px;
}

.product-info {
  margin-top: 10px;
  flex-basis: 500px;
}

.color-box {
  width: 40px;
  height: 40px;
  margin-top: 5px;
}

.cart {
  margin-right: 25px;
  float: right;
  border: 1px solid #d8d8d8;
  padding: 5px 20px;
}

button {
  margin-top: 30px;
  border: none;
  background-color: #1e95ea;
  color: white;
  height: 40px;
  width: 100px;
  font-size: 14px;
}

.disabledButton {
  background-color: #d8d8d8;
}

.review-form {
  width: 30%;
  padding: 20px;
  border: 1px solid #d8d8d8;
}

input {
  width: 100%;
  height: 25px;
  margin-bottom: 20px;
}

textarea {
  width: 100%;
  height: 60px;
}
```

</details>

## 10. Forms

- [Lesson link](https://www.vuemastery.com/courses/intro-to-vue-js/forms/)
- Key concepts

  - We create a new `Vue.component('product-review', {})`, as a child of the `product` component.
  - We use `v-model` to create two-way binding between user input and Vue.js.
    - `template`: we update the component's `template` with a `<form class="review-form" @submit.prevent="onSubmit">` to accept user input. Each `input` field is bound to Vue.js with `v-model`, like `<input id="name" v-model="name">`.
  - `v-model` modifiers:
    - `.number` modifier to increment (there is a bug with it)
    - `.prevent` modifier to prevent page reload when form is submitted.
  - We update the `product-review` component's methods with `methods: { onSubmit() {} }` to configure form submission, so that the form submits data to the `product` component. We include an emitter with `this.$emit('review-submitted', productReview)` to send the data up to the `product` component.
  - We display the reviews with the `<product-review>` div, which holds content for the `product-review` component, and is nested within the template of the `product` component. We listen for the `review-submitted` event to be emitted from the child `product-review` component, and trigger a new `addReview(productReview)` method.

    ```html
    <product-review @review-submitted="addReview"></product-review>
    ```

  - We add the `addReview(productReview)` method to the `product` component.

    ```js
    methods: {
      addReview(productReview) {
        this.reviews.push(productReview)
      }
    }
    ```

  - Form validation: adding some simple form validation, checking if all fields are filled in, and then adding an error div to the template of the `product-review` component to display any errors.

- Challenge

  - Add a question to the form, "Would you recommend this product?": `<p>Would you recommend this product?</p>`
  - Add "Yes" and "No" radio buttons:
    - I initially had `<button type="radio">Yes</button><button type="radio">No</button>`, but realized it might be difficult to store the response, because clicking the radio button triggers an event. I could set up a separate event for that, but easier to just have a `<label>` element for each choice as the instructor does.
  - Add response to the `productReview` object
  - Validate form
    - HTML5 `<form required>` can be used.
    - Here, we build in some custom form validation. Add error handling checks for `this.recommend` to `methods: { onSubmit() {} }` in the `product-review` component.

[CodePen solution](https://codepen.io/br3ndonland/pen/VRdObr)

JavaScript

```js
// Add a question to the form: “Would you recommend this product”. Then take in that response from the user via radio buttons of “yes” or “no” and add it to the productReview object, with form validation.
Vue.component("product", {
  props: {
    premium: {
      type: Boolean,
      required: true
    }
  },
  template: `
    <div class="product">
      <div class="product-image">
        <img :src="image" />
      </div>
      <div class="product-info">
        <h1>{{ product }}</h1>
        <p v-if="inStock">In Stock</p>
        <p v-else>Out of Stock</p>
        <p>Shipping: {{ shipping }}</p>
        <ul>
          <li v-for="detail in details">{{ detail }}</li>
        </ul>
        <div class="color-box"
          v-for="(variant, index) in variants"
          :key="variant.variantId"
          :style="{ backgroundColor: variant.variantColor }"
          @mouseover="updateProduct(index)"
          >
        </div>
        <button v-on:click="addToCart"
          :disabled="!inStock"
          :class="{ disabledButton: !inStock }"
          >
        Add to cart
        </button>
      </div>
      <div>
        <h2>Reviews</h2>
        <p v-if="!reviews.length">There are no reviews yet.</p>
        <ul v-else>
          <li v-for="(review, index) in reviews" :key="index">
            <p>{{ review.name }}</p>
            <p>Rating:{{ review.rating }}</p>
            <p>{{ review.review }}</p>
          </li>
        </ul>
      </div>
        <product-review @review-submitted="addReview"></product-review>
    </div>
  `,
  data() {
    return {
      product: "Socks",
      brand: "Vue Mastery",
      selectedVariant: 0,
      details: ["80% cotton", "20% polyester", "Gender-neutral"],
      variants: [
        {
          variantId: 2234,
          variantColor: "green",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg",
          variantQuantity: 10
        },
        {
          variantId: 2235,
          variantColor: "blue",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-blue-onWhite.jpg",
          variantQuantity: 0
        }
      ],
      reviews: []
    }
  },
  methods: {
    addToCart() {
      this.$emit("add-to-cart", this.variants[this.selectedVariant].variantId)
    },
    updateProduct(index) {
      this.selectedVariant = index
    },
    addReview(productReview) {
      this.reviews.push(productReview)
    }
  },
  computed: {
    title() {
      return `${this.brand} ${this.product}`
    },
    image() {
      return this.variants[this.selectedVariant].variantImage
    },
    inStock() {
      return this.variants[this.selectedVariant].variantQuantity
    },
    shipping() {
      if (this.premium) {
        return "Free"
      }
      return 2.99
    }
  }
})
Vue.component("product-review", {
  template: `
    <form class="review-form" @submit.prevent="onSubmit">
      <p class="error" v-if="errors.length">
        <b>Please correct the following error(s):</b>
        <ul>
          <li v-for="error in errors">{{ error }}</li>
        </ul>
      </p>
      <p>
        <label for="name">Name:</label>
        <input id="name" v-model="name">
      </p>
      <p>
        <label for="review">Review:</label>
        <textarea id="review" v-model="review"></textarea>
      </p>
      <p>
        <label for="rating">Rating:</label>
        <select id="rating" v-model.number="rating">
          <option>5</option>
          <option>4</option>
          <option>3</option>
          <option>2</option>
          <option>1</option>
        </select>
      </p>
      <p>Would you recommend this product?</p>
      <label>
        Yes
        <input type="radio" value="Yes" v-model="recommend"/>
      </label>
      <label>
        No
        <input type="radio" value="No" v-model="recommend"/>
      </label>
      <p>
        <input type="submit" value="Submit">
      </p>
    </form>
  `,
  data() {
    return {
      name: null,
      review: null,
      rating: null,
      recommend: null,
      errors: []
    }
  },
  methods: {
    onSubmit() {
      this.errors = []
      if (this.name && this.review && this.rating && this.recommend) {
        let productReview = {
          name: this.name,
          review: this.review,
          rating: this.rating,
          recommend: this.recommend
        }
        this.$emit("review-submitted", productReview)
        this.name = null
        this.review = null
        this.rating = null
        this.recommend = null
      } else {
        if (!this.name) this.errors.push("Name required.")
        if (!this.review) this.errors.push("Review required.")
        if (!this.rating) this.errors.push("Rating required.")
        if (!this.recommend) this.errors.push("Recommendation required.")
      }
    }
  }
})
var app = new Vue({
  el: "#app",
  data: {
    premium: true,
    cart: []
  },
  methods: {
    updateCart(id) {
      this.cart.push(id)
    }
  }
})
```

HTML

```html
<div class="nav-bar"></div>
<div id="app">
  <div class="cart">
    <p>Cart({{ cart.length }})</p>
  </div>
  <product :premium="premium" @add-to-cart="updateCart"></product>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.10/vue.min.js"></script>
```

<details><summary>CSS</summary>

```css
body {
  font-family: tahoma;
  color: #282828;
  margin: 0px;
}

.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  height: 60px;
  margin-bottom: 15px;
}

.product {
  display: flex;
  flex-flow: wrap;
  padding: 1rem;
}

img {
  border: 1px solid #d8d8d8;
  width: 70%;
  margin: 40px;
  box-shadow: 0px 0.5px 1px #d8d8d8;
}

.product-image {
  width: 80%;
}

.product-image,
.product-info {
  margin-top: 10px;
  width: 50%;
}

.color-box {
  width: 40px;
  height: 40px;
  margin-top: 5px;
}

.cart {
  margin-right: 25px;
  float: right;
  border: 1px solid #d8d8d8;
  padding: 5px 20px;
}

button {
  margin-top: 30px;
  border: none;
  background-color: #1e95ea;
  color: white;
  height: 40px;
  width: 100px;
  font-size: 14px;
}

.disabledButton {
  background-color: #d8d8d8;
}

.review-form {
  width: 400px;
  padding: 20px;
  margin: 40px;
  border: 1px solid #d8d8d8;
}

input {
  width: 100%;
  height: 25px;
  margin-bottom: 20px;
}

textarea {
  width: 100%;
  height: 60px;
}
```

</details>

## 11. Tabs

[Lesson link](https://www.vuemastery.com/courses/intro-to-vue-js/tabs)

Key concepts

- Displaying only one review at a time with tabs:

  - Create the `product-tabs` component:
    - `template` to set up the HTML for each tab
    - `data() {}` containing an array for the tabs
    - An entry for `selectedTab`. In this case, we set `'Reviews'` to be the default selected tab.
    - The `tab` in the `template` is set by `v-for="(tab, index) in tabs"`, which calls the `tabs: []` array set in `data()`. The `index` is needed to enable [keyed `v-for`](https://vuejs.org/v2/style-guide/#Keyed-v-for-essential).
    - Two-way binding enables us to update the data displayed to the selected tab, with an event handler, `@click="selected Tab = tab"`.
    - We also need to visually indicate which tab is selected. We bind an `activeTab` class to the `selectedTab` with `:class="{ activeTab: selectedTab === tab }"`.
  - `product-tabs` will need to accept `reviews` as a prop.
  - Add an HTML element for the `product-tabs` component to the parent element (`product-reviews`).
  - Add `v-show` to each component to selectively display content: `v-show="selectedTab === 'Reviews'"`.

- When inspecting with the Vue dev tools, we see that `<productReview>` is a grandchild of `<Product>`. We need to modify the components so the grandchild can communicate up to the grandparent.
  - Rather than configure each component's communication system individually, we are going to create a new global component called `eventBus` and configure communication for the entire app at once. It's like a bus on the road. It moves data from one place to another. This is a small-scale temporary solution for moving data around the app.
  - As the application grows, we need state management with **Vuex**.
  - The new component includes the `mounted` lifecycle hook. There are several of these lifecycle hooks. These will be covered more in the next course.

Challenge: Add tabs for shipping and details that display the shipping cost and product details.

- Create a new `info-tabs` component. At first, I was attempting to add the new info to the `product-tabs` component, but the solution uses a separate component.
- Add 'Shipping' and 'Details' to the `data() { return { tabs: [] } }` array in the `product-tabs` component.
- Add divs for shipping and details to the template section of the `product-tabs` component.
- Pass data for shipping and details to the `product-tabs` component as props.

[CodePen solution](https://codepen.io/br3ndonland/pen/BbPKWq)

JavaScript

```js
//Create tabs for “Shipping” and “Details” that display the shipping cost and product details, respectively.
var eventBus = new Vue()
Vue.component("product", {
  props: {
    premium: {
      type: Boolean,
      required: true
    }
  },
  template: `
    <div class="product">
    <div class="product-image">
      <img :src="image" />
    </div>
    <div class="product-info">
      <h1>{{ product }}</h1>
      <p v-if="inStock">In Stock</p>
      <p v-else>Out of Stock</p>
      <info-tabs :shipping="shipping" :details="details"></info-tabs>
      <div class="color-box"
        v-for="(variant, index) in variants"
        :key="variant.variantId"
        :style="{ backgroundColor: variant.variantColor }"
        @mouseover="updateProduct(index)"
        >
      </div>
      <button v-on:click="addToCart"
        :disabled="!inStock"
        :class="{ disabledButton: !inStock }"
        >
        Add to cart
      </button>
    </div>
    <product-tabs :reviews="reviews"></product-tabs>
    </div>
  `,
  data() {
    return {
      product: "Socks",
      brand: "Vue Mastery",
      selectedVariant: 0,
      details: ["80% cotton", "20% polyester", "Gender-neutral"],
      variants: [
        {
          variantId: 2234,
          variantColor: "green",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg",
          variantQuantity: 10
        },
        {
          variantId: 2235,
          variantColor: "blue",
          variantImage:
            "https://www.vuemastery.com/images/challenges/vmSocks-blue-onWhite.jpg",
          variantQuantity: 0
        }
      ],
      reviews: []
    }
  },
  methods: {
    addToCart() {
      this.$emit("add-to-cart", this.variants[this.selectedVariant].variantId)
    },
    updateProduct(index) {
      this.selectedVariant = index
    }
  },
  computed: {
    title() {
      return `${this.brand} ${this.product}`
    },
    image() {
      return this.variants[this.selectedVariant].variantImage
    },
    inStock() {
      return this.variants[this.selectedVariant].variantQuantity
    },
    shipping() {
      if (this.premium) {
        return "Free"
      }
      return 2.99
    }
  },
  mounted() {
    eventBus.$on("review-submitted", productReview => {
      this.reviews.push(productReview)
    })
  }
})
Vue.component("product-review", {
  template: `
    <form class="review-form" @submit.prevent="onSubmit">
      <p>
        <label for="name">Name:</label>
        <input id="name" v-model="name">
      </p>
      <p>
        <label for="review">Review:</label>
        <textarea id="review" v-model="review"></textarea>
      </p>
      <p>
        <label for="rating">Rating:</label>
        <select id="rating" v-model.number="rating">
          <option>5</option>
          <option>4</option>
          <option>3</option>
          <option>2</option>
          <option>1</option>
        </select>
      </p>
      <p>
        <input type="submit" value="Submit">
      </p>
    </form>
  `,
  data() {
    return {
      name: null,
      review: null,
      rating: null,
      errors: []
    }
  },
  methods: {
    onSubmit() {
      this.errors = []
      if (this.name && this.review && this.rating) {
        let productReview = {
          name: this.name,
          review: this.review,
          rating: this.rating
        }
        eventBus.$emit("review-submitted", productReview)
        this.name = null
        this.review = null
        this.rating = null
      } else {
        if (!this.name) this.errors.push("Name required.")
        if (!this.review) this.errors.push("Review required.")
        if (!this.rating) this.errors.push("Rating required.")
      }
    }
  }
})
Vue.component("product-tabs", {
  props: {
    reviews: {
      type: Array,
      required: false
    }
  },
  template: `
    <div>
      <ul>
        <span class="tabs"
          :class="{ activeTab: selectedTab === tab }"
          v-for="(tab, index) in tabs"
          @click="selectedTab = tab"
          :key="tab"
          >
          {{ tab }}
        </span>
      </ul>
      <div v-show="selectedTab === 'Reviews'">
        <p v-if="!reviews.length">There are no reviews yet.</p>
        <ul v-else>
          <li v-for="(review, index) in reviews" :key="index">
            <p>{{ review.name }}</p>
            <p>Rating:{{ review.rating }}</p>
            <p>{{ review.review }}</p>
          </li>
        </ul>
      </div>
      <div v-show="selectedTab === 'Make a Review'">
        <product-review></product-review>
      </div>
    </div>
  `,
  data() {
    return {
      tabs: ["Reviews", "Make a Review"],
      selectedTab: "Reviews"
    }
  }
})
Vue.component("info-tabs", {
  props: {
    shipping: {
      required: true
    },
    details: {
      type: Array,
      required: true
    }
  },
  template: `
    <div>
      <ul>
        <span class="tabs"
          :class="{ activeTab: selectedTab === tab }"
          v-for="(tab, index) in tabs"
          @click="selectedTab = tab"
          :key="tab"
          >{{ tab }}
        </span>
      </ul>
      <div v-show="selectedTab === 'Shipping'">
        <p>{{ shipping }}</p>
      </div>
      <div v-show="selectedTab === 'Details'">
        <ul>
          <li v-for="detail in details">{{ detail }}</li>
        </ul>
      </div>
    </div>
  `,
  data() {
    return {
      tabs: ["Shipping", "Details"],
      selectedTab: "Shipping"
    }
  }
})
var app = new Vue({
  el: "#app",
  data: {
    premium: true,
    cart: []
  },
  methods: {
    updateCart(id) {
      this.cart.push(id)
    }
  }
})
```

HTML

```html
<div class="nav-bar"></div>
<div id="app">
  <div class="cart">
    <p>Cart({{ cart.length }})</p>
  </div>
  <product :premium="premium" @add-to-cart="updateCart"></product>
</div>
<script src="https://cdnjs.cloudflare.com/ajax/libs/vue/2.6.10/vue.min.js"></script>
```

<details><summary>CSS</summary>

```css
body {
  font-family: tahoma;
  color: #282828;
  margin: 0px;
}

.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  height: 60px;
  margin-bottom: 15px;
}

.product {
  display: flex;
  flex-flow: wrap;
  padding: 1rem;
}

img {
  border: 1px solid #d8d8d8;
  width: 70%;
  margin: 40px;
  box-shadow: 0px 0.5px 1px #d8d8d8;
}

.product-image {
  width: 80%;
}

.product-image,
.product-info {
  margin-top: 10px;
  width: 50%;
}

.color-box {
  width: 40px;
  height: 40px;
  margin-top: 5px;
}

.cart {
  margin-right: 25px;
  float: right;
  border: 1px solid #d8d8d8;
  padding: 5px 20px;
}

button {
  margin-top: 30px;
  border: none;
  background-color: #1e95ea;
  color: white;
  height: 40px;
  width: 100px;
  font-size: 14px;
}

.disabledButton {
  background-color: #d8d8d8;
}

.review-form {
  width: 400px;
  padding: 20px;
  margin: 40px;
  border: 1px solid #d8d8d8;
}

input {
  width: 100%;
  height: 25px;
  margin-bottom: 20px;
}

textarea {
  width: 100%;
  height: 60px;
}

.tabs {
  margin-left: 20px;
  cursor: pointer;
}

.activeTab {
  color: #16c0b0;
  text-decoration: underline;
}
```

</details>

**COURSE COMPLETE!!! I RULE!!!**

<img src="img/vue-mastery-01-complete.png" alt="Vue Mastery Intro to Vue.js course completion page" width="600px" />
