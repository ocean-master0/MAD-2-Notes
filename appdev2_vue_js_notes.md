# 📚 AppDev 2 — Vue.js 2 & JavaScript Complete Notes
### Based on PYQ Analysis (May 2025 + Sep 2025)

---

## 🔥 TOPIC FREQUENCY ANALYSIS (Most Important → Least Important)

| Rank | Topic | Category | Times Repeated | Priority |
|------|-------|----------|---------------|----------|
| 1 | `this` keyword (regular vs arrow functions) | JavaScript | 5–6 times | 🔴 VERY HIGH |
| 2 | Vue Directives (v-bind, v-on, v-model, v-if, v-show, v-for) | Vue.js | 5–6 times | 🔴 VERY HIGH |
| 3 | Array methods (filter, map, sort, forEach, join) | JavaScript | 5 times | 🔴 VERY HIGH |
| 4 | Closures & var scoping | JavaScript | 4 times | 🔴 HIGH |
| 5 | Vue Reactivity System | Vue.js | 3–4 times | 🟠 HIGH |
| 6 | Computed Properties vs Methods | Vue.js | 3 times | 🟠 HIGH |
| 7 | bind / call / apply | JavaScript | 2–3 times | 🟡 MEDIUM |
| 8 | Vue Components & Props | Vue.js | 2–3 times | 🟡 MEDIUM |
| 9 | Dynamic Class Binding (:class) | Vue.js | 2 times | 🟡 MEDIUM |
| 10 | JavaScript Classes & Inheritance | JavaScript | 2 times | 🟡 MEDIUM |
| 11 | Event Listeners (addEventListener vs onclick) | JavaScript | 1–2 times | 🟢 LOW |
| 12 | Vue Named Slots | Vue.js | 1–2 times | 🟢 LOW |
| 13 | Default Parameters (\|\|, ??) | JavaScript | 1–2 times | 🟢 LOW |
| 14 | v-cloak directive | Vue.js | 1 time | 🟢 LOW |

---

# PART 1: VUE.JS 2 COMPLETE NOTES

---

## 1️⃣ Vue.js 2 — How It Works (Virtual DOM)
> 🔴 VERY IMPORTANT — Q112 (May 2025)

### Virtual DOM Kya Hota Hai?
- Jab Vue me data update hota hai, Vue **directly real DOM ko update nahi karta**
- Pehle ek virtual (JS object) copy of DOM banata hai
- Phir **diff algorithm** se purana aur naya virtual DOM compare karta hai
- Sirf jo parts change hue hain, wahi real DOM me update hote hain

```
User changes data
      ↓
Vue creates new Virtual DOM
      ↓
Diff with old Virtual DOM (finds changes)
      ↓
Only changed parts updated in Real DOM ✅
```

### ❌ Wrong Options (Trap!)
- DOM immediately update hota hai without virtual DOM → **GALAT**
- Page reload hota hai → **GALAT**
- Full component re-renders from scratch → **GALAT**
- ✅ **Correct: "The virtual DOM detects changes and updates efficiently"**

---

## 2️⃣ Vue.js 2 — Template Syntax & Directives
> 🔴 MOST REPEATED TOPIC

### 2.1 Text Interpolation
```html
<!-- Double curly braces for text -->
<p>{{ message }}</p>
<p>{{ message.toUpperCase() }}</p>
<p>{{ count + 1 }}</p>
```
> ⚠️ Note: `{{ }}` sirf **text content** ke liye hai, attributes ke liye nahi!

### 2.2 v-bind — Attribute Binding
```html
<!-- Attribute bind karna -->
<img v-bind:src="imageUrl">
<a v-bind:href="link">Click</a>

<!-- Shorthand -->
<img :src="imageUrl">
<a :href="link">Click</a>

<!-- Class bind karna -->
<div :class="myClass"></div>
```
> ✅ v-bind binds HTML attributes or component properties to Vue data expressions

### 2.3 v-on — Event Binding
```html
<!-- Event listener attach karna -->
<button v-on:click="doSomething">Click Me</button>
<input v-on:keyup.enter="submit">

<!-- Shorthand -->
<button @click="doSomething">Click Me</button>
<button @click="count++">Increment</button>
```
> ✅ v-on attaches event listeners to DOM elements

