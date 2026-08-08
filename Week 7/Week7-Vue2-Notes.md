# Week 7 


## Table of Contents

1. [Vuex — State Management for Vue 2](#1-vuex--state-management-for-vue-2)
   - 1.1 [Definition & Why We Need It](#11-definition--why-we-need-it)
   - 1.2 [Installing Vuex in a Vue 2 App](#12-installing-vuex-in-a-vue-2-app)
   - 1.3 [State](#13-state)
   - 1.4 [Getters](#14-getters)
   - 1.5 [Mutations](#15-mutations)
   - 1.6 [Actions](#16-actions)
   - 1.7 [Mapping Helpers](#17-mapping-helpers-mapstate-mapgetters-mapmutations-mapactions)
   - 1.8 [Modules](#18-modules)
   - 1.9 [Comparison: Mutation vs Action](#19-comparison-mutation-vs-action)
2. [Vue Router — Routing for Vue 2 (vue-router 3)](#2-vue-router--routing-for-vue-2-vue-router-3)
   - 2.1 [Definition & Why We Need It](#21-definition--why-we-need-it)
   - 2.2 [Basic Setup](#22-basic-setup)
   - 2.3 [Dynamic Route Matching](#23-dynamic-route-matching)
   - 2.4 [Reacting to Route Changes](#24-reacting-to-route-changes)
   - 2.5 [Nested Routes](#25-nested-routes)
   - 2.6 [Named Routes and Named Views](#26-named-routes-and-named-views)
   - 2.7 [Programmatic Navigation](#27-programmatic-navigation)
   - 2.8 [History Mode](#28-history-mode)
3. [Form Validation in Vue 2](#3-form-validation-in-vue-2)
   - 3.1 [Why Validate on Client and Server Both](#31-why-validate-on-client-and-server-both)
   - 3.2 [Core Vue Directives Used](#32-core-vue-directives-used)
   - 3.3 [Basic Custom Validation Example](#33-basic-custom-validation-example)
   - 3.4 [Overriding Native Browser Validation](#34-overriding-native-browser-validation)
   - 3.5 [Server-Side (Async) Validation](#35-server-side-async-validation)
   - 3.6 [Third-Party Validation Libraries](#36-third-party-validation-libraries)
4. [Important Definitions — Quick Revision](#4-important-definitions--quick-revision)
5. [Quick Revision Sheet](#5-quick-revision-sheet)
6. [Exam-Oriented Questions](#6-exam-oriented-questions)

---

## 1. Vuex — State Management for Vue 2

### 1.1 Definition & Why We Need It

**Definition:**
Vuex is a state management pattern + library for Vue.js applications. It serves as a centralized store for all the components in an application, with rules ensuring that the state can only be mutated in a predictable fashion.

**Explanation:**
Jab aapki app me bahut saare components hote hain aur unme se kai components ko same data (state) chahiye hota hai — jaise ek logged-in user ka naam, ya ek shopping cart ki list — to sirf props (parent → child) aur events (child → parent) se manage karna mushkil ho jata hai, especially jab **sibling components** (jo ek dusre ke parent-child nahi, balki same level pe hain) ko data share karna ho. Vuex is problem ko solve karta hai ek **global, centralized store** dekar jise koi bhi component directly access kar sakta hai — lekin us store ko **modify sirf controlled tareeke se** kiya ja sakta hai, taaki debugging aur maintainability easy rahe.

> 🎯 **Exam Point:** Vuex officially Vue team dwara maintained hai, isliye Vue ke future updates ke saath sync me rehta hai — ye ek badi reason hai ki Vuex, alag-alag community state-management libraries se zyada reliable maana jata hai (Vue 2 official docs, "What is Vuex?").

### 1.2 Installing Vuex in a Vue 2 App

Vue 2 ke saath Vuex use karne ke liye (per official Vuex docs for Vue 2, Vuex 3.x):

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

Isko root Vue instance me inject karte hain taaki har component ko `this.$store` ke through store accessible ho jaaye:

```javascript
new Vue({
  el: '#app',
  store, // inject store into all child components
  render: h => h(App)
})
```

### Line-by-Line Explanation

- `Vue.use(Vuex)` — Vuex ko Vue plugin ke roop me register karta hai, jisse `this.$store` har component ke andar available ho jaata hai.
- `new Vuex.Store({...})` — ek Store instance banata hai jisme `state`, `mutations`, `getters`, `actions` define kiye jaate hain.
- Root Vue instance me `store: store` (short form `store`) pass karna zaroori hai — isse hi ye poore component tree me inject hota hai.

### 1.3 State

**Definition:**
The `state` in a Vuex store is a single object tree that serves as the "single source of truth" for the entire application.

**Explanation:**
Vuex me hum ek hi shared state object rakhte hain — jaise Vue component ke `data` object jaisa hi hota hai, bas ab wo globally accessible hai. Iska structure bhi tree jaisa hota hai (kyunki components ka nesting bhi tree jaisa hota hai), isliye ise kabhi-kabhi *shared state tree* bhi bola jaata hai.

Component ke andar state access karne ka standard tarika **computed property** ke through hai (per Vue 2 official Vuex docs):

```javascript
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```

> 💡 **Easy Way to Remember:** Har component apna khud ka **local state** (apna `data`) bhi rakh sakta hai — Vuex sirf **shared** state ke liye hai jo multiple components use karte hain.

### 1.4 Getters

**Definition:**
Getters are computed properties for the store — they let components derive a value from the store's state without duplicating the logic in every component.

**Explanation:**
Agar bahut saare components ko store ke state se ek complex/derived value chahiye (jaise "completed todos ka count"), to har jagah wahi logic repeat karne ke bajaye, ek **getter** define karte hain — same tarah jaise Vue component me `computed` property hoti hai.

```javascript
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```

Access karne ka tarika:

```javascript
store.getters.doneTodos // → array of done todos
```

### 1.5 Mutations

**Definition:**
The only way to actually change state in a Vuex store is by committing a mutation. Vuex mutations are very similar to events — each mutation has a string type and a handler function, and this handler function is where the actual state modification happens. Mutation handlers **must be synchronous functions**.

**Explanation:**
Vuex me hum kabhi bhi state variable ko directly modify nahi karte (jaise `store.state.count = 5` — ye galat pattern hai). Iske bajaye, ek mutation function define karte hain aur usko **commit** karte hain. `commit` word ka use isliye kiya gaya hai jaise database transaction ya `git commit` me hota hai — ye ek explicit, trackable action hoti hai.

```javascript
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})

store.commit('increment')
```

Payload ke saath commit karna:

```javascript
mutations: {
  increment (state, n) {
    state.count += n
  }
}

// Style 1: normal payload
store.commit('increment', 10)

// Style 2: object-style commit
store.commit({
  type: 'increment',
  amount: 10
})
```

> ⚠️ **Important:** Vuex official documentation ke according, mutation handlers **strictly synchronous** hone chahiye. Agar aap mutation ke andar asynchronous call (jaise `fetch`) daalte hain, to Vue devtools ki **time-travel debugging** feature kaam nahi karegi, kyunki devtools ye track nahi kar payegi ki mutation exactly kis point pe complete hua.

### Debugging Support

Vue Devtools mutations ko log karte hain — har commit ka type, payload, aur uske baad ka poora state snapshot record ho jaata hai. Isse **"time-travel debugging"** possible hoti hai — matlab aap kisi bhi purani mutation ke point pe wapas ja kar dekh sakte ho system kis state me tha.

### 1.6 Actions

**Definition:**
Actions are similar to mutations, but instead of mutating the state, actions commit mutations. Actions can contain arbitrary asynchronous operations.

**Explanation:**
Kyunki mutations sirf synchronous ho sakte hain, aur real applications me kaafi baar humein asynchronous operations karne padte hain (jaise API se data fetch karna), Vuex ek alag concept deta hai — **actions**. Action khud state modify nahi karta, balki mutation ko **commit** karta hai. Action ko call karne ke liye `dispatch` use hota hai, `commit` nahi.

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
```

Destructuring ke saath cleaner syntax:

```javascript
actions: {
  increment ({ commit }) {
    commit('increment')
  }
}
```

Action ko call karna:

```javascript
store.dispatch('increment')
```

### Real Async Example (per Vuex docs pattern)

```javascript
actions: {
  incrementAsync ({ commit }) {
    return new Promise((resolve) => {
      setTimeout(() => {
        commit('increment')
        resolve()
      }, 1000)
    })
  }
}
```

### Dry Run

```text
Call:      store.dispatch('incrementAsync')
Step 1:    action function starts execution
Step 2:    setTimeout registers a 1000ms delayed callback
Step 3:    (1000 ms baad) callback fires
Step 4:    commit('increment') called → mutation runs synchronously
Step 5:    state.count updated
Step 6:    Promise resolves
```

### Composing Actions

```javascript
actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    await dispatch('actionA') // waits for actionA to finish
    commit('gotOtherData', await getOtherData())
  }
}
```

`await dispatch(...)` isliye kaam karta hai kyunki `dispatch` hamesha ek **Promise** return karta hai (Vuex 3.x behaviour, jo Vue 2 ke saath compatible hai).

### 1.7 Mapping Helpers: `mapState`, `mapGetters`, `mapMutations`, `mapActions`

**Explanation:**
Har component me baar-baar `this.$store.state.xyz` ya `this.$store.commit('xyz')` likhna repetitive ho jaata hai. Isliye Vuex helper functions deta hai jo inhe component options me directly map kar dete hain (Vue 2 Options API ke saath):

```javascript
import { mapState, mapGetters, mapMutations, mapActions } from 'vuex'

export default {
  computed: {
    ...mapState(['count']),
    ...mapGetters(['doneTodos'])
  },
  methods: {
    ...mapMutations(['increment']),
    ...mapActions(['incrementAsync'])
  }
}
```

> 💻 **Coding Point:** `...` (object spread) use hota hai kyunki ye helper functions ek object return karte hain jise `computed`/`methods` options ke andar merge karna hota hai.

### 1.8 Modules

**Definition:**
Because using a single state tree, all state of our application is contained inside one big object. Vuex allows us to divide the store into modules — each module can contain its own state, mutations, actions, and getters.

**Explanation:**
Jab app bahut badi ho jaati hai, to ek hi store object me sab kuch rakhna messy ho jaata hai. Vuex **modules** feature deta hai jisse store ko chhote-chhote logical parts me divide kar sakte hain.

```javascript
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})
```

### 1.9 Comparison: Mutation vs Action

| Feature | Mutation | Action |
|---|---|---|
| Purpose | Actually changes the state | Commits one or more mutations (business logic) |
| Called via | `store.commit('type')` | `store.dispatch('type')` |
| Synchronous/Async | Must be **synchronous** | Can contain **asynchronous** code |
| Devtools time-travel | Fully supported | Not directly (only the mutations it triggers are recorded) |
| First argument | `state` | `context` object (`{ commit, state, dispatch, getters }`) |

---

## 2. Vue Router — Routing for Vue 2 (vue-router 3)

### 2.1 Definition & Why We Need It

**Definition:**
Vue Router is the official router for Vue.js. It deeply integrates with Vue.js core to make building Single Page Applications with Vue.js a breeze.

**Explanation:**
Traditional websites me har page ek naya HTML load karta hai server se — full page reload hota hai. SPAs (Single Page Applications) me hum ek hi HTML page load karte hain, aur "navigation" JavaScript ke through client-side pe hi handle hoti hai — bina server se pura naya page maange. Vue Router ye kaam karta hai: URL ke basis pe decide karta hai ki kaun sa component render hoga.

> ⚠️ **Note:** Vue 2 ke saath **Vue Router 3.x** use hota hai (Vue Router 4.x sirf Vue 3 ke liye hai). Aapke `vue-cli/my-first-app` project me Vue `^2.6.14` use ho raha hai, isliye is context me sirf Vue Router 3 syntax hi correct hai.

### 2.2 Basic Setup

Vue CLI project me install karna:

```bash
npm install vue-router@3
```

HTML template me navigation aur outlet (per official Vue Router docs):

```html
<div id="app">
  <p>
    <!-- use router-link component for navigation -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- route outlet -->
  <router-view></router-view>
</div>
```

JavaScript setup:

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

// 1. Define route components
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }

// 2. Define routes: each route maps to a component
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]

// 3. Create the router instance
const router = new VueRouter({
  routes
})

// 4. Mount root instance
new Vue({
  router
}).$mount('#app')
```

### Line-by-Line Explanation

- `<router-link to="/foo">` — Ye compile hokar ek `<a>` tag ban jaata hai; iska `to` prop target route define karta hai. Isse browser full reload nahi karta, JS click ko intercept karta hai.
- `<router-view>` — Ye ek "outlet" hai — jis bhi component ka route currently match ho raha hai, wo yahan render hota hai.
- `new VueRouter({ routes })` — router instance banata hai; ES6 shorthand `routes` matlab `routes: routes`.

### Advantages
- Server ko hit kiye bina navigation.
- Clickable links jo directly components ke beech switch karte hain.
- Page ke sirf kuch parts refresh hote hain, pura page nahi.

### 2.3 Dynamic Route Matching

**Definition:**
A "dynamic segment" is denoted by a colon `:` — it is used to map routes with the same rendering component to different params (e.g., `/user/1`, `/user/2`).

```javascript
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}

const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})
```

`/user/foo` aur `/user/bar` dono is same route `/user/:id` se match honge, aur matched value `this.$route.params.id` ke through access hogi.

### 2.4 Reacting to Route Changes

> ⚠️ **Important:** Jab hum `/user/one` se `/user/two` navigate karte hain, Vue Router **same component instance ko reuse** karta hai (kyunki dono routes same component render karte hain). Isse component ka lifecycle hook dobara nahi chalta.

Is case me reactive updates ke liye `$route` object pe watcher lagana padta hai:

```javascript
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // react to route changes...
    }
  }
}
```

Ya `beforeRouteUpdate` navigation guard bhi use kiya ja sakta hai.

### 2.5 Nested Routes

**Definition:**
Nested routes allow a `router-view` to be placed inside a route's own component, enabling nested UI structures to be matched with a nested route configuration.

```javascript
const routes = [
  {
    path: '/user/:id',
    component: User,
    children: [
      { path: 'profile', component: UserProfile },
      { path: 'posts', component: UserPosts }
    ]
  }
]
```

`User` component ke andar bhi ek `<router-view>` hoga jahan `UserProfile` ya `UserPosts` render honge based on URL (`/user/1/profile` ya `/user/1/posts`).

### 2.6 Named Routes and Named Views

**Named Routes** — routes ko readability aur maintainability ke liye naam de sakte hain:

```javascript
const routes = [
  { path: '/user/:id', name: 'user', component: User }
]
```

```html
<router-link :to="{ name: 'user', params: { id: 123 } }">User</router-link>
```

**Named Views** — ek hi route pe multiple `router-view` components ko alag-alag naam se associate karna:

```html
<router-view></router-view>
<router-view name="sidebar"></router-view>
```

```javascript
const routes = [
  {
    path: '/',
    components: {
      default: Home,
      sidebar: Sidebar
    }
  }
]
```

### 2.7 Programmatic Navigation

`router-link` ke alawa, JavaScript se bhi navigation kiya ja sakta hai:

```javascript
router.push('/user/123')
router.push({ path: '/user/123' })
router.push({ name: 'user', params: { id: 123 } })
```

### 2.8 History Mode

**Definition:**
By default, Vue Router uses "hash mode" (URLs with `#`). Setting `mode: 'history'` uses the HTML5 History API to push proper URLs without page reloads.

```javascript
const router = new VueRouter({
  mode: 'history',
  routes
})
```

> 🎯 **Exam Point:** History mode zyada natural URL structure deta hai (bina `#`), lekin server ko configure karna padta hai taaki har URL par `index.html` serve ho, warna direct URL access pe 404 aayega.

---

## 3. Form Validation in Vue 2

### 3.1 Why Validate on Client and Server Both

**Definition:**
Form validation checks whether user-entered data meets specific criteria before it is accepted or submitted.

**Explanation:**
Client-side validation (browser ke andar) fast feedback deta hai aur unnecessary server requests ko rokta hai — server ka load kam hota hai. Lekin server ko **kabhi bhi client validation pe pura bharosa nahi karna chahiye**, kyunki koi bhi request directly (bina browser use kiye, jaise script se) server tak pahunch sakti hai. Isliye **security-critical checks hamesha server-side pe bhi dobara honi chahiye.**

### 3.2 Core Vue Directives Used

Per Vue 2 official documentation, ye core reactivity features form validation me use hote hain:

| Feature | Purpose |
|---|---|
| `v-model` | Form input (text, select, checkbox etc.) ko Vue instance ke `data` property ke saath two-way bind karta hai. |
| `v-if` | Condition false hone par element ko DOM se **completely remove** kar deta hai (error message dikhane ke liye best). |
| `v-show` | Element DOM me rehta hai, sirf CSS `display` toggle hota hai. |
| `@submit` (i.e. `v-on:submit`) | Form ke submit event ko ek method se bind karta hai. |

### 3.3 Basic Custom Validation Example

(Per Vue 2 Cookbook — Form Validation pattern)

```html
<form id="app" @submit="checkForm" action="https://vuejs.org/" method="post">
  <p v-if="errors.length">
    <b>Please correct the following error(s):</b>
    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>
  </p>

  <p>
    <label for="name">Name</label>
    <input id="name" v-model="name" type="text" name="name">
  </p>

  <p>
    <label for="age">Age</label>
    <input id="age" v-model="age" type="number" name="age" min="0">
  </p>

  <p>
    <input type="submit" value="Submit">
  </p>
</form>
```

```javascript
const app = new Vue({
  el: '#app',
  data: {
    errors: [],
    name: null,
    age: null
  },
  methods: {
    checkForm: function (e) {
      if (this.name && this.age) {
        return true
      }

      this.errors = []

      if (!this.name) {
        this.errors.push('Name required.')
      }
      if (!this.age) {
        this.errors.push('Age required.')
      }

      e.preventDefault()
    }
  }
})
```

### Code Explanation

- `data` me `errors`, `name`, `age` ko explicitly declare karna zaroori hai — Vue 2 ki reactivity system sirf un properties ko track karti hai jo instance creation ke time already `data` me mojood thi. Agar koi property baad me add ki jaaye, wo reactive nahi hogi (Vue 2 ki ek well-known limitation).
- `checkForm(e)` — `e` submit event object hai. Jab validation fail ho, `e.preventDefault()` call karke default form-submit (jo browser ko server pe POST request bhejta) rok diya jaata hai.
- `v-if="errors.length"` — Array khali ho to `errors.length` `0` hota hai jo JavaScript me falsy hai, isliye error box automatically hide ho jaata hai.

### Dry Run

| Step | `this.name` | `this.age` | Action | `errors` |
|---|---|---|---|---|
| 1 | `null` | `null` | Both missing → push both messages | `['Name required.', 'Age required.']` |
| 2 | `"Ravi"` | `null` | Name ok, age missing | `['Age required.']` |
| 3 | `"Ravi"` | `21` | Both present → `return true` early | `[]` (form submits) |

### 3.4 Overriding Native Browser Validation

Browser khud bhi kuch inputs (jaise `type="email"`) ko validate karta hai. Isse override karne ke liye `novalidate="true"` form attribute pe lagate hain:

```html
<form id="app" @submit="checkForm" action="https://vuejs.org/" method="post" novalidate="true">
  ...
  <input id="email" v-model="email" type="email" name="email">
  ...
</form>
```

```javascript
methods: {
  checkForm: function (e) {
    this.errors = []

    if (!this.name) {
      this.errors.push('Name required.')
    }
    if (!this.email) {
      this.errors.push('Email required.')
    } else if (!this.validEmail(this.email)) {
      this.errors.push('Valid email required.')
    }

    if (!this.errors.length) {
      return true
    }

    e.preventDefault()
  },
  validEmail: function (email) {
    const re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
    return re.test(email)
  }
}
```

> 💡 **Easy Way to Remember:** `novalidate="true"` browser ki automatic validation ko off karta hai; iske baad validation ki **poori zimmedari aapke JS code ki hai**.

### 3.5 Server-Side (Async) Validation

Kuch validations (jaise "kya ye username already exist karta hai") server se check karne padte hain — ye asynchronous hota hai (`fetch`):

```javascript
const app = new Vue({
  el: '#app',
  data: {
    errors: [],
    name: ''
  },
  methods: {
    checkForm: function (e) {
      e.preventDefault()
      this.errors = []

      if (this.name === '') {
        this.errors.push('Product name is required.')
        return
      }

      fetch(apiUrl + encodeURIComponent(this.name))
        .then(async res => {
          if (res.status === 200) {
            alert('OK')
          } else if (res.status === 400) {
            const errorResponse = await res.json()
            this.errors.push(errorResponse.error)
          }
        })
    }
  }
})
```

> 🧠 **Concept:** Yahan `e.preventDefault()` **hamesha** shuru me hi call kiya jaata hai (async check se pehle), kyunki humein server ka response aane tak wait karna hai — form ko turant submit nahi hone dena.

### 3.6 Third-Party Validation Libraries

Vue 2 ecosystem me kuch popular dedicated form-validation libraries bhi hain jo error-message management, complex rules jaise features built-in dete hain:

- **VeeValidate** (Vue 2 compatible versions available)
- **vue-validator**

Ye libraries manual `errors` array maintain karne ki jagah declarative validation rules provide karti hain.

---

## 4. Important Definitions — Quick Revision

### 1. Vuex
> **Definition:** Vuex is a state management pattern + library for Vue.js applications that serves as a centralized store for all components, with rules ensuring the state can only be mutated in a predictable fashion.

### 2. State (in Vuex)
> **Definition:** A single object tree that serves as the single source of truth for the shared data across all components of the application.

### 3. Mutation
> **Definition:** The only mechanism to change state in Vuex; a synchronous function committed via `store.commit(type, payload)`.

### 4. Action
> **Definition:** A function that can contain asynchronous operations and commits one or more mutations; triggered via `store.dispatch(type, payload)`.

### 5. Getter
> **Definition:** A computed property for the Vuex store, used to derive values from state without repeating logic in components.

### 6. Vue Router
> **Definition:** The official routing library for Vue.js that maps URL paths to components, enabling client-side navigation in Single Page Applications.

### 7. Dynamic Segment
> **Definition:** A route path segment prefixed with `:` (e.g., `/user/:id`) that matches variable values and exposes them via `$route.params`.

### 8. `router-view`
> **Definition:** A component that acts as an outlet, rendering the component matched by the current route.

### 9. Form Validation
> **Definition:** The process of checking whether data entered into a form meets specified criteria before allowing submission.

---

## 5. Quick Revision Sheet

- **Vuex core pieces:** `state` (data), `getters` (computed), `mutations` (sync, `commit`), `actions` (async, `dispatch`), `modules` (split large stores).
- **Rule:** Never mutate `store.state` directly — always go through a committed mutation.
- **Mutations = synchronous only** (needed for reliable time-travel debugging); **Actions = can be async**.
- **Vue Router core pieces:** `routes` array → `component`, `<router-link to="...">` for navigation, `<router-view>` as the render outlet.
- **Dynamic route reuse gotcha:** navigating between routes sharing the same component does NOT recreate the component — use a `$route` watcher or `beforeRouteUpdate`.
- **Form validation:** `v-model` for two-way binding, `v-if` to conditionally show error blocks, `@submit` + `e.preventDefault()` to intercept default form submission.
- **`novalidate="true"`** disables native browser validation so custom JS validation takes over.
- Vue 2 reactivity limitation: properties must exist in `data` at creation time to be reactive.

---

## 6. Exam-Oriented Questions

### Short Questions
1. Vuex me `commit` aur `dispatch` me kya difference hai?
2. `v-if` aur `v-show` me kya fark hai, aur error messages dikhane ke liye kaun better hai?
3. Vue Router me dynamic segment kya hota hai? Example do.

### Long Questions
1. Vuex ke state management pattern (State → Actions → View → State) ko diagram ke saath explain karo, aur batao ki mutations synchronous kyun hone chahiye.
2. Ek Vue 2 component banao jo form validation (custom + client-side) implement kare, jisme name aur email required ho.
3. Nested routes aur named views ka use case samjhao Vue Router ke context me.

### Conceptual Questions
1. Agar aap `/user/1` se `/user/2` navigate karte ho, to component reuse kyun hota hai, aur is behaviour ko handle karne ke liye kya karna padta hai?
2. Client-side form validation ke bawajood server-side validation kyun zaroori hai?

### Viva Questions
1. Vuex ke andar mutation ke bajaye action kab use karoge?
2. `this.$store` kaise available hota hai har component ke andar?
3. `router-link` normal `<a>` tag se kaise different behave karta hai?

---
Last Updated: 08 Aug 2026