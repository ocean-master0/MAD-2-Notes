# Modern Application Development 2 — Week 6 Notes


## Table of Contents

1. [Persistent Storage](#1-persistent-storage)
   - Cookies
   - sessionStorage
   - localStorage
   - IndexedDB
   - localStorage vs sessionStorage
2. [Form Validation in Vue 2](#2-form-validation-in-vue-2)
   - Client-side vs Server-side
   - Vue Validation Techniques
   - preventDefault / @submit.prevent
   - Computed Property for Validation
3. [Vue 2 Single File Components (SFC)](#3-vue-2-single-file-components-sfc)
   - Why SFC?
   - Structure (.vue file)
   - Separation of Concerns
4. [Managing Components — Build Tools](#4-managing-components--build-tools)
   - Problems with CDN Approach
   - Webpack / Vite / ESBuild
   - NPM — Node Package Manager
   - Babel
5. [Vue Router (Vue 2 CDN)](#5-vue-router-vue-2-cdn)
   - What is Routing / SPA?
   - Routes, router-link, router-view
   - Creating Router
   - Nested Routes
   - Route Parameters
   - Programmatic Navigation
   - History Mode vs Hash Mode
   - 404 / Wildcard Route
6. [Vue 2 Testing](#6-vue-2-testing)
   - Unit Testing
   - E2E Testing
   - vue-test-utils (shallowMount)
   - Writing Test Cases
7. [Graded Questions — Solved](#7-graded-questions--solved)
8. [Important Comparisons](#8-important-comparisons)
9. [Important Definitions](#9-important-definitions--quick-revision)
10. [Important Code Patterns](#10-important-code-patterns)
11. [Exam Preparation](#11-exam-preparation)
12. [One-Day-Before-Exam Revision](#12-one-day-before-exam-revision)

---

# 1. Persistent Storage

## Why Persistent Storage?

### Problem

> **Definition:**
> By default, Vue's reactive `data` object resets to its initial state every time the page is reloaded. This means all user input, settings, and session data are lost on refresh.

Vue ka data sirf **memory mein** rehta hai. Jaise hi page refresh hoti hai — sab kuch gone!

```
Page Refresh → Vue restarts → data = initial state
```

### Solution: Client-Side Storage

Browser mein data save kar lo taaki refresh ke baad bhi mile.

### Three Use Cases

```
True Persistence    →  Server (database) — most reliable
Offline Support     →  localStorage (no server needed)
Simple/Config Apps  →  localStorage / sessionStorage / Cookies
```

---

## 1.1 Cookies

### Definition

> A **cookie** is a small piece of data (max ~4KB) stored by the browser, associated with a domain, and automatically sent with every HTTP request to that domain.

### Explanation

Cookie ek chhoti parchi jaisi hoti hai jo browser save karta hai. Jab bhi server ko request bhejo, ye parchi saath mein automatically jaati hai. Iska size limit bahut chhota hai (~4KB).

### Use in JS

```javascript
// Set karna
document.cookie = "username=Abhishek";

// Session cookie — `expires` aur `max-age` omit karo; browser session end par delete hoti hai
document.cookie = "username=Abhishek; path=/";
```

### Limitations

- Sirf ~4KB data
- Har HTTP request ke saath automatically send hoti hai (unnecessary data transfer)
- Simple data ke liye suitable — objects ke liye nahi

> 🎯 **Exam Point:** Cookies automatically HTTP requests ke saath bhejte hain — localStorage nahi bhejti.

---

## 1.2 sessionStorage

### Definition

> **sessionStorage** is a Web Storage API that stores key-value pairs **for the duration of the page session only**. Data is cleared when the browser tab or window is closed.

### Explanation

Session storage ek temporary locker ki tarah hai — tab khuli hai tab tak data hai. Jaise hi tab band karo, sab data gone!



### Important Points

- Tab/window band hone pe data delete ho jaata hai
- Per-tab isolated — ek tab ka data doosri tab nahi dekh sakti
- ~5MB storage (cookies se zyada)
- Key-value pairs (strings only!)

### Code Example

```javascript
// Data save karna
sessionStorage.setItem('mode', 'dark');

// Data padhna
let mode = sessionStorage.getItem('mode');  // 'dark'

// Data delete karna
sessionStorage.removeItem('mode');

// Sab clear karna
sessionStorage.clear();
```

---

## 1.3 localStorage

### Definition

> **localStorage** is a Web Storage API that stores key-value pairs **persistently** — data remains stored even after the browser is closed and reopened, until explicitly deleted by JavaScript or the user.

### Explanation

localStorage ek permanent locker hai — browser band karo, restart karo, sab theek. Data tab tak rehta hai jab tak aap khud delete naho karo.



### Important Points

- Data browser restart ke baad bhi rehta hai
- Same origin ke saath tied hai: **protocol + hostname + port**
- Ek origin ka data doosre origin se access nahi hota. `http://example.com` aur `https://example.com` ka storage alag hai; subdomains bhi alag origin hote hain
- ~5 MiB storage per origin (browser policy ke hisaab se)
- Sirf **strings** store karta hai — objects ke liye `JSON.stringify()` zaroori!

### Code Example

```javascript
// Simple value save karna
localStorage.setItem('username', 'Abhishek');

// Value padhna
let name = localStorage.getItem('username');  // 'Abhishek'

// Key delete karna
localStorage.removeItem('username');

// Kitne items hain?
let count = localStorage.length;  // number

// Sab clear karna
localStorage.clear();
```

### ⚠️ Objects/Arrays ke liye JSON

```javascript
// ❌ WRONG — objects directly store nahi hote
localStorage.setItem('user', { name: 'Abhi' });
// localStorage.getItem('user') → "[object Object]" — GARBAGE!

// ✅ CORRECT — JSON.stringify se string banao
let user = { name: 'Abhi', age: 20 };
localStorage.setItem('user', JSON.stringify(user));

// Wapas padhna — JSON.parse se object banao
let savedUser = JSON.parse(localStorage.getItem('user'));
console.log(savedUser.name); // 'Abhi'
```

### localStorage with Vue 2

```javascript
// Complete pattern — persist karna aur restore karna
new Vue({
    el: '#app',
    data: {
        number: '',    // Input field se bind hai
    },
    
    // Page load pe — localStorage se restore karo
    mounted() {
        if (localStorage.number) {
            this.number = localStorage.number;
        }
    },
    
    methods: {
        addNumber() {
            // Save to localStorage
            localStorage.number = this.number;
            // Ya: localStorage.setItem('number', this.number);
        }
    }
});
```

### TodoList with localStorage (Array)

```javascript
const STORAGE_KEY = 'todo-list';

new Vue({
    el: '#app',
    data: {
        newData: '',
        todoList: []
    },
    methods: {
        addTodoList() {
            this.todoList.push(this.newData);
            this.newData = '';  // Input clear karo
            
            // Array ko JSON string banake save karo
            localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todoList));
        }
    },
    mounted() {
        // Restore array from localStorage
        let saved = localStorage.getItem(STORAGE_KEY);
        if (saved) {
            this.todoList = JSON.parse(saved);  // String → Array
        }
    }
});
```

---

## 1.4 IndexedDB

### Definition

> **IndexedDB** is a transactional, object-oriented client-side database system built into browsers. It allows storing and retrieving complex JavaScript objects using a key, supporting large amounts of structured data.

### Explanation

IndexedDB ek full database hai — browser ke andar! Simple key-value se zyada complex data store kar sakte ho. Jaise ek mini-SQLite browser mein.

### When to Use

- Bahut zyada data store karna ho (hundreds of MB bhi possible)
- Complex queries chahiye
- Offline-first applications
- Files, blobs store karne ho

### Comparison

| Feature | Cookies | sessionStorage | localStorage | IndexedDB |
|---------|---------|----------------|--------------|-----------|
| Size | ~4KB | ~5 MiB | ~5 MiB | Browser quota ke according |
| Persistence | Configurable | Session only | Permanent | Permanent |
| Data type | String | String | String | Objects, Files |
| Sent with HTTP? | ✅ Yes | ❌ No | ❌ No | ❌ No |
| Complexity | Simple | Simple | Simple | Complex API |
| Use case | Auth tokens | Temp state | Config, simple data | Large structured data |

---

## 1.5 localStorage vs sessionStorage — Key Differences

| Feature | localStorage | sessionStorage |
|---------|-------------|----------------|
| Persistence | Permanent (till deleted) | Session only (tab close = gone) |
| Browser restart | Data survives ✅ | Data lost ❌ |
| Tab isolation | Shared across tabs | Per-tab isolated |
| Use case | User preferences, settings | One-time form data, temp state |

> ⚠️ **Important:** sessionStorage data ek tab ke baad doosri tab mein **share nahi hota** — yahan ek tab close hona data loss ka reason hai.

> 🎯 **Exam Point (from Graded Q5 vs Q6):**
> - **sessionStorage** use kiya → Tab close karo → Data gone → App fresh start
> - **localStorage** use kiya → Tab close karo → Data persists → Same state milti hai

---

## Storage API Methods — Complete Reference

```javascript
// localStorage ya sessionStorage — same API!
storage.setItem(key, value)     // Save karo
storage.getItem(key)            // Padhna
storage.removeItem(key)         // Ek item delete karo
storage.clear()                 // Sab delete karo
storage.length                  // Kitne items hain (property)
storage.key(index)              // Index se key name lo
```

> ⚠️ **Common Mistake (from Graded Q8):** `removeElement()` koi method nahi hai! Sahi method hai `removeItem()`. Aur `length` ek **property** hai, method nahi.

> ⚠️ **Exam Trap:** Ek domain ka localStorage data uski **subdomain se access nahi** hota directly.

---

# 2. Form Validation in Vue 2

## Definition

> **Form Validation** is the process of checking user input against predefined rules before the data is submitted or processed, ensuring data integrity and security.

## Why Validate?

- User ko invalid input ka immediate feedback mile
- Unnecessary server request kam ho
- **Security/data integrity ke liye server-side validation aur sanitization zaroori hai**; client-side validation bypass ho sakti hai

## Client-Side vs Server-Side Validation

| Feature | Client-Side | Server-Side |
|---------|-------------|-------------|
| Where runs | Browser (Vue/JS) | Server (Flask/Python) |
| Speed | Fast — instant feedback | Slower — round trip |
| Security | ❌ Can be bypassed | ✅ Essential for security |
| Purpose | UX improvement | Data integrity, security |
| Required? | Optional (nice to have) | **Always required** |

> ⚠️ **Important:** Client-side validation sirf UX ke liye hai — security ke liye **always server-side** bhi karo! Client-side easily bypass ho sakta hai (browser console se).

---

## 2.1 Vue Validation Techniques

### Using `v-if` / `v-show` for Error Messages
### `v-if` (Conditional Rendering)

**Definition:** `v-if` DOM (Document Object Model) level par elements ko dynamically **create** ya **destroy** (remove) karta hai based on the condition.

**Code Example:**

```html
<div v-if="isVisible">Hello Vue!</div>

```

* **Jab Condition True ho (`isVisible = true`):** Element DOM mein fully insert aur render hota hai.
* *Actual DOM Output:* `<div>Hello Vue!</div>`


* **Jab Condition False ho (`isVisible = false`):** Element DOM se completely destroy/remove ho jata hai.
* *Actual DOM Output:* `<!--v-if-->` *(Sirf memory ke liye ek comment node bachta hai, element exist nahi karta)*



---

### `v-show` (Conditional Display)

**Definition:** `v-show` element ko humesha DOM mein render karke rakhta hai, aur sirf CSS ki `display` property ko toggle karta hai.

**Code Example:**

```html
<div v-show="isVisible">Hello Vue!</div>

```

* **Jab Condition True ho (`isVisible = true`):** CSS normal rehti hai aur element visible hota hai.
* *Actual DOM Output:* `<div>Hello Vue!</div>`


* **Jab Condition False ho (`isVisible = false`):** Element DOM mein hi rehta hai, bas uspe inline CSS `display: none` lag jata hai jisse wo hide ho jata hai.
* *Actual DOM Output:* `<div style="display: none;">Hello Vue!</div>`

```html
<!-- Error message sirf tab dikhe jab error ho -->
<input v-model="username" placeholder="Enter username">
<div v-if="error" class="error">
    {{ error }}
</div>
```

### Using `v-model` for Two-Way Binding


**Definition:** `v-model` form inputs (jaise `<input>`, `<textarea>`, `<select>`) aur Vue instance ke data/state ke beech **Two-Way Data Binding** create karta hai. Iska matlab hai: State change hone par input update hoga, aur user ke input type karne par state automatically update hogi.

**Code Example (Checkbox - True/False ke liye):**

```html
<input type="checkbox" v-model="isChecked">
<p>Status: {{ isChecked }}</p>

```

* **Jab Value True ho (`isChecked = true`):**
* *UI par asar:* Checkbox par tick (checked) show hoga.
* *State par asar:* Agar user UI me checkbox ko tick karta hai, toh variable automatically `true` ho jayega.


* **Jab Value False ho (`isChecked = false`):**
* *UI par asar:* Checkbox khali (unchecked) show hoga.
* *State par asar:* Agar user UI me checkbox ko untick karta hai, toh variable automatically `false` ho jayega.



---

**Code Example (Text Input - General Use):**

```html
<input type="text" v-model="userName">

```

* **Kaise kaam karta hai:** Agar script me `userName = "Abhishek"` set kiya, toh textbox me pehle se "Abhishek" likha aayega. Aur agar user textbox me type karke usko "Amit" banata hai, toh script me `userName` ki value apne aap "Amit" ban jayegi.

```javascript
// v-model automatically Vue variable se input sync karta hai
// Jab input change hoti hai → Vue variable update hoti hai → validation re-runs
```

### Using `@blur` for Field-Level Validation

```html
<!-- User jab field se bahar jaaye (focus lose) tab validate karo -->
<input v-model="form.name" @blur="validateName">
```

### Using `@submit.prevent` for Form Submission

```html
<!-- Default form submission (page reload) rok do, apna handler chalaao -->
<form @submit.prevent="submitForm">
    ...
</form>
```

> 💡 **Easy Way to Remember:** `@submit.prevent` = `event.preventDefault()` + `@submit` ek saath!

---

## 2.2 Complete Form Validation Example

> **Vue 2 pattern:** CDN-based Vue 2 apps are started with `new Vue({ el: '#app', ... })`. The instance below uses the Vue 2 Options API (`data`, `computed`, and `methods`), so it can be used directly with the Vue 2 CDN build.

```html
<!-- index.html -->
<div id="app">
    <form @submit.prevent="submitForm">
        
        <label>Name:</label>
        <input v-model="form.name" 
               @blur="validateName"
               placeholder="Min 3 characters">
        <span v-if="errors.name" style="color:red;">{{ errors.name }}</span>
        
        <label>Password:</label>
        <input v-model="form.password" 
               @blur="validatePassword"
               type="password"
               placeholder="Min 6 characters">
        <span v-if="errors.password" style="color:red;">{{ errors.password }}</span>
        
        <!-- Submit button disabled hai jab tak form valid nahi -->
        <button type="submit" :disabled="!isFormValid">Submit</button>
        
    </form>
</div>
```

```javascript
// app.js — Vue 2
new Vue({
    el: '#app',
    data() {
        return {
            form: { name: '', password: '' },
            errors: { name: '', password: '' }
        }
    },
    
    computed: {
        // Sab fields valid hain toh true
        isFormValid() {
            return this.form.name &&
                   this.form.password &&
                   !this.errors.name &&
                   !this.errors.password;
        }
    },
    
    methods: {
        validateName() {
            this.errors.name = this.form.name.length < 3 ? 'Min 3 chars' : '';
        },
        validatePassword() {
            this.errors.password = this.form.password.length < 6 ? 'Min 6 chars' : '';
        },
        submitForm() {
            // Final check before submit
            this.validateName();
            this.validatePassword();
            if (this.isFormValid) {
                // Proceed — send data
                console.log('Form submitted!', this.form);
            }
        }
    }
});
```

### Code Explanation — Line by Line

| Code | Kya karta hai |
|------|--------------|
| `@blur="validateName"` | Jab user name field se bahar jaaye — validate karo |
| `v-if="errors.name"` | Error message sirf tab dikhao jab error ho |
| `:disabled="!isFormValid"` | Button disable hai jab form valid nahi |
| `computed: { isFormValid }` | Reactive — automatically recalculate hoti hai jab data change ho |
| `@submit.prevent="submitForm"` | Form default submit rok ke apna function chalaao |

---

## 2.3 Computed vs Methods for Validation

```javascript
// Computed — reactive, cached
computed: {
    isFormValid() {
        return this.form.name && this.form.password && 
               !this.errors.name && !this.errors.password;
    }
}
// Template mein: :disabled="!isFormValid" (no parentheses)

// Method — not cached, runs every time
methods: {
    checkValid() {
        return this.form.name && this.form.password;
    }
}
// Template mein: :disabled="!checkValid()" (with parentheses)
```

| | Computed | Method |
|--|----------|--------|
| Caching | ✅ Yes — only re-runs when dependencies change | ❌ No — runs every render |
| When to use | Derived values that depend on reactive data | Actions, event handlers |
| Template syntax | `{{ isFormValid }}` | `{{ checkValid() }}` |

> 🔥 **Very Important:** Validation ke liye `computed` best hai kyunki ye **reactive** aur **cached** hota hai — unnecessary re-calculations nahi hoti.

---

## 2.4 Custom Validation

```javascript
// Vue 2 Options API: methods ke andar custom email-domain check
methods: {
    validateEmail() {
        const email = this.form.email;
        
        // Basic format check
        if (!email.includes('@')) {
            this.errors.email = 'Invalid email format';
            return;
        }
        
        // Custom domain check
        if (!email.endsWith('@iitm.ac.in')) {
            this.errors.email = 'Must use IITM email';
            return;
        }
        
        this.errors.email = '';
    }
}
```

---

# 3. Vue 2 Single File Components (SFC)

## Definition

> A **Vue 2 Single File Component (SFC)** is a `.vue` file that keeps a component's HTML template, JavaScript logic, and CSS styling together. In Vue 2, component logic is commonly written with the **Options API**: `data()`, `props`, `computed`, `methods`, and lifecycle hooks such as `mounted()`.

## Why SFC? — Problems with CDN Approach

### Problem 1: Global Namespace

```javascript
// CDN approach mein — sab global scope mein:
Vue.component('my-button', { ... });
Vue.component('my-card', { ... });

// Problem: Doosri library mein bhi 'my-button' ho toh clash!
```

### Problem 2: String Templates

```javascript
// Backticks mein HTML — editor support nahi milta properly
const Home = {
    template: `
        <div>
            <h1>...</h1>
        </div>
    `
}
// Syntax highlighting nahi, IDE warnings nahi, hard to manage
```

### Problem 3: CSS Global Scope

```html
<!-- CDN approach mein CSS sirf globally apply hoti hai -->
<style>
    p { color: red; }  <!-- Har jagah ke p element red ho jaate hain! -->
</style>
<!-- Component-level CSS isolation possible nahi hai easily -->
```

### Problem 4: No Build Step

- Babel se backwards compatibility nahi
- Modern JS features older browsers mein kaam nahi karte
- Code optimization, tree-shaking, minification nahi ho sakti

---

## 3.1 SFC Structure (.vue file)

```vue
<!-- MyComponent.vue -->

<!-- Part 1: HTML Template -->
<template>
    <div class="my-component">
        <h1>{{ title }}</h1>
        <p>{{ message }}</p>
        <button @click="greet">Click Me</button>
    </div>
</template>


<!-- Part 2: Vue 2 Options API logic -->
<script>
export default {
    name: 'MyComponent',
    data() {
        return {
            title: 'Hello World',
            message: ''
        }
    },
    methods: {
        greet() {
            this.message = 'Hello!';
        }
    }
}
</script>


<!-- Part 3: Scoped CSS (only applies to THIS component) -->
<style scoped>
    p {
        font-size: 2em;
        text-align: center;
    }
    /* Ye CSS sirf is component ke p tags pe lagegi */
</style>
```

### Three Parts Explained

| Part | Language | Purpose |
|------|----------|---------|
| `<template>` | HTML + Vue directives | Semantic content — kya dikhega |
| `<script>` | JavaScript (ES6+) | Logic — kaise kaam karega |
| `<style scoped>` | CSS | Presentation — kaisa dikhega |

### `scoped` Attribute — Important!

```css
/* scoped ke bina — global CSS (sab elements pe) */
<style>
p { color: red; }  /* Poori app ke p red ho jaate hain */
</style>

/* scoped ke saath — sirf is component ke andar */
<style scoped>
p { color: red; }  /* Sirf is component ke p red hoga */
</style>
```

> 🎯 **Exam Point:** `<style scoped>` CSS ko block-scoped banata hai — sirf us component pe apply hoti hai jahan likhi gayi hai.

---

## 3.2 Separation of Concerns in SFC

> ❓ **Question:** SFC mein sab ek file mein hai — toh separation of concerns kahan gaya?

> ✅ **Answer:** Separation of concerns ka matlab **alag files** nahi, **alag responsibilities** hai!

SFC mein:
- `<template>` → structure/content (HTML ki zimmedari)
- `<script>` → logic (JS ki zimmedari)
- `<style scoped>` → presentation (CSS ki zimmedari)

Ye teen clearly alag sections hain — alag files nahi hain, lekin responsibilities properly separated hain!

```
File: MyComponent.vue
├── <template>  → WHAT to display (HTML responsibility)
├── <script>    → HOW it works (JS responsibility)
└── <style>     → HOW it looks (CSS responsibility)
```

---

# 4. Managing Components — Build Tools

## 4.1 Why Build Tools?

`.vue` files JavaScript directly nahi samjhata. Inhe convert karna padta hai.

```
MyComponent.vue
      |
      | (Build Tool - Webpack/Vite)
      |
      v
MyComponent.js + MyComponent.css + template
```

## 4.2 Build Tools

### Webpack

> Webpack ek module bundler hai jo multiple files (JS, CSS, images) ko ek ya kuch output files mein bundle karta hai.

### Vite / ESBuild

> Vite aur ESBuild newer, faster build tools hain. Vue 2 project mein Vite use karte waqt Vue 2-compatible plugin (for example `@vitejs/plugin-vue2`) configure karna hota hai. Webpack is also a common Vue 2 setup.

### Babel

> **Babel** is a JavaScript compiler that converts modern JavaScript (ES2015+) into backwards-compatible versions for older browsers.

```
Modern JS (ES2021)
      |
   Babel
      |
Old-browser-compatible JS (ES5)
```

**Polyfill:** Agar koi browser feature natively support nahi karta, polyfill woh functionality provide karta hai. Babel sirf tab relevant polyfills add karta hai jab aap `@babel/preset-env` ko `useBuiltIns` aur compatible `core-js` configuration ke saath set karte ho.

---

## 4.3 NPM — Node Package Manager

### Definition

> **NPM (Node Package Manager)** is a package manager for JavaScript that allows developers to systematically install, manage, and share JavaScript libraries (packages/modules).

### Common Commands

```bash
npm init          # Naya project setup karo
npm install vue@2.7.16   # Vue 2 install karo
npm install       # package.json se sab dependencies install karo
npm run dev       # Development server start karo
npm run build     # Production build banao
```

### Issues with NPM

- Kuch packages apni transitive dependencies ke saath install hote hain
- `node_modules` ka size package tree par depend karta hai; bade projects mein 100 MB+ ho sakta hai
- **Supply Chain Attack:** Kisi package mein malicious code inject ho sakta hai

Illustrative dependency tree (har npm package ka tree alag hota hai):

```
your-project
    |
    +--- package-a
    |      +--- package-a-dependency
    +--- package-b
           +--- package-b-dependency
```

> ⚠️ **Important:** NPM supply chain attacks ek serious security concern hai — publicly managed packages mein malicious code inject hone ka risk hota hai.

---

## 4.4 SFC Tooling Summary

```
Developer writes:        Build Tools:           Browser gets:
.vue files          →    Webpack/Vite       →   .js + .css + .html
.js files           →    Babel              →   ES5 compatible JS
                    →    NPM packages       →   Bundled, minified
```

> 💡 **Easy Way to Remember:** CDN approach = simple, limited. Build tools = complex, powerful, production-ready.

---

# 5. Vue Router (Vue 2 CDN)

## 5.1 What is Navigation?

### Definition

> **Vue Router** is the official routing library for Vue.js that enables navigation between different views/components without reloading the entire page, enabling **Single Page Applications (SPA)**.

### Traditional Website vs SPA

```
Traditional Website:
Home → [Browser reloads entire page] → About → [Browser reloads] → Contact

SPA with Vue Router:
Home → About → Contact
(No page reload! Only component changes)
```

**SPA (Single Page Application):** Ek hi HTML file load hoti hai — baaki sab JavaScript se handle hota hai.

### Why Vue Router?

- Page reload nahi — fast navigation
- Browser history manage karta hai (Back button kaam karta hai)
- Different URLs → different components
- Nested routes support
- Route parameters (dynamic URLs)

---

## 5.2 Setup — CDN

```html
<!-- Vue 2 pehle load karo -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>

<!-- Vue Router 3 (Vue 2 ke saath compatible) -->
<script src="https://unpkg.com/vue-router@3.6.5/dist/vue-router.js"></script>
```

> ⚠️ **Important:** Is Vue 2 notes file ke router examples ke liye **Vue Router 3** use karo. `new Vue(...)`, `new VueRouter(...)`, `mode: 'history'`, aur wildcard path `'*'` yahan Vue 2 + Vue Router 3 syntax hain.

---

## 5.3 Components — Building Blocks

```javascript
// Each component = one "page"
const Home = {
    template: `<h2>Welcome to Home Page</h2>`
};

const About = {
    template: `<h2>About Us</h2>`
};

const Contact = {
    template: `<h2>Contact Page</h2>`
};
```

---

## 5.4 Routes — URL to Component Mapping

```javascript
// Routes array — URL ko component se connect karo
const routes = [
    {
        path: '/',          // URL
        component: Home     // Kaun sa component dikhao
    },
    {
        path: '/about',
        component: About
    },
    {
        path: '/contact',
        component: Contact
    }
];
```

### Route Mapping

```
URL: /          → Home Component
URL: /about     → About Component
URL: /contact   → Contact Component
```

---

## 5.5 Creating Router Object

```javascript
// VueRouter instance banao
const router = new VueRouter({
    routes: routes  // Ya shorthand: routes
});

// Vue instance mein router inject karo
new Vue({
    el: '#app',
    router: router  // Ya shorthand: router
});
```

---

## 5.6 HTML Templates

### `<router-view>` — Component Display Area

```html
<div id="app">
    <!-- Yahan current route ka component render hoga -->
    <router-view></router-view>
</div>
```

### `<router-link>` — Navigation Links

```html
<!-- <a> tags ki jagah <router-link> use karo -->
<router-link to="/">Home</router-link>
<router-link to="/about">About</router-link>
<router-link to="/contact">Contact</router-link>
```

**Why `<router-link>` instead of `<a>`?**

| `<a href="/about">` | `<router-link to="/about">` |
|--------------------|------------------------------|
| Page reload hoti hai | No page reload |
| History manually manage | Vue Router manages history |
| No active class | Active route automatically gets `router-link-active` class |

---

## 5.7 Complete Example

```html
<!DOCTYPE html>
<html>
<head>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>
    <script src="https://unpkg.com/vue-router@3.6.5/dist/vue-router.js"></script>
</head>
<body>

<div id="app">
    <h1>Vue Router Demo</h1>
    
    <!-- Navigation links -->
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link> |
    <router-link to="/contact">Contact</router-link>
    
    <hr>
    
    <!-- Component renders here -->
    <router-view></router-view>
</div>

<script>
// Step 1: Define components
const Home    = { template: '<h2>Home Page</h2>' };
const About   = { template: '<h2>About Page</h2>' };
const Contact = { template: '<h2>Contact Page</h2>' };

// Step 2: Define routes
const routes = [
    { path: '/',        component: Home },
    { path: '/about',   component: About },
    { path: '/contact', component: Contact }
];

// Step 3: Create router
const router = new VueRouter({ routes });

// Step 4: Mount Vue with router
new Vue({
    el: '#app',
    router
});
</script>

</body>
</html>
```

### Navigation Flow

```
User types URL or clicks router-link
            |
            v
      Vue Router intercepts
            |
            v
      Matches a route in routes[]
            |
            v
      Loads the matching component
            |
            v
      Renders inside <router-view>
      (No page reload!)
```

---

## 5.8 Nested Routes

### Definition

> **Nested Routes** allow a parent route's component to render child components within its own `<router-view>`, creating a hierarchy of views.

### Why Nested Routes?

Kuch pages ke andar sub-pages hote hain:

```
/about           → About page
/about/user      → About page + User info section
/about/settings  → About page + Settings section
```

### Code

```javascript
const About = {
    template: `
        <div>
            <h1>About Page</h1>
            
            <!-- Child routes yahan render honge -->
            <router-link to="/about/user">User Info</router-link>
            <router-link to="/about/settings">Settings</router-link>
            
            <router-view></router-view>  <!-- Child component here! -->
        </div>
    `
};

const AboutUser = {
    template: '<div>User: {{ $route.params.username }}</div>'
};

const routes = [
    {
        path: '/about',
        component: About,
        children: [               // Nested routes
            { 
                path: '/about/:username',  // Child route
                component: AboutUser
            }
        ]
    }
];
```

> ⚠️ **Important:** Parent component mein `<router-view>` hona zaroori hai nahi toh child routes render nahi honge!

---

## 5.9 Route Parameters

### Path Parameters (Dynamic Routes)

```javascript
// Route mein :id ek placeholder hai
{ path: '/user/:id', component: User }

// URL: /user/123 → $route.params.id = '123'
// URL: /user/456 → $route.params.id = '456'
```

```javascript
// Component mein access karna
const User = {
    template: '<div>User ID: {{ $route.params.id }}</div>',
    
    created() {
        console.log(this.$route.params.id);  // '123'
    }
};
```

### Query Parameters

```javascript
// URL: /search?q=vue&sort=new
console.log(this.$route.query.q);     // 'vue'
console.log(this.$route.query.sort);  // 'new'
```

### All $route Properties

```javascript
this.$route.params   // Path parameters: { id: '123' }
this.$route.query    // Query params: { q: 'vue' }
this.$route.path     // Current path: '/user/123'
this.$route.name     // Route name (if defined)
this.$route.fullPath // Full URL: '/user/123?q=vue'
```

---

## 5.10 Programmatic Navigation

```javascript
// Push — history mein add karo (Back button kaam karta hai)
this.$router.push('/about');
this.$router.push('/user/' + userId);

// Go — history mein navigate karo
this.$router.go(-1);  // Back (1 step peeche)
this.$router.go(-2);  // 2 steps peeche
this.$router.go(1);   // Forward

// Replace — history replace karo (Back button kaam nahi karega)
this.$router.replace('/login');
```

### After Form Submission — Navigate

```javascript
submitForm() {
    if (this.isFormValid) {
        // Password aur name route params mein bhejo
        this.$router.push(`/about/${this.form.password}/${this.form.name}`);
    }
}
```

---

## 5.11 Hash Mode vs History Mode

| Feature | Hash Mode (Default) | History Mode |
|---------|--------------------|--------------------|
| URL Format | `http://example.com/#/about` | `http://example.com/about` |
| Browser Support | Hash-based routing support wale browsers | HTML5 History API support wale browsers |
| Server Required? | No — hash nahi jaata to server | **Yes — server config zaroori** |
| Use Case | Development, GitHub Pages | Production |

### Enable History Mode

```javascript
const router = new VueRouter({
    mode: 'history',   // Hash (#) hat jaata hai URL se
    routes
});
```

### Server Configuration for History Mode

```javascript
// Express.js example
app.get('*', (req, res) => {
    res.sendFile('index.html');  // Sab routes pe index.html serve karo
});
```

> ⚠️ **Important:** History mode mein server ko configure karna padta hai. Warna direct URL visit pe 404 aayega.

---

## 5.12 Wildcard / 404 Route

```javascript
const NotFound = {
    template: '<div>404 - Page Not Found!</div>'
};

const routes = [
    { path: '/', component: Home },
    { path: '/about', component: About },
    
    // Wildcard — koi bhi matching route nahi mila → 404
    { path: '*', component: NotFound }
    
    // Ya redirect karo:
    // { path: '*', redirect: '/home' }
];
```

> 🎯 **Exam Point:** Wildcard route `*` **sab se last** mein daalo — nahi toh sab kuch wildcard match kar lega!

---

## 5.13 Vue Router Summary Table

| Concept | Code | Purpose |
|---------|------|---------|
| Include Router | CDN script tag | Router use karne ke liye |
| Define Routes | `const routes = [...]` | URL-to-component mapping |
| Create Router | `new VueRouter({ routes })` | Router instance |
| Mount Router | `new Vue({ router })` | Vue mein inject karo |
| Display Component | `<router-view>` | Current route ka component |
| Navigate | `<router-link to="...">` | No-reload navigation |
| Programmatic nav | `this.$router.push('/path')` | JS se navigate |
| Get param | `this.$route.params.id` | URL parameters |
| Get query | `this.$route.query.search` | Query string params |
| History mode | `mode: 'history'` | Clean URLs |
| Nested routes | `children: [...]` | Sub-pages |
| 404 catch | `{ path: '*', component: NotFound }` | Wildcard |

---

# 6. Vue 2 Testing

## 6.1 Why Test Vue Components?

### Definition

> **Unit Testing** for Vue components is the process of testing individual components in isolation to verify that they behave correctly under various data conditions and user interactions.

### Why Test?

- Component sahi kaam kar raha hai sure karne ke liye
- Regression prevent karna — naya code purana kuch break na kare
- Documentation ka kaam bhi karta hai — tests batate hain component kya karta hai
- Confidence ke saath refactor karna

---

## 6.2 Types of Testing

### Unit Testing

- **Kya test karta hai:** Individual component
- **Kaise:** Component ko ek test DOM mein mount karo, data inject karo, output check karo
- **Tools:** Jest, Mocha, vue-test-utils

### End-to-End (E2E) Testing

- **Kya test karta hai:** Poori application — frontend + backend
- **Kaise:** Real browser mein run karo, user actions simulate karo
- **Tools:** Cypress, Playwright, Selenium

### Cross-Browser Testing

- Chrome, Firefox, Safari, Edge mein kaam karta hai ya nahi
- Older browser versions ke saath compatibility
- **Diminishing returns:** Har purane browser ke liye support rakhna impossible task ban sakta hai — ek boundary set karo

---

## 6.3 vue-test-utils

### Definition

> **Vue Test Utils v1** is the Vue 2-compatible unit-testing library. It provides utilities to mount Vue 2 components into a virtual DOM, set data, trigger events, and make assertions about rendered output.

### Installation

```bash
npm install --save-dev @vue/test-utils@1 @vue/vue2-jest@29 jest@29 \
  jest-environment-jsdom@29 babel-jest@29 @babel/core @babel/preset-env \
  vue-template-compiler@2.7.16
# `vue-template-compiler` ka version installed Vue 2 version se exactly match hona chahiye
```

### Jest configuration for `.vue` files

```javascript
// jest.config.js
module.exports = {
    testEnvironment: 'jsdom',
    moduleFileExtensions: ['js', 'json', 'vue'],
    transform: {
        '^.+\\.js$': 'babel-jest',
        '^.+\\.vue$': '@vue/vue2-jest'
    }
};
```

### shallowMount

```javascript
import { shallowMount } from '@vue/test-utils';

// shallowMount = component ko virtual DOM mein mount karo
// (Child components "stub" ho jaate hain — actually render nahi hote)
const wrapper = shallowMount(MyComponent);
```

> 💡 **Easy Way to Remember:**
> - `shallowMount` → Component mount karo lekin child components ko real render mat karo
> - `mount` → Component + sab children ko fully render karo

---

## 6.4 Test Structure

### Basic Test

```javascript
import { shallowMount } from '@vue/test-utils';
import Hello from './Hello.vue';  // Ya component object

test('Login Validation Test', async () => {
    
    // Step 1: Component mount karo
    const wrapper = shallowMount(Hello);
    
    // Step 2: Data set karo (simulate user input)
    await wrapper.setData({ username: '   '.repeat(7) }); // 7 spaces
    
    // Step 3: Assertion — expected behavior check karo
    expect(wrapper.find('.error').exists()).toBe(true);
    
    // Step 4: Data change karo
    await wrapper.setData({ username: 'Ramachandra' });
    
    // Step 5: New assertion
    expect(wrapper.find('.error').exists()).toBe(false);
});
```

### Code Explanation

| Code | Kya karta hai |
|------|--------------|
| `shallowMount(Hello)` | Component ko virtual DOM mein mount karo |
| `await wrapper.setData({...})` | Component data update aur DOM re-render complete hone ka wait karo |
| `wrapper.find('.error')` | DOM mein `.error` class wala element dhundo |
| `.exists()` | Element hai ya nahi (true/false) |
| `expect(...).toBe(true)` | Assert karo ki ye true hona chahiye |

---

## 6.5 Test Suite

```javascript
// Multiple tests ek component ke liye
describe('Hello Component', () => {
    
    it('shows error when username is empty', async () => {
        const wrapper = shallowMount(Hello);
        await wrapper.setData({ username: '' });
        expect(wrapper.find('.error').exists()).toBe(true);
    });
    
    it('shows error when username is 7 spaces', async () => {
        const wrapper = shallowMount(Hello);
        await wrapper.setData({ username: '       ' });  // 7 spaces
        expect(wrapper.find('.error').exists()).toBe(true);
    });
    
    it('no error when username is valid', async () => {
        const wrapper = shallowMount(Hello);
        await wrapper.setData({ username: 'Ramachandra' });  // Valid
        expect(wrapper.find('.error').exists()).toBe(false);
    });
    
    it('error when first char is special', async () => {
        const wrapper = shallowMount(Hello);
        await wrapper.setData({ username: '#Radha$' });
        expect(wrapper.find('.error').exists()).toBe(true);
    });
});
```

### `describe` vs `it`/`test`

- `describe(...)` → Tests ka group (test suite)
- `it(...)` / `test(...)` → Individual test case
- `expect(...)` → Assertion — kya expect karte ho
- `.toBe(...)` → Exact match check

---

## 6.6 What Can You Test?

```javascript
// DOM elements check
wrapper.find('.error').exists()       // Element hai ya nahi
wrapper.find('input').element         // Actual DOM element
wrapper.findAll('li').length          // Kitne list items hain

// Data check  
wrapper.vm.username                   // Component ki reactive data
wrapper.vm.error                      // Computed property value

// Events simulate
await wrapper.find('button').trigger('click')  // Click simulate
await wrapper.find('input').trigger('input')   // Input simulate

// Props check
wrapper.props('title')                // Component props
```

---

## 6.7 Tools for Testing

| Tool | Purpose |
|------|---------|
| **Jest** | Test runner — tests run karta hai |
| **Mocha** | Alternative test runner |
| **Chai** | Assertion library — `expect(...).to.equal(...)` |
| **vue-test-utils** | Vue-specific mounting utilities |

---

# 7. Graded Questions — Solved

## Q1: localStorage Number Persistence

```javascript
// Answer: A
// code1: "number" → v-model="number" → directly number ko bind karo
// code2: localStorage.number = this.number; → save karo

mounted() {
    if (localStorage.number) {
        this.number = localStorage.number; // Restore on load
    }
},
methods: {
    addNumber() {
        localStorage.number = this.number; // Save on button click
    }
}
```

**Why A?** `mounted()` mein `localStorage.number` → `this.number` mein daal rahe hain. Toh input box mein `this.number` reflect hona chahiye via `v-model="number"`. Button click pe same number save hona chahiye.

---

## Q2: TodoList with localStorage

```javascript
// Answer: A
localStorage.setItem(STORAGE_KEY, JSON.stringify(this.todoList))
```

**Why?**
- Array ko localStorage mein save karna hai
- localStorage sirf strings store karta hai
- `JSON.stringify()` → Array ko JSON string mein convert karo
- `setItem()` → store karo (getItem nahi — ye save karna hai!)

---

## Q3 & Q4: sessionStorage Alternating Pattern

```javascript
created() {
    const current_user = sessionStorage.getItem('userId');
    const current_user_id = current_user ? Number(current_user) : 2;  // Default = 2
    this.currentUser = this.users.find(user => user.user_id === current_user_id);
    sessionStorage.setItem('userId', current_user_id === 1 ? '2' : '1');  // Alternate!
}
```

**Pattern:**
```
Correct trace:
First visit:    current_user = null → current_user_id = 2 → shows user 2 (Abhisek) → sets userId=1
1st refresh:    current_user = 1   → current_user_id = 1 → shows user 1 (Narendra) → sets userId=2
2nd refresh:    current_user = 2   → current_user_id = 2 → shows user 2 (Abhisek) → sets userId=1
...
After 2021 refreshes (odd number): shows user 1 (Narendra) → Answer A
```

> 🎯 **Exam Point:** After ODD number of refreshes → user 1 (Narendra). After EVEN refreshes → user 2 (Abhisek). First visit = user 2.

---

## Q5 & Q6: sessionStorage vs localStorage — Tab Close

**Q5 (sessionStorage):**
- User clicks Normal → mode='normal' saved in sessionStorage
- Tab close kiya → **sessionStorage saaf ho gayi!**
- Reopen → no sessionStorage data → `created()` default: `this.mode = 'dark'` → black background
- **Answer: A (black)**

**Q6 (localStorage):**
- User clicks Normal → mode='normal' saved in localStorage
- Tab close kiya → **localStorage persists!**
- Reopen → localStorage.getItem('mode') = 'normal' → `this.mode = 'normal'` → red background
- **Answer: B (red)**

> 🔥 **Very Important Exam Point:**
> - **sessionStorage** + tab close → **data gone** → default state
> - **localStorage** + tab close → **data stays** → last saved state

---

## Q8: localStorage API — True/False

```
A. localStorage is client-side storage → TRUE ✅

B. removeElement() method → FALSE ❌ (Correct method: removeItem())

C. length property → TRUE ✅ (property hai, method nahi — storage.length)

D. Subdomain access → FALSE ❌ (Domain ka data subdomain se access nahi hota)
```

**Answer: B and D are false**

---

## Q9: Vue Testing — Validation Assertions

```javascript
test('checks username validation cases', async () => {
// Component validates:
// 1. trim().length < 7 → error (true)
// 2. First char ASCII 33-47 or 58-64 → error (true) [special chars like #, $, @, !]

await wrapper.setData({ username: ' '.repeat(7) })  // async test: 7 spaces = trim → empty → length 0 < 7 → TRUE
expect(...).toBe(C1)  // C1 = true

await wrapper.setData({ username: 'Ramachandra' })   // 11 chars, starts with 'R' (82) → valid → FALSE
expect(...).toBe(C2)  // C2 = false

await wrapper.setData({ username: '#Radha$' })       // '#' ASCII = 35 (in 33-47 range) → TRUE
expect(...).toBe(C3)  // C3 = true

await wrapper.setData({ username: '__Radha' })       // '_' ASCII = 95 (NOT in 33-47 or 58-64) → length 7 → FALSE
expect(...).toBe(C4)  // C4 = false
});

// Answer: C — true, false, true, false
```

---

# 8. Important Comparisons

## 8.1 localStorage vs sessionStorage vs Cookies

| Feature | Cookies | sessionStorage | localStorage |
|---------|---------|----------------|--------------|
| Size | ~4KB | ~5 MiB | ~5 MiB |
| Persistence | Configurable | Tab session | Permanent |
| Tab close | Depends on expiry | **Lost** | **Survives** |
| Browser restart | Depends | Lost | **Survives** |
| HTTP auto-send | ✅ Yes | ❌ No | ❌ No |
| Subdomain access | Configurable | ❌ No | ❌ No |
| API | `document.cookie` | `sessionStorage.API` | `localStorage.API` |

## 8.2 Hash Mode vs History Mode

| | Hash Mode | History Mode |
|--|-----------|--------------|
| URL | `/about` becomes `/#/about` | `/about` |
| Server config | Not needed | **Required** |
| Direct URL access | Works | Needs server config |
| Use case | Simple, dev | Production |

## 8.3 shallowMount vs mount

| | shallowMount | mount |
|--|--------------|-------|
| Child components | Stubbed (not rendered) | Fully rendered |
| Speed | Faster | Slower |
| Use case | Isolated unit tests | Integration tests |

## 8.4 <router-link> vs <a>

| | `<a href="...">` | `<router-link to="...">` |
|--|------------------|--------------------------|
| Page reload | ✅ Yes | ❌ No |
| SPA behavior | ❌ | ✅ |
| Active class | ❌ | ✅ (auto) |
| History management | ❌ | ✅ |

## 8.5 computed vs methods for Validation

| | computed | methods |
|--|----------|---------|
| Caching | ✅ Yes | ❌ No |
| Runs when | Dependencies change | Every render |
| Template use | `{{ value }}` | `{{ value() }}` |
| Best for | Derived state, validation | Actions, events |

---

# 9. Important Definitions — Quick Revision

### 1. Persistent Storage
> Client-side mechanisms (cookies, localStorage, sessionStorage, IndexedDB) that allow data to be saved in the browser beyond the lifetime of a single page session.

### 2. localStorage
> A Web Storage API that stores key-value string pairs persistently in the browser, surviving tab closures and browser restarts, scoped to the origin domain.

### 3. sessionStorage
> A Web Storage API that stores key-value string pairs only for the duration of the page session; data is lost when the tab or window is closed.

### 4. Cookies
> Small pieces of data (~4KB) stored by the browser and automatically sent with every HTTP request to the associated domain.

### 5. IndexedDB
> A transactional, object-oriented, client-side database system that allows storing and querying large amounts of structured data including complex JavaScript objects.

### 6. Form Validation
> The process of checking user-entered data against predefined rules before submission, either on the client-side (for UX) or server-side (for security).

### 7. Single File Component (SFC)
> A Vue.js `.vue` file that encapsulates a component's HTML template, JavaScript logic, and scoped CSS in three clearly separated sections within one file.

### 8. Vue Router
> The official routing library for Vue.js that manages navigation between components based on the URL, enabling Single Page Application (SPA) behavior without page reloads.

### 9. SPA (Single Page Application)
> A web application that loads a single HTML page and dynamically updates the content using JavaScript as the user navigates, without full page reloads.

### 10. Route
> A mapping between a URL path and a Vue component, defined in the routes array passed to VueRouter.

### 11. `<router-view>`
> A special Vue Router component that acts as a placeholder where the matched route's component is rendered.

### 12. `<router-link>`
> A Vue Router component that renders a navigation link, preventing page reloads and integrating with Vue Router's history management.

### 13. Nested Routes
> A routing pattern where a parent route contains child routes, allowing components to render sub-components within their own `<router-view>`.

### 14. History Mode
> A Vue Router mode that uses HTML5 History API to create clean URLs without the `#` hash, requiring server-side configuration to serve `index.html` for all routes.

### 15. Unit Testing (Vue)
> Testing individual Vue components in isolation using a virtual DOM, injecting test data, and making assertions about the rendered output and component behavior.

### 16. shallowMount
> A vue-test-utils function that mounts a Vue component for testing while stubbing child components, enabling isolated unit testing.

### 17. Webpack
> A module bundler that takes JavaScript modules (including `.vue` files) and their dependencies and bundles them into optimized output files for the browser.

### 18. Babel
> A JavaScript compiler that converts modern JavaScript (ES2015+) into backwards-compatible code for older browsers, optionally injecting polyfills for missing features.

### 19. NPM (Node Package Manager)
> A package manager for JavaScript that installs, manages, and shares JavaScript libraries and modules, used to set up Vue projects with build tools.

### 20. Polyfill
> Extra JavaScript code that provides browser functionality which is not natively supported. Babel can add relevant polyfills only when its polyfill configuration is enabled.

---

# 10. Important Code Patterns

## Pattern 1: localStorage Save & Restore

```javascript
// Save
localStorage.setItem('key', JSON.stringify(data));

// Restore in mounted()
mounted() {
    let saved = localStorage.getItem('key');
    if (saved) {
        this.data = JSON.parse(saved);
    }
}
```

## Pattern 2: sessionStorage Mode Toggle

```javascript
// Save mode
sessionStorage.setItem('mode', 'dark');

// Restore in created()
created() {
    let mode = sessionStorage.getItem('mode');
    this.mode = mode || 'dark';  // Default 'dark' agar kuch nahi mila
}
```

## Pattern 3: Vue Router — Basic Setup

```javascript
const routes = [
    { path: '/',        component: Home },
    { path: '/about',  component: About },
    { path: '*',       component: NotFound }  // 404
];
const router = new VueRouter({ routes });
new Vue({ el: '#app', router });
```

## Pattern 4: Vue Router — Nested + Params

```javascript
const routes = [{
    path: '/about',
    component: About,
    children: [
        { path: '/about/:username', component: AboutUser }
    ]
}];

// Access in component:
this.$route.params.username
```

## Pattern 5: Programmatic Navigation

```javascript
// Push (navigatable back)
this.$router.push('/home');
this.$router.push(`/user/${this.userId}`);

// Go back
this.$router.go(-1);
```

## Pattern 6: Form Validation + Router

```javascript
submitForm() {
    this.validateAll();
    if (this.isFormValid) {
        this.$router.push(`/about/${this.form.password}/${this.form.name}`);
        this.form = { name: '', password: '' };  // Reset
    }
}
```

## Pattern 7: Vue Testing — shallowMount

```javascript
import { shallowMount } from '@vue/test-utils';

test('validates username', async () => {
    const wrapper = shallowMount(Hello);
    
    // Test invalid
    await wrapper.setData({ username: 'ab' });
    expect(wrapper.find('.error').exists()).toBe(true);
    
    // Test valid
    await wrapper.setData({ username: 'Abhishek' });
    expect(wrapper.find('.error').exists()).toBe(false);
});
```

## Pattern 8: History Mode Router

```javascript
const router = new VueRouter({
    mode: 'history',  // Hash hat jaata hai
    routes
});
```

---

# 11. Exam Preparation

## 11.1 Important Short Questions (with Answers)

### Q: What is the difference between localStorage and sessionStorage?

**Answer:** Both are Web Storage APIs but differ in persistence:
- `localStorage`: Data persists even after browser restart; survives tab close
- `sessionStorage`: Data is lost when the tab or window is closed

### Q: Why must complex objects be JSON.stringify'd before localStorage?

**Answer:** localStorage only stores strings. If an object is stored directly, it becomes `"[object Object]"`. `JSON.stringify()` converts the object to a JSON string, and `JSON.parse()` restores it on retrieval.

### Q: What is `<router-view>` and `<router-link>`?

**Answer:**
- `<router-view>` — Placeholder where the matched route's component renders
- `<router-link>` — Navigation link that prevents page reload, used instead of `<a>`

### Q: What is shallowMount in vue-test-utils?

**Answer:** `shallowMount` mounts a Vue component in a virtual DOM for testing, but stubs (replaces with dummies) all child components. This allows testing the component in isolation.

### Q: What is History Mode in Vue Router?

**Answer:** History mode uses the HTML5 History API to produce clean URLs without the `#` hash (e.g., `/about` instead of `/#/about`). It requires server configuration to serve `index.html` for all routes to avoid 404 errors on direct URL access.

### Q: Why is client-side validation not enough for security?

**Answer:** Client-side validation can be bypassed by users (e.g., via browser developer console or direct API calls). Server-side validation is **always required** for security because it cannot be bypassed.

### Q: What are nested routes?

**Answer:** Nested routes allow a parent component to have its own `<router-view>` where child routes render. Defined using the `children` array inside a route config. Both parent and child need their own `<router-view>`.

---

## 11.2 Important Long Questions

### Q1: Explain localStorage with Vue 2. How do you persist data and restore it?

Structure: Definition → Problem → Solution → Code → JSON.stringify need → mounted() restore pattern → setItem/getItem methods

### Q2: Explain Vue Router — what is it, how does it work, what are routes, router-link, router-view?

Structure: SPA explanation → setup → routes array → VueRouter instance → HTML tags → navigation flow diagram

### Q3: What are the problems with CDN-based Vue approach and how do SFCs solve them?

Structure: 4 problems (namespace, string templates, CSS, no build step) → SFC structure → separation of concerns still maintained

### Q4: Explain unit testing in Vue using vue-test-utils.

Structure: Why test → shallowMount concept → test structure → assertions → tools (Jest/Mocha/Chai)

---

## 11.3 Viva Questions

1. Cookies automatically HTTP ke saath kyon jaate hain?
2. sessionStorage tab-isolated kyun hota hai?
3. `removeItem` vs `removeElement` — kaunsa sahi hai?
4. Subdomain same localStorage access kar sakta hai ya nahi?
5. Vue Router 2 ke saath Vue Router version kaun sa?
6. History mode mein server fallback aur client-side `*` route dono ka kya role hai?
7. `shallowMount` vs `mount` — kab kaunsa use karoge?
8. Computed property aur validation ka kya connection hai?
9. `@submit.prevent` ka kya matlab hai?
10. Wildcard route ko sab se aakhir mein kyun rakhte hain?

---

# 12. One-Day-Before-Exam Revision

## 🔥 Top 15 Most Important Points

1. **localStorage** — permanent; `sessionStorage` — tab close = gone
2. **localStorage sirf strings** — objects ke liye `JSON.stringify()` / `JSON.parse()`
3. **`removeItem()`** correct hai — `removeElement()` exist nahi karta
4. Ek domain ka localStorage **subdomain se access nahi** hota
5. **`<router-link>` not `<a>`** — page reload nahi hoti
6. **`<router-view>`** — matched component yahan render hota hai
7. Vue 2 mein **Vue Router version 3** CDN se
8. **History mode** — clean URL; server config zaroori; `/*` to index.html
9. **Wildcard `*`** route — sab se aakhir mein daalo (404 handling)
10. **Nested routes** — parent mein `children:[]` aur parent template mein `<router-view>`
11. **`this.$router.push()`** — programmatic navigation
12. **`this.$route.params.id`** — URL parameters access
13. **`shallowMount`** — child components stub hote hain; isolation test
14. **Client-side validation** — UX ke liye; server-side validation — **always required** for security
15. **`<style scoped>`** — CSS sirf us component pe laagu hoti hai

## 📋 Code Cheat Sheet

```javascript
// localStorage
localStorage.setItem('key', JSON.stringify(obj));
let obj = JSON.parse(localStorage.getItem('key'));
localStorage.removeItem('key');
localStorage.length; // property

// Vue Router Basic
const routes = [
  { path: '/', component: Home },
  { path: '/:id', component: Detail },
  { path: '*', component: NotFound }
];
const router = new VueRouter({ routes });
new Vue({ el: '#app', router });

// Params access
this.$route.params.id
this.$route.query.search
this.$route.path

// Navigate
this.$router.push('/path');
this.$router.go(-1);

// History mode
const router = new VueRouter({ mode: 'history', routes });

// Nested routes
{ path: '/about', component: About,
  children: [{ path: '/about/:user', component: AboutUser }] }

// Vue Testing
test('shows validation error', async () => {
    const wrapper = shallowMount(MyComponent);
    await wrapper.setData({ username: 'test' });
    expect(wrapper.find('.error').exists()).toBe(true);
});

// Form Validation
computed: {
    isFormValid() {
        return this.form.name && !this.errors.name;
    }
}
```

## 🎯 Key Differences — One-Liners

- **localStorage vs sessionStorage:** localStorage = permanent; sessionStorage = tab ki zindagi tak
- **Hash mode vs History mode:** Hash = `#/path` (works anywhere); History = `/path` (needs server)
- **router-link vs a:** router-link = no reload, router managed; a = full reload
- **shallowMount vs mount:** shallowMount = children stubbed; mount = fully rendered
- **computed vs methods:** computed = cached; methods = runs every time
- **Client vs Server validation:** Client = UX only; Server = security essential
- **SFC vs CDN approach:** SFC = modular, scoped CSS, build step needed; CDN = simple, global scope

---

laast updated: 08-Aug-2026