### 2.4 v-model — Two-Way Data Binding
```html
<!-- Input aur data dono sync rahenge -->
<input v-model="message">
<textarea v-model="text"></textarea>

<!-- Equivalent to: -->
<input :value="message" @input="message = $event.target.value">
```
> ✅ v-model creates two-way data binding between form inputs and Vue data properties

### 2.5 v-if vs v-show
```html
<!-- v-if — element DOM se REMOVE/ADD hota hai -->
<p v-if="isVisible">Show me!</p>

<!-- v-show — element DOM me rehta hai, sirf CSS display toggle hota hai -->
<p v-show="isVisible">Show me!</p>
```

| Feature | v-if | v-show |
|---------|------|--------|
| DOM behavior | Element **remove** ho jaata hai | Element **rehta hai** (display: none) |
| Performance | Re-render hota hai | Faster toggle |
| Initial render | Lazy (false pe kuch nahi banta) | Always renders |
| Use case | Rarely toggled content | Frequently toggled |

> 🎯 PYQ Trick: **v-show sirf CSS display toggle karta hai, DOM se remove nahi karta**

### 2.6 v-for — List Rendering
```html
<ul>
  <li v-for="item in items" :key="item.id">
    {{ item.name }}
  </li>
</ul>

<!-- Index ke saath -->
<li v-for="(item, index) in items" :key="index">
  {{ index }}: {{ item }}
</li>
```

```javascript
var app = new Vue({
  el: '#app',
  data: { numbers: [1, 2, 3] }
});

app.data.numbers.push(4); // ⚠️ Ye reactive nahi hoga!
// Q171: push() ke baad v-for 3 elements dikhayega, 4 nahi!
```

> 🎯 PYQ Q171: `app.data.numbers.push(4)` directly data object pe call karna **reactive nahi hota**. Answer: **3** `<p>` elements render honge

### 2.7 v-cloak — Flash of Uncompiled Content
```html
<style>
  [v-cloak] { display: none; }
</style>

<div v-cloak>{{ message }}</div>
```
> ✅ v-cloak prevents the flash of uncompiled template content **before Vue initializes**

---

## 3️⃣ Vue Directives Matching Table (PYQ Q167)
> 🔴 REPEATED QUESTION

| Directive | Function |
|-----------|---------|
| **v-bind** | C: Binds HTML attributes or component properties to Vue data expressions |
| **v-model** | A: Creates two-way data binding between form inputs and Vue data properties |
| **v-on** | D: Attaches event listeners to DOM elements for handling user interactions |
| **v-show** | B: Conditionally shows or hides elements using CSS display property |
| **v-cloak** | E: Prevents the flash of uncompiled template content before Vue initializes |

> ✅ **Correct Answer: 1-C, 2-A, 3-D, 4-B, 5-E**

---

## 4️⃣ Vue.js Reactivity System
> 🔴 VERY IMPORTANT — Q115 (May 2025)

### Kya Reactivity Trigger Karta Hai?
```javascript
data() {
  return {
    items: [1, 2, 3],
    obj: { a: 1 }
  }
}
```

```javascript
// ✅ REACTIVE hai (triggers update):
this.items.push(4)       // Array method use karna
this.items.splice(0, 1)  // splice bhi reactive hai
this.items = [10, 2, 3]  // Poori array replace karna
this.obj.a = 2           // Existing property update karna

// ❌ NOT REACTIVE (Vue 2 me nahi detect hota):
this.items[0] = 10       // Index se direct set karna → NOT REACTIVE!
this.items.length = 0    // Length directly set karna → NOT REACTIVE!
this.$set(this.items, 0, 10)  // Ye use karo instead
```

> 🎯 **PYQ Q115 Answer: `this.items[0] = 10` → Ye Vue 2 me reactivity trigger NAHI karta!**

### Vue.set() — Correct Way
```javascript
// New property add karna object me (reactive)
this.$set(this.obj, 'newProp', value)
Vue.set(this.items, index, newValue)
```

---

## 5️⃣ Computed Properties vs Methods
> 🔴 REPEATED — Q113, Q117

### Computed Properties
```javascript
computed: {
  // ✅ CORRECT — pure function, returns value
  fullName() {
    return this.firstName + ' ' + this.lastName;
  },

  // ✅ CORRECT — getter + setter
  fullName: {
    get() { return this.firstName + ' ' + this.lastName; },
    set(value) {
      const names = value.split(' ');
      this.firstName = names[0];
      this.lastName = names[1];
    }
  },

  // ✅ CORRECT — function style
  fullName: function() {
    return this.firstName + ' ' + this.lastName;
  },

  // ❌ INCORRECT — async computed kaam nahi karta Vue 2 me!
  async fullName() {
    return await this.fetchFullName();  // WRONG!
  }
}
```

> 🎯 **PYQ Q117: `async` computed property INCORRECT hai Vue 2 me!**

### Computed vs Methods Comparison
| Feature | Computed | Methods |
|---------|----------|---------|
| Caching | ✅ Yes (only re-runs when dependency changes) | ❌ No (runs every time) |
| Reactive | ✅ Yes | ✅ Yes |
| Use case | Derived data | Event handlers, actions |
| Syntax | Property style | Function call style |

### Shopping Cart Example (PYQ Q113)
```javascript
computed: {
  status() {
    if (this.itemCount === 0) return 'Empty Cart';
    else if (this.itemCount < 3) return 'Few Items';
    else return 'Full Cart';
  }
}
```
> **Add 3 times, Remove 1 time** → itemCount = 2 → status = "Few Items"
> totalCost = 0 + 15 + 15 + 15 - 15 = **$30**
> ✅ **Answer: Items: 2, Total Cost: $30, Status: Few Items**

---

## 6️⃣ String Concatenation Bug in Vue (PYQ Q168)
> 🔴 TRICKY CONCEPT

```javascript
data() {
  return {
    totalCost: '0',   // ⚠️ STRING hai, number nahi!
    unitPrice: 25,
    orderCount: 0
  }
},
methods: {
  orderCoffee() {
    this.totalCost += this.unitPrice;  // String + Number = String concatenation!
    this.orderCount++;
  }
}
```

```
Initial: totalCost = '0' (string)
Click 1: '0' + 25 = '025'
Click 2: '025' + 25 = '02525'
Click 3: '02525' + 25 = '0252525'
```

> 🎯 **PYQ Q168 Answer: Total Cost: 0252525, Order Count: 3**
> Fix: `totalCost: 0` (number, not string)

---

## 7️⃣ Vue Components & Props
> 🟠 MEDIUM-HIGH IMPORTANCE

### Component Registration
```javascript
Vue.component('my-counter', {
  props: ['start'],   // Parent se value receive karna
  data: function () {
    return {
      count: this.start  // Props se initial value lena
    }
  },
  template: `
    <div>
      <p>{{ count }}</p>
      <button @click="increment">Increment</button>
    </div>
  `,
  methods: {
    increment() { this.count++; }
  }
});
```

```html
<!-- Parent me use karna -->
<div id="app">
  <my-counter :start="5"></my-counter>  <!-- :start means v-bind:start -->
</div>
```

> 🎯 **PYQ Q120: `:start="5"` — colon means v-bind, toh value 5 (number) pass hoga**
> `data: { start: 10 }` parent ka data hai, component ka count `this.start = 5` se start hoga
> ✅ **Answer: Counter starts at 5**

### Props vs Data
```javascript
// Props — Parent se aata hai (read-only)
props: ['title', 'count']

// Data — Component ka own state
data() {
  return {
    localCount: this.count  // Props se initialize kar sakte ho
  }
}
```

---

## 8️⃣ Dynamic Class Binding (:class)
> 🟡 MEDIUM — Q126, Q127

```html
<div :class="[baseClass, { highlighted: isHighlighted, 'btn-primary': isPrimary, 'btn-disabled': !isEnabled }]">
  {{ buttonText }}
</div>
```

```javascript
data: {
  baseClass: 'btn',     // Always lagega
  isHighlighted: false, // highlighted class nahi lagegi
  isPrimary: true,      // btn-primary lagegi
  isEnabled: true       // btn-disabled nahi lagegi (!isEnabled = false)
}
```

**Initially (before click):** `btn btn-primary` ✅

**After toggleState() once:**
```javascript
this.isHighlighted = !false = true   → highlighted add
this.isPrimary = !true = false        → btn-primary remove
this.isEnabled = !true = false        → !isEnabled = true → btn-disabled add
```
**After 1 click:** `btn highlighted btn-disabled` ✅

> 🎯 **PYQ Q126: btn, btn-primary | Q127: btn, highlighted, btn-disabled**

---

## 9️⃣ Named Slots (PYQ Q177)
```javascript
// Child Component
Vue.component('child-component', {
  props: ['title'],
  template: `
    <div>
      <h2>{{ title }}</h2>
      <slot name="header"></slot>  <!-- Named slot -->
      <p>Child Content</p>
    </div>
  `
});
```

```html
<!-- Parent me use karna -->
<child-component :title="message">
  <template slot="header">
    <h3>{{ message.toUpperCase() }}</h3>
  </template>
</child-component>
```

**When user types "hi":**
- `message` becomes `"hi"` (v-model reactive)
- `<h2>` shows `"hi"` (title prop)
- `<h3>` in slot shows `"HI"` (toUpperCase)
- `<p>` (in parent) shows `"hi"` (via {{ message }})

> 🎯 **PYQ Q176: Both `<p>` and `<h2>` update and display "hi"** ✅
> Q177: Features used = v-model (two-way binding) + named slot + props

---

## 🔟 Vue 2 v-for Reactivity (PYQ Q171)
```javascript
var app = new Vue({
  el: '#app',
  data: { numbers: [1, 2, 3] }
});

app.data.numbers.push(4);  // ⚠️ DIRECT app.data ACCESS — NOT REACTIVE
```

```html
<p v-for="n in numbers">{{ n }}</p>
```

> `app.data` ke bajaay `app.numbers.push(4)` use karna chahiye reactive ke liye
> `app.data.numbers.push(4)` → Vue is update ko **detect nahi karta**
> ✅ **PYQ Q171 Answer: 3 `<p>` elements render honge (not 4)**

---

---

# PART 2: JAVASCRIPT COMPLETE NOTES

---

## 1️⃣ `this` Keyword — Regular Function vs Arrow Function
> 🔴 MOST REPEATED JS TOPIC — Q119, Q122, Q123, Q173

### Golden Rules
```
Regular function → this = jis object ne call kiya (runtime me decide)
Arrow function   → this = lexical scope (jahan define hua wahan ka this)
```

### PYQ Q166 — Closures with Subject
```javascript
let subject = "Dog";  // global

function camera() {
  let subject = "Deer";  // local
  function click() {
    console.log(`clicked picture of ${subject}`);  // closure — refers to "Deer"
  }
  subject = "Cat";  // ← ye subject "Cat" ho gaya!
  return click;
}

let cameraPicture = camera();  // subject is now "Cat" when click is returned
cameraPicture();  // "clicked picture of Cat"
```
> 🎯 **Answer: "clicked picture of Cat"** — closure captures the VARIABLE, not the VALUE

### PYQ Q173 — this in Regular vs Arrow
```javascript
const globalVar = 50;

const object1 = {
  globalVar: 100,
  method1: function() {           // Regular function
    console.log(globalVar, this.globalVar);  // 50, 100
    return () => {                // Arrow function inside regular
      console.log(globalVar, this.globalVar);  // 50, 100 (inherits this from method1)
    }
  }
}

const object2 = {
  globalVar: 200,
  method2: () => {               // Arrow function as method
    console.log(globalVar, this.globalVar);  // 50, undefined (this = global/window, no globalVar)
    return function() {          // Regular function returned
      console.log(globalVar, this.globalVar);  // 50, undefined (called without object context)
    }
  }
}

const fn1 = object1.method1();  // logs: 50 100
fn1();                           // logs: 50 100
const fn2 = object2.method2();  // logs: 50 undefined
fn2();                           // logs: 50 undefined
```
> 🎯 **PYQ Q173 Answer:**
> ```
> 50 100
> 50 100
> 50 undefined
> 50 undefined
> ```

### PYQ Q119 — Nested Object this
```javascript
const grandParent = {
  username: 'GrandParent',
  getName: function() {         // Regular: this = grandParent
    return this.username;       // "GrandParent"
  },
  parent: {
    username: 'Parent',
    getName: function() {       // Regular: this = parent object
      return this.username;     // "Parent"
    },
    child: {
      username: 'Child',
      getNameArrow: () => {     // Arrow: this = lexical (module/window level)
        return this.username;   // undefined (no username in module scope)
      },
      getNameFunc: function() { // Regular: this = child object
        return this.username;   // "Child"
      }
    }
  }
}

console.log("A: " + grandParent.getName());                          // A: GrandParent
console.log("B: " + grandParent.parent.getName());                   // B: Parent
console.log("C: " + grandParent.parent.child.getNameFunc());         // C: Child
console.log("D: " + grandParent.parent.child.getNameArrow());        // D: undefined
```
> 🎯 **PYQ Q119 Answer: A: GrandParent, B: Parent, C: Child, D: undefined**

---

## 2️⃣ Closures & var Scoping
> 🔴 HIGH REPEAT — Q116

### var vs let Scope
```javascript
// var — function scoped, NOT block scoped
// let — block scoped ✅

// Classic Closure Trap with var:
function createFunctions() {
  var funcs = [];
  for (var i = 0; i < 3; i++) {    // var i — shared across all iterations!
    funcs.push(function() {
      return i;                      // All functions capture the SAME i
    });
  }
  return funcs;
}

const functions = createFunctions();
console.log(functions[0](), functions[1](), functions[2]());
// Output: 3, 3, 3  ← Loop khatam ho chuka hai, i = 3 ho gaya
```

> 🎯 **PYQ Q116 Answer: 3, 3, 3**

### Fix with let:
```javascript
for (let i = 0; i < 3; i++) {   // let — each iteration ka own i
  funcs.push(function() {
    return i;   // Captures own i: 0, 1, 2
  });
}
// Output: 0, 1, 2
```

### PYQ Q121 — Shared Closure (createMultiplier)
```javascript
function createMultiplier(base) {
  let multiplier = base;  // Shared closure variable

  function updateMultiplier(newValue) {
    multiplier = newValue;  // Modifies shared multiplier
    return multiplier;
  }

  function multiply(num) {
    return num * multiplier;  // Uses shared multiplier
  }

  return { update: updateMultiplier, calculate: multiply, getMultiplier: () => multiplier };
}

const math1 = createMultiplier(3);  // math1's multiplier = 3
const math2 = createMultiplier(5);  // math2's multiplier = 5 (separate closure!)

console.log(math1.calculate(4));    // 4 * 3 = 12
console.log(math2.calculate(2));    // 2 * 5 = 10
console.log(math1.update(7));       // math1's multiplier = 7, returns 7
console.log(math1.calculate(4));    // 4 * 7 = 28
console.log(math2.getMultiplier()); // math2's multiplier = 5 (unaffected!)
console.log(math1.getMultiplier()); // math1's multiplier = 7
```
> 🎯 **PYQ Q121 Answer: 12, 10, 7, 28, 5, 7**

---

## 3️⃣ Array Methods — filter, map, sort, forEach, join
> 🔴 MOST REPEATED — Q164, Q165, Q172, Q118

### 3.1 Array.sort()
```javascript
// Default sort — lexicographic (alphabetical)
[10, 2, 1].sort()  // [1, 10, 2] — String comparison!

// Numeric sort — comparator function use karo
[10, 2, 1].sort((a, b) => a - b)   // [1, 2, 10] — Ascending
[10, 2, 1].sort((a, b) => b - a)   // [10, 2, 1] — Descending
```

### PYQ Q164 — sort + map + join
```javascript
let employees = [
  { name: "Rahul", age: 28 },
  { name: "Priya", age: 24 },
  { name: "Amit",  age: 32 }
];

employees.sort((a, b) => a.age - b.age);
// Sort by age ascending: Priya(24), Rahul(28), Amit(32)

console.log(employees.map(e => e.name).join(" - "));
// ["Priya", "Rahul", "Amit"].join(" - ")
// "Priya - Rahul - Amit"
```
> 🎯 **Answer: "Priya - Rahul - Amit"**

### 3.2 Array.filter()
```javascript
const nums = [1, 2, 3, 4, 5];
const evens = nums.filter(n => n % 2 === 0);  // [2, 4]
```

### PYQ Q165 — filter + map + sort (with toUpperCase)
```javascript
const users = [
  { name: "Amit",   age: 25 },
  { name: "Bhavna", age: 20 },
  { name: "Chirag", age: 30 }
];

const names = users
  .filter(u => u.age >= 25)          // Amit(25), Chirag(30) — Bhavna OUT
  .map(u => u.name.toUpperCase())    // ["AMIT", "CHIRAG"]
  .sort();                           // Alphabetical: ["AMIT", "CHIRAG"]

console.log(JSON.stringify(names));  // '["AMIT","CHIRAG"]'
```
> 🎯 **Answer: `["AMIT", "CHIRAG"]`**

### 3.3 Array.map()
```javascript
const arr = [1, 2, 3];
const doubled = arr.map(n => n * 2);  // [2, 4, 6]

// map with this context
const result = arr.map(function(item, index) {
  return this[index] * item;
}, [10, 20, 30]);
// this = [10, 20, 30] (second argument)
// result = [1*10, 2*20, 3*30] = [10, 40, 90]
```
> 🎯 **PYQ Q114 Answer: `[10, 40, 90]`** — `this` is the second arg `[10, 20, 30]`

### PYQ Q172 — Chained filter + map + filter
```javascript
const products = [
  { name: 'Laptop', price: 1200, category: 'Electronics', inStock: true },
  { name: 'Book',   price: 25,   category: 'Education',   inStock: false },
  { name: 'Phone',  price: 800,  category: 'Electronics', inStock: true },
  { name: 'Pen',    price: 5,    category: 'Stationery',  inStock: true },
  { name: 'Tablet', price: 600,  category: 'Electronics', inStock: false }
];

const result = products
  .filter(item => item.category === 'Electronics' && item.inStock)
  // → Laptop(inStock:true), Phone(inStock:true) — Tablet out (inStock:false)

  .map(item => ({ ...item, discountedPrice: item.price * 0.9 }))
  // → Laptop: 1200*0.9=1080, Phone: 800*0.9=720

  .filter(item => item.discountedPrice > 500);
  // → Laptop(1080>500)✅, Phone(720>500)✅ — Both pass

console.log(result.length);    // 2
console.log(result[0].name);   // "Laptop" (first element)
```
> 🎯 **PYQ Q172 Answer: 2, Laptop**

### PYQ Q118 — Priority-based sort
```javascript
const products = [
  { id: 101, name: "laptop",   price: 800 },
  { id: 102, name: "mouse",    price: 25 },
  { id: 103, name: "keyboard", price: 60 },
  { id: 104, name: "monitor",  price: 300 }
];

const priorities = { "102": 1, "104": 2, "101": 3, "103": 4 };

products
  .filter(product => product.price > 50)
  // → laptop(800)✅, keyboard(60)✅, monitor(300)✅ — mouse(25)❌

  .map(product => ({ ...product, priority: priorities[product.id] }))
  // → laptop(priority:3), keyboard(priority:4), monitor(priority:2)

  .sort((a, b) => a.priority - b.priority)
  // → monitor(2), laptop(3), keyboard(4)

  .forEach(product => console.log(product.name));
  // monitor, laptop, keyboard
```
> 🎯 **PYQ Q118 Answer: monitor → laptop → keyboard**

---

## 4️⃣ bind, call, apply
> 🟡 MEDIUM — Q122

### Differences
```javascript
func.call(thisArg, arg1, arg2)    // Immediately call karo, args comma-separated
func.apply(thisArg, [arg1, arg2]) // Immediately call karo, args array me
func.bind(thisArg, arg1)          // New function return karo (nahi call karta)
```

### PYQ Q122 — bind + call example
```javascript
var globalScore = 25;

const player1 = {
  score: 150,
  getScore: function() { return this.score; },
  displayScore: function(bonus) { return this.score + (bonus || 0); }
};

const player2 = { score: 200 };
const player3 = { score: 75 };

const method1 = player1.getScore;              // Reference without binding
const method2 = player1.getScore.bind(player2); // Bound to player2
const method3 = player1.displayScore.bind(player3); // Bound to player3

console.log(method1());           // undefined — 'this' is global/window, no 'score' property!
console.log(method2());           // 200 — bound to player2
console.log(method3(50));         // 75 + 50 = 125
console.log(method3.call(player1, 25)); // 75 + 25 = 100 — bind already set player3, call doesn't override
```
> 🎯 **PYQ Q122 Answer: undefined, 200, 125, 100**
> Key: Ek baar bind ho gaya toh call/apply this override nahi kar sakta!

---

## 5️⃣ JavaScript Classes & Inheritance
> 🟡 MEDIUM — Q174

### Class Basics
```javascript
class Region {
  constructor(region) {
    this.region = region;
  }
  describe() {
    return `${this.region} is a region.`;
  }
}

class Country extends Region {
  constructor(region, name) {
    super(region);         // Parent constructor call karna MANDATORY hai!
    this.name = name;
  }
  describe() {             // Method override (polymorphism)
    return `${this.name} is in ${this.region}.`;
  }
}

let r = new Region("Asia");
let c = new Country("Europe", "Germany");
let f = c.describe;        // Method reference (unbound!)

console.log(r.describe()); // "Asia is a region."     ✅
console.log(c.describe()); // "Germany is in Europe." ✅
// f() — TypeError! 'this' undefined in strict mode (or wrong context)
```

> 🎯 **PYQ Q174:**
> - `r.describe()` → "Asia is a region." ✅
> - `c.describe()` → "Germany is in Europe." ✅
> - `f()` directly → **WRONG** (this is not bound to c, so this.name & this.region = undefined/error)
> - Country overrides both constructor AND describe() from Region ✅
> - `super(region)` IS required to initialize `this.region` ✅

---

## 6️⃣ Event Listeners — addEventListener vs onclick
> 🟡 MEDIUM — Q170

```javascript
const btn = document.getElementById('btn');

btn.addEventListener('click', () => console.log('A'));  // Adds listener
btn.onclick = () => console.log('B');                   // Replaces any previous onclick
btn.addEventListener('click', () => console.log('C'));  // Adds another listener
btn.click();  // Programmatically triggers click event
```

**Execution Order:**
1. `addEventListener('click', A)` — registered ✅
2. `btn.onclick = B` — sets onclick property ✅
3. `addEventListener('click', C)` — registered ✅
4. `btn.click()` → triggers all: **A, B, C** (in order)

> ✅ **PYQ Q170 Answer: A, B, C all get logged**
>
> Key Rules:
> - `addEventListener` multiple listeners support karta hai (replace nahi karta)
> - `onclick` sirf ek listener rakh sakta hai (replace karta hai)
> - `btn.click()` actual click ki tarah triggers karta hai

---

## 7️⃣ Default Parameters — || vs ??
> 🟢 LOW — Q175

```javascript
// Method 1: || (OR operator) — falsy values ke liye
function multiply(a, b, c) {
  a = a || 1;
  b = b || 1;
  c = c || 1;
  return a * b * c;
}
// Problem: multiply(0, 2, 3) → 1*2*3=6 (0 is falsy, 1 use hoga!)

// Method 2: Default Parameters ✅ (BEST)
function multiply(a = 1, b = 1, c = 1) {
  return a * b * c;
}
// multiply() → 1*1*1 = 1
// multiply(2) → 2*1*1 = 2
// multiply(2, 3) → 2*3*1 = 6

// Method 3: ?? (Nullish Coalescing) — sirf null/undefined ke liye
let multiply = (a, b, c) => {
  return (a ?? 1) * (b ?? 1) * (c ?? 1);
}
// multiply(0) → 0*1*1 = 0 (0 is not null/undefined!)
```

> 🎯 **PYQ Q175:** Correct approaches for "undefined treat as 1":
> ✅ `function multiply(a=1, b=1, c=1)` — Default params
> ✅ `(a ?? 1) * (b ?? 1) * (c ?? 1)` — Nullish coalescing
> ❌ `a = a || 1` — Fails for 0 values (0 falsy hai)
> ❌ `if(a === undefined) a = 1` without return — No return statement!

---

## 8️⃣ Nested Loops — String Concatenation Trap (PYQ Q169)
> 🟢 LOW

```javascript
let result = "";
for (let i = 0; i < 2; i++) {    // i = 0, 1
  for (var j = 0; j < 2; j++) {  // j = 0, 1
    result += i + j;              // String concatenation! (result is "")
  }
}
console.log(result);
```

**Step by step:**
```
i=0, j=0: result = "" + (0+0) = "" + 0 = "0"
i=0, j=1: result = "0" + (0+1) = "0" + 1 = "01"
i=1, j=0: result = "01" + (1+0) = "01" + 1 = "011"
i=1, j=1: result = "011" + (1+1) = "011" + 2 = "0112"
```

> 🎯 **Answer: "0112"**
> ✅ True statements:
> - Inner loop 2 times for each outer loop iteration ✅
> - Values are concatenated into a **string**, not added as numbers ✅
> ❌ Output is "0011" → WRONG
> ❌ Output is "0123" → WRONG

---

## 9️⃣ this Context — Arrow vs Regular in Objects (PYQ Q123)
> 🔴 HIGH

```javascript
function normalFunc() {
  console.log(this.constructor.name);  // Regular: this = caller
}

const arrowFunc = () => {
  console.log(this.constructor.name);  // Arrow: this = Window/global
}

const objj = {
  name: 'Test',
  normalMethod: normalFunc,
  arrowMethod: arrowFunc,
  outer: function() {
    return () => {         // Arrow inside regular function
      console.log(this.name);  // this = objj (lexical from outer)
    };
  }
};

const inner = objj.outer();  // outer() me this = objj

objj.normalMethod();  // "Object" (this.constructor.name of objj = "Object")
objj.arrowMethod();   // "Window" (arrow, this = global)
inner();              // "Test" (arrow captures outer's this = objj)
new arrowFunc();      // TypeError! Arrow functions can't be constructors
```

> 🎯 **PYQ Q123 Answer: All 4 are true:**
> - `obj.normalMethod()` prints "Object" ✅
> - `obj.arrowMethod()` prints "Window" or throws error in strict mode ✅
> - `inner()` prints "Test" ✅
> - `new arrowFunc()` throws error (arrow functions cannot be constructors) ✅

---

---

# PART 3: QUICK REFERENCE CHEATSHEET

---

## Vue.js 2 Quick Reference

```javascript
new Vue({
  el: '#app',           // Mount point
  data() {              // Reactive data
    return { message: 'Hello', count: 0, items: [] }
  },
  computed: {           // Cached derived values
    upperMessage() { return this.message.toUpperCase(); }
  },
  methods: {            // Event handlers & actions
    increment() { this.count++; }
  },
  watch: {              // Watch for changes
    count(newVal, oldVal) { console.log(newVal); }
  }
})
```

## JavaScript Array Methods Quick Reference

```javascript
const arr = [1, 2, 3, 4, 5];

// filter — elements jo condition pass karein
arr.filter(x => x > 2)           // [3, 4, 5]

// map — har element transform karo
arr.map(x => x * 2)              // [2, 4, 6, 8, 10]

// sort — sort (default: alphabetical!)
arr.sort((a, b) => a - b)        // [1, 2, 3, 4, 5] ascending
arr.sort((a, b) => b - a)        // [5, 4, 3, 2, 1] descending

// forEach — iterate, returns nothing
arr.forEach(x => console.log(x)) // undefined return hoga

// find — pehla matching element
arr.find(x => x > 3)             // 4

// reduce — aggregate
arr.reduce((acc, x) => acc + x, 0) // 15

// join — array to string
['a', 'b', 'c'].join(' - ')      // "a - b - c"
```

---

## 🎯 EXAM DAY TIPS

### Vue.js Traps to Remember:
1. `v-show` = CSS display:none (DOM me rehta hai)
2. `v-if` = DOM se remove (expensive!)
3. `this.items[0] = value` → **NOT reactive** in Vue 2
4. `async computed` → **INVALID** in Vue 2
5. `app.data.numbers.push()` → NOT reactive (app.numbers.push() use karo)
6. `totalCost: '0'` (string) → += causes string concatenation, not addition!
7. `v-bind:start="5"` vs `:start="5"` — same thing (colon = shorthand)

### JavaScript Traps to Remember:
1. `var` in loop → all closures share SAME variable
2. Arrow function → `this` lexical hai, caller se inherit NAHI karta
3. `bind()` once → `call()`/`apply()` se this override nahi hota
4. `arr.sort()` without comparator → **string** sort (not numeric!)
5. `method1 = obj.getScore` → unbound hai, `this` undefined hoga
6. String + Number → String concatenation (not addition)
7. Arrow functions → `new` se use nahi ho sakta (not constructors)

---

> **📝 Note:** Is notes me se **this keyword**, **Vue directives**, aur **Array methods** pe **sabse zyada dhyan do** — ye teeno topics har quiz me aate hain!
