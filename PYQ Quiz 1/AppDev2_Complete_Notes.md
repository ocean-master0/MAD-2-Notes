# 📘 AppDev 2 — Complete Exam Notes

> **Based on:** Quiz 1 (Jan 2026) + End-Term PYQ Analysis  


---

## 📑 Table of Contents

| # | Topic |
|---|-------|
| 1 | JavaScript Scope — `var`, `let`, `const` |
| 2 | Hoisting |
| 3 | Functions — Declaration vs Expression vs Arrow |
| 4 | `this` Keyword + `call()`, `apply()`, `bind()` |
| 5 | Closures |
| 6 | IIFE (Immediately Invoked Function Expression) |
| 7 | JavaScript Classes & Inheritance |
| 8 | Prototype Chain |
| 9 | Object Destructuring |
| 10 | Browser Storage — `localStorage`, `sessionStorage`, Cookies |
| 11 | Vue.js Basics — `data`, `computed`, `methods` |
| 12 | Vue.js Reactivity + `setInterval` |
| 13 | Vue.js Slots — Default, Named, Fallback |
| 14 | Vue.js Lifecycle Hooks |
| 15 | UI State (Ephemeral State) |
| 16 | HTTP Stateless + State Management |

---

## 1️⃣ JavaScript Scope — `var`, `let`, `const`

### 🔑 Concept

**Scope** = Variable kahaan accessible hai, kahaan nahi.

| Keyword | Scope | Re-declare | Re-assign | Hoisted? |
|---------|-------|-----------|-----------|----------|
| `var` | **Function** scope | ✅ Yes | ✅ Yes | ✅ Yes (undefined se) |
| `let` | **Block** scope | ❌ No | ✅ Yes | ✅ Yes (TDZ se) |
| `const` | **Block** scope | ❌ No | ❌ No | ✅ Yes (TDZ se) |

### 📌 Important Rules

```
var   → function ke andar kahi bhi accessible
let   → { } block ke bahar nahi
const → { } block ke bahar nahi + value change nahi kar sakte
```

### 💻 Example

```javascript
function demo() {
    if (true) {
        var x = 10;    // function-scoped
        let y = 20;    // block-scoped
        const z = 30;  // block-scoped
    }
    console.log(x);  // ✅ 10  → var function scope me hai
    console.log(y);  // ❌ ReferenceError → let block ke bahar nahi
    console.log(z);  // ❌ ReferenceError → const block ke bahar nahi
}
```

### ⚠️ Global var aur window

```javascript
var name = "Abhishek";
console.log(window.name); // "Abhishek" ✅

let age = 20;
console.log(window.age);  // undefined ❌ — let global window se attach nahi hota
```

> **Exam Trick:** `var` global scope me declare karo to `window` object ka property ban jata hai. `let`/`const` ke saath NAHI hota.

### 🎯 PYQ Connection (Q2)
- `var` → function-scoped ✅ CORRECT
- `let` & `const` → block-scoped ✅ CORRECT
- `let` → hoisted but `undefined` se initialize nahi hota (TDZ) ✅ CORRECT
- `const` → declaration ke saath initialize karna ZAROOR hai ✅ CORRECT
- Global `var` → window se attached hota hai (NOT `let`) ❌ WRONG statement in PYQ

---

## 2️⃣ Hoisting

### 🔑 Concept

JavaScript code run karne se **pehle** variables aur functions ko memory me le jata hai. Isse **Hoisting** kehte hain.

```
Phase 1: Creation Phase → Memory allocate karo
Phase 2: Execution Phase → Code chalao
```

### 📌 Rules

```
var        → Hoisted + undefined se initialize
let/const  → Hoisted but TDZ me (Temporal Dead Zone)
Function Declaration → Poora function hoisted
Function Expression  → Sirf variable hoisted (var → undefined)
Arrow Function       → Sirf variable hoisted (var → undefined)
```

### 💻 Examples

**`var` hoisting:**
```javascript
console.log(a);  // undefined (error nahi)
var a = 10;
console.log(a);  // 10
```

**`let` → TDZ error:**
```javascript
console.log(b);  // ❌ ReferenceError: Cannot access 'b' before initialization
let b = 20;
```

**Function Declaration — fully hoisted:**
```javascript
sayHello();  // ✅ "Hello" — kaam karta hai
function sayHello() {
    console.log("Hello");
}
```

**Function Expression — NOT fully hoisted:**
```javascript
greet();  // ❌ TypeError: greet is not a function
var greet = function() {
    console.log("Hi");
};
```

> **Explanation:** `var greet` hoisted hua (value = `undefined`), lekin function value hoisted nahi hui.

### 🎯 PYQ Connection (Q3)
- Every function is an object ✅ CORRECT
- Arrow functions ke paas apna `this` binding NAHI hota ✅ CORRECT
- Function declarations are hoisted ✅ CORRECT
- Function expressions are hoisted **with their function value** ❌ WRONG

---

## 3️⃣ Functions — Declaration vs Expression vs Arrow

### 📊 Comparison Table

| Type | Syntax | Hoisted? | Own `this`? |
|------|--------|----------|-------------|
| Declaration | `function foo() {}` | ✅ Fully | ✅ Yes |
| Expression | `const foo = function() {}` | ❌ No | ✅ Yes |
| Arrow | `const foo = () => {}` | ❌ No | ❌ **No** |

### 💻 Arrow Function — `this` ka farak

```javascript
const obj = {
    name: "Abhishek",
    
    // Regular function → this = obj
    greet: function() {
        console.log(this.name);  // "Abhishek"
    },
    
    // Arrow function → this = outer this (window/undefined)
    greetArrow: () => {
        console.log(this.name);  // undefined
    }
};

obj.greet();       // "Abhishek" ✅
obj.greetArrow();  // undefined ❌
```

> **Exam Trick:** Arrow function apne **parent scope ka `this`** use karta hai, apna khud ka nahi.

---

## 4️⃣ `this` Keyword + `call()`, `apply()`, `bind()`

### 🔑 Concept

`this` = **Kaunsa object method ko call kar raha hai**

### 📌 `this` Rules

```
obj.method()        → this = obj
function()          → this = window (sloppy) / undefined (strict)
Arrow function      → this = enclosing scope ka this
new Constructor()   → this = naya object
call/apply/bind     → this = jo tum manually set karo
```

### 💻 `apply()` Example (PYQ Q4)

```javascript
var a = 3;
const obj1 = {
    a: 20,
    show: function() {
        console.log(this.a);
    }
};
const obj2 = { a: 10 };

obj1.show.apply(obj1);  // 20 ✅
```

**Kyun 20?**
- `apply(obj1)` → `this = obj1`
- `obj1.a = 20`
- Output: `20`

> **Agar `apply(obj2)` hota:** Output = `10`  
> **Agar `apply(window)` hota:** Output = `3` (global var a)

### 📊 `call`, `apply`, `bind` Difference

| Method | Kab Use | Arguments |
|--------|---------|-----------|
| `call(obj, a, b)` | Turant call karta hai | Individually comma se |
| `apply(obj, [a, b])` | Turant call karta hai | Array me |
| `bind(obj)` | New function return karta hai | Baad me call karo |

```javascript
function greet(city, country) {
    console.log(`${this.name} from ${city}, ${country}`);
}
const person = { name: "Abhishek" };

greet.call(person, "Patna", "India");      // call → arguments comma se
greet.apply(person, ["Patna", "India"]);   // apply → arguments array me
const boundGreet = greet.bind(person);     // bind → function return karta hai
boundGreet("Patna", "India");              // baad me call karo
```

---

## 5️⃣ Closures

### 🔑 Concept

**Closure** = Inner function apne outer function ke variables ko **yaad rakhta hai**, chahe outer function khatam ho gaya ho.

```
Outer Function khatam ho gaya
       ↓
Lekin inner function ke paas
outer ka variable access hai
       ↓
Yahi CLOSURE hai
```

### 💻 Basic Example

```javascript
function outer() {
    let count = 0;  // outer ka variable
    
    return function inner() {
        return count++;  // inner, outer ka count yaad rakhta hai
    };
}

const c1 = outer();  // c1 = inner function, apna count = 0
const c2 = outer();  // c2 = inner function, ALAG count = 0

console.log(c1());  // 0 (count++ → pehle 0 return, phir count = 1)
console.log(c1());  // 1 (count++ → pehle 1 return, phir count = 2)
console.log(c2());  // 0 (c2 ka apna alag count = 0)
```

> **KEY POINT:** `c1` aur `c2` ke count **ALAG-ALAG** hain. Har `outer()` call pe naya closure banta hai.

### 📌 Post-increment vs Pre-increment

```javascript
let x = 5;

// Post-increment: pehle return, phir increment
console.log(x++);  // 5  (x ab 6 hai)

// Pre-increment: pehle increment, phir return
console.log(++x);  // 7  (x pehle 7 hua, phir 7 return)
```

### 🎯 PYQ Connection (Q12 — Closure Output)

```javascript
function outer() {
    let count = 0;
    return function inner() {
        return count++;
    };
}

const c1 = outer();
const c2 = outer();

console.log(c1())  // 0
console.log(c1())  // 1
console.log(c2())  // 0  ← c2 ka apna count hai
```

**Answer: 0, 1, 0** ✅

---

## 6️⃣ IIFE — Immediately Invoked Function Expression

### 🔑 Concept

Function define hote hi **turant** execute ho jata hai.

```javascript
(function() {
    // ye immediately chalega
})();
```

### 💻 Example (PYQ Q14 se)

```javascript
function createCounter(start) {
    let count = start;  // count = 3
    
    return function(step) {
        if (typeof step === "number") {
            count += step;
            return count;
        }
        
        return (function() {     // ← IIFE
            let temp = count;    // temp = current count ka snapshot
            return function(reset = false) {
                if (reset) {
                    count = start;
                } else {
                    count++;
                }
                return temp + count;
            };
        })();  // ← immediately execute hota hai
    };
}

const counter = createCounter(3);  // start = 3, count = 3

let a = counter(2);      // step = 2, count = 5, return 5 → a = 5
let d = counter()(true); // step undefined → IIFE chalega
                         // IIFE: temp = 5
                         // (true) → reset = true → count = start = 3
                         // return temp + count = 5 + 3 = 8 → d = 8

console.log(a + d);  // 5 + 8 = 13 ✅
```

### 📌 IIFE ka use

```javascript
// IIFE se private scope banta hai
(function() {
    let secret = "hidden";  // bahar accessible nahi
    console.log(secret);    // "hidden"
})();

console.log(secret);  // ❌ ReferenceError
```

---

## 7️⃣ JavaScript Classes & Inheritance

### 🔑 Concept

```
class → Blueprint (design)
new   → Object banana (actual copy)
extends → Inheritance (parent se properties lena)
super   → Parent constructor call karna
```

### 💻 Example (PYQ Q8 & Q15)

```javascript
class Device {
    constructor(type) {
        this.type = type;
    }
    
    info() {
        return `${this.type} device`;
    }
}

class Mobile extends Device {
    constructor(type, brand) {
        super(type);        // ← Device ka constructor call karo
        this.brand = brand;
    }
    
    info(brand = "type") {  // ← METHOD OVERRIDING
        return `${this.brand} is a ${this.type} device`;
    }
}

let d = new Device("Electronic");
let m = new Mobile("Electronic", "Samsung");

console.log(d.info());   // "Electronic device" ✅
console.log(m.info());   // "Samsung is a Electronic device" ✅

// FUNCTION REFERENCE vs FUNCTION CALL
let x = m.info;   // ← sirf reference, call nahi kiya
x();              // ❌ TypeError: this = undefined → runtime error
```

### 📊 Important Concepts

| Concept | Code | Explanation |
|---------|------|-------------|
| Object banana | `new Device("Electronic")` | Constructor chalega |
| Inheritance | `extends Device` | Parent ke sab methods milenge |
| Parent Constructor | `super(type)` | ZAROOR likhna hai child constructor me |
| Method Override | Child me same naam ka method | Child ka chalega, parent ka nahi |
| Function Reference | `let x = m.info` | Sirf reference, `this` bind nahi |
| Function Call | `m.info()` | `this = m`, safe |

### ⚠️ Function Reference Problem

```javascript
let x = m.info;  // Reference copy kiya
x();             // this = undefined (strict mode) → Error!

// Fix: bind use karo
let y = m.info.bind(m);
y();  // ✅ this = m → Safe
```

> **PYQ Q15 Answer:** `d.info()` → ✅, `m.info()` → ✅, `x()` → ✅ (runtime error), `d.constructor()` → ❌ (string return nahi karta)

---

## 8️⃣ Prototype Chain

### 🔑 Concept

JavaScript me **har object ek parent object se connected** hota hai. Isse **prototype chain** kehte hain.

### 💻 Example (PYQ Q8)

```javascript
const course = {
    courseName: 'Modern Application Development 2',
    courseCode: 'mad2',
};

const student = {
    __proto__: course,   // ← student, course se inherit karta hai
    studentName: 'Rakesh',
    studentCity: 'Delhi',
};

const { courseName } = student;  // Destructuring
console.log(courseName);  // "Modern Application Development 2" ✅
```

**Kyun kaam kiya?**  
`student` me directly `courseName` nahi tha.  
JavaScript ne prototype chain follow ki:  
```
student → __proto__ = course → courseName mila ✅
```

### 📊 Prototype Chain Diagram

```
student object
    │
    ├── studentName: "Rakesh"
    ├── studentCity: "Delhi"
    └── __proto__ → course object
                        │
                        ├── courseName: "MAD 2"
                        └── courseCode: "mad2"
```

---

## 9️⃣ Object Destructuring

### 🔑 Concept

Object se variables **shortcut me** nikalna.

```javascript
const { propertyName } = object;
// Ye equivalent hai:
// const propertyName = object.propertyName;
```

### 💻 Examples

```javascript
const person = { name: "Abhishek", age: 20, city: "Patna" };

// Normal way
const name = person.name;

// Destructuring
const { name, age, city } = person;

// Rename karo
const { name: myName } = person;  // myName = "Abhishek"

// Default value
const { phone = "N/A" } = person;  // phone = "N/A" (kyunki person me phone nahi)
```

---

## 🔟 Browser Storage

### 📊 Storage Types Comparison (PYQ Q6)

| Storage Type | Data Kab Tak Rehta Hai | Size | Har Request ke Saath Jaata Hai? |
|-------------|----------------------|------|--------------------------------|
| **Session Storage** | Tab/browser close hone tak | ~5MB | ❌ No |
| **localStorage** | Hamesha (manually delete karo) | ~5–10MB | ❌ No |
| **Cookie** | Expiry set karo | ~4KB | ✅ Yes |
| **Token Storage** | Depends on implementation | Varies | ❌ (manually bhejte hain) |

### 📌 PYQ Q6 Correct Matching

```
1. Session Storage → B: Data persists only until browser/tab closed
2. Token Storage   → C: Often used to store authentication tokens securely on client side
3. Cookie Storage  → D: Data persists across sessions, sent with every HTTP request
```

### 💻 localStorage Example (PYQ Q5)

```javascript
function addItemToCart(item) {
    let cart = JSON.parse(localStorage.getItem('cart')) || [];
    cart.push(item);
    localStorage.setItem('cart', JSON.stringify(cart));
}

function getCartItems() {
    return localStorage.getItem('cart') || [];
}

addItemToCart({ id: 1, name: 'Laptop' }.name);  // 'Laptop' push hua
console.log(getCartItems());
```

**PYQ Q5 Logic:**  
Page load hone par `addItemToCart` call hoti hai. Browser **2 baar refresh** kiya → total 3 baar run hua.

```
1st run: cart = [] → push 'Laptop' → cart = ['Laptop']
2nd run: cart = ['Laptop'] → push 'Laptop' → cart = ['Laptop', 'Laptop']
3rd run: cart = ['Laptop', 'Laptop'] → push 'Laptop' → cart = ['Laptop', 'Laptop', 'Laptop']
```

**Answer:** `['Laptop', 'Laptop', 'Laptop']` ✅

> **KEY:** `localStorage` **browser refresh ke baad bhi** data rakhta hai! Session storage nahi rakhta.

---

## 1️⃣1️⃣ Vue.js Basics

### 🔑 Vue Instance

```javascript
const app = new Vue({
    el: '#app',        // Kaunse HTML element ko control karo
    data: {            // Reactive data (variables)
        message: "Hello"
    },
    computed: {        // Calculated properties (cache hoti hain)
        reversedMsg() {
            return this.message.split('').reverse().join('');
        }
    },
    methods: {         // Functions
        greet() {
            alert("Hi!");
        }
    }
});
```

### 📊 data vs computed vs methods

| | `data` | `computed` | `methods` |
|---|--------|-----------|-----------|
| Kya hai | Raw variables | Calculated values | Functions |
| Cache hoti hai? | N/A | ✅ Yes | ❌ No |
| Kab update hoti hai | Manually | Dependencies change pe | Har call pe |
| Template me use | `{{ message }}` | `{{ reversedMsg }}` | `{{ greet() }}` |

---

## 1️⃣2️⃣ Vue.js Reactivity + `setInterval`

### 💻 PYQ Q7 — setInterval + Computed Property

```javascript
const app = new Vue({
    el: '#app',
    data: {
        principal: 0,
        annualInterestRate: 0,
        duration: 0,
        totalPayableAmount: 0,
    },
    computed: {
        simpleInterest() {
            return (this.principal * this.annualInterestRate * this.duration) / 100;
        }
    }
})

const appData = [
    [2000, 10, 2],   // index 0 (pop → last element pehle nikalta hai)
    [3000, 20, 3],   // index 1
    [5000, 30, 4],   // index 2
]

let handler = setInterval(() => {
    data = appData.pop()          // Array se last element nikalo
    app.principal = data[0]
    app.annualInterestRate = data[1]
    app.duration = data[2]
    app.totalPayableAmount += app.simpleInterest
}, 1000)
```

**4 seconds me kya hoga:**

```
t=1s: pop() → [5000, 30, 4]
      SI = (5000 × 30 × 4) / 100 = 6000
      total = 0 + 6000 = 6000

t=2s: pop() → [3000, 20, 3]
      SI = (3000 × 20 × 3) / 100 = 1800
      total = 6000 + 1800 = 7800

t=3s: pop() → [2000, 10, 2]
      SI = (2000 × 10 × 2) / 100 = 400
      total = 7800 + 400 = 8200

t=4s: pop() → undefined (array empty)
      data = undefined → data[0] errors → setInterval ruk jata hai
      (Ya undefined si values se total change nahi hota)
```

**Answer: 8200** ✅

> **KEY CONCEPT:** `array.pop()` → **last element** nikalta hai, first nahi!

---

## 1️⃣3️⃣ Vue.js Slots

### 🔑 Concept

**Slot** = Child component me **placeholder** hota hai, parent apna content wahan bhejta hai.

```
Parent → Content bhejta hai
Child  → <slot> me receive karta hai
```

### 📊 3 Types of Slots

| Type | Child me | Parent me | Use case |
|------|----------|-----------|----------|
| Default Slot | `<slot></slot>` | Normal HTML | Generic content |
| Named Slot | `<slot name="header">` | `<template v-slot:header>` | Specific placement |
| Fallback Content | `<slot>Default text</slot>` | Kuch nahi bheja | Backup content |

### 💻 Complete Example (PYQ Q13)

**Child Component (script.js):**
```html
<div class="container">
    <slot name="complete">
        <h3>Learned DBMS</h3>   <!-- Fallback content -->
    </slot>
    
    <slot>
        Exploring Frontend       <!-- Default slot, fallback -->
    </slot>
    
    <slot name="last"></slot>    <!-- Named slot, no fallback -->
</div>
```

**Parent (index.html):**
```html
<my-comp>
    <h3>Exploring Vue Js</h3>        <!-- Default slot content -->
    
    <template v-slot:last>            <!-- Named slot "last" content -->
        <h3>Learning App Dev 2</h3>
    </template>
</my-comp>
```

**Step-by-step Analysis:**

| Child Slot | Parent ne diya? | Output |
|-----------|----------------|--------|
| `name="complete"` | ❌ Nahi | Fallback: **Learned DBMS** |
| Default `<slot>` | ✅ `<h3>Exploring Vue Js</h3>` | **Exploring Vue Js** |
| `name="last"` | ✅ `<template v-slot:last>` | **Learning App Dev 2** |

**Final Browser Output:**
```
Learned DBMS
Exploring Vue Js
Learning App Dev 2
```

### 📌 Slot Rules (Yaad Rakho!)

```
Rule 1: Parent content diya → Parent content render hoga
Rule 2: Parent content nahi diya → Fallback render hoga
Rule 3: No fallback + no content → Kuch nahi dikhega
Rule 4: v-slot sirf <template> ya component par → kabhi <slot> par nahi
```

### ⚠️ Important Syntax

```html
<!-- ✅ CORRECT - Parent me named slot provide karna -->
<template v-slot:header>Content</template>
<template #header>Content</template>  <!-- shorthand -->

<!-- ❌ WRONG - v-slot kabhi <slot> par nahi likhte -->
<slot v-slot:header>Content</slot>
```

### 🎯 Exam Quick Reference Table

| Slot Type | Child | Parent | Output |
|-----------|-------|--------|--------|
| Default Slot | `<slot>Fallback</slot>` | `<comp>Hello</comp>` | Hello |
| Default Slot | `<slot>Fallback</slot>` | `<comp></comp>` | Fallback |
| Named Slot | `<slot name="x">Fallback</slot>` | `<template v-slot:x>Hi</template>` | Hi |
| Named Slot | `<slot name="x">Fallback</slot>` | (kuch nahi diya) | Fallback |

---

## 1️⃣4️⃣ Vue.js Lifecycle Hooks

### 🔑 Lifecycle Order

```
1. beforeCreate   → Vue instance bani, data/methods nahi
2. created        → Data/methods ready, DOM nahi
3. beforeMount    → DOM HTML parsed, Vue render nahi kiya
4. mounted        → DOM fully ready, Vue render complete
5. beforeUpdate   → Data change hua, DOM purana
6. updated        → DOM updated
7. beforeDestroy  → Component hatne se pehle
8. destroyed      → Component hat gaya
```

### 📊 Hook Comparison Table (PYQ Q16, Q17)

| Hook | Data Ready? | DOM Exists? | `{{}}` Replaced? | DOM Manipulation |
|------|------------|-------------|-----------------|-----------------|
| `beforeCreate` | ❌ | ❌ | ❌ | ❌ |
| `created` | ✅ | ❌ | ❌ | ❌ |
| `beforeMount` | ✅ | ✅ (HTML) | ❌ | ❌ |
| `mounted` | ✅ | ✅ | ✅ | ✅ |

### 💻 PYQ Q16-17 Example

```html
<!-- index.html -->
<div id="app">
    <p id="text">{{ message }}</p>
</div>
```

```javascript
// script.js
new Vue({
    el: "#app",
    data: { message: "Start" },
    
    beforeMount() {
        this.message = this.message + " A";   // message = "Start A"
        console.log("beforeMount:", this.message);  // "Start A"
        // DOM abhi bhi {{ message }} dikhata hai
    },
    
    mounted() {
        const el = document.getElementById("text");
        if (el) {
            el.innerText = el.innerText + " DOM";   // "Start A DOM"
            console.log("DOM text:", el.innerText);  // "Start A DOM"
        }
    }
});
```

**Step-by-step:**

```
Step 1: Browser HTML parse karta hai
        DOM: <p id="text">{{ message }}</p>

Step 2: beforeMount() chalti hai
        memory: message = "Start A"
        DOM: abhi bhi {{ message }} hai ← UNCHANGED

Step 3: Vue mount karta hai
        DOM: <p id="text">Start A</p>

Step 4: mounted() chalti hai
        el.innerText = "Start A" + " DOM" = "Start A DOM"
        Final DOM: <p id="text">Start A DOM</p>
```

**Q16 Answer:** `beforeMount` me `<p id="text">` exists karta hai but still `{{ message }}` show karta hai ✅

**Q17 Answer:** Final text = `Start A DOM` ✅

---

## 1️⃣5️⃣ UI State (Ephemeral State)

### 🔑 Concept

**UI State** = Interface ke temporary, short-lived elements.

### 📊 Types of State

| State Type | Example | Where Stored |
|-----------|---------|-------------|
| **UI State (Ephemeral)** | Loading spinner, selected tab, dropdown open/close | Component ke andar |
| **App State** | User login, cart items, user preferences | Global store |
| **Server State** | Database data (users, products, orders) | Server/DB |
| **URL State** | Current page, filters in URL | Browser URL |

### 💻 Example

```javascript
// UI State examples
data: {
    isLoading: false,        // ← Ephemeral (short-lived)
    activeTab: 'home',       // ← Ephemeral (UI ka ek state)
    isDropdownOpen: false,   // ← Ephemeral
    
    // These are NOT ephemeral:
    users: [],               // ← App/Server state
    currentUser: null,       // ← App state
}
```

### 🎯 PYQ Q10 Answer

> **"UI State (Ephemeral State)"** refers to **short-lived interface elements like loading indicators or selected tabs** ✅

---

## 1️⃣6️⃣ HTTP Stateless + State Management

### 🔑 HTTP Stateless Kya Hai?

```
Client ne Server ko request bheja → Server ne respond kiya
Server ko YAAD NAHI ki tune pehle request ki thi

Har request → Fresh start
```

### 📌 State Management Solutions

| Approach | Description |
|----------|-------------|
| **Cookies** | Client me store, har request ke saath server ko jaata hai |
| **Session** | Server me store, cookie me sirf Session ID |
| **JWT Token** | Client me store (localStorage/sessionStorage), manually header me bhejte hain |
| **State in Request** | Client explicitly state bhejta hai har request me |

### 🎯 PYQ Q11 Answer

> **"Client or server maintaining state and explicitly exchanging it through requests"** ✅

Kyunki HTTP stateless hai, isliye **client ya server manually state manage karta hai** aur har request me explicitly share karta hai.

---

## 🔥 Quick Revision — Exam Day Cheatsheet

### JavaScript

```
var    → function scope, hoisted (undefined)
let    → block scope, hoisted (TDZ)
const  → block scope, hoisted (TDZ), no reassign

var global → window.varName ✅
let global → window.letName ❌

count++  → pehle return, phir increment
++count  → pehle increment, phir return

Closure → inner function, outer ka variable yaad rakhta hai
IIFE    → (function(){ })() → turant execute

class A { }
class B extends A { }  → B inherits A
super() → A ka constructor call karo (B ke constructor me zaroor)

Method Reference: let x = obj.method  → this = undefined
Method Call:      obj.method()         → this = obj
```

### Vue.js

```
data     → reactive variables
computed → cached calculated values
methods  → functions

Slot Types:
  Default → <slot></slot>  ←→  <comp>content</comp>
  Named   → <slot name="x">  ←→  <template v-slot:x>
  Fallback → <slot>Fallback text</slot>

v-slot → sirf <template> ya component par (kabhi <slot> par nahi)
#name  → v-slot:name ka shorthand

Lifecycle Order:
beforeCreate → created → beforeMount → mounted
                                ↑
              beforeMount me: DOM hai, Vue render nahi hua, {{}} abhi hai
              mounted me:     DOM hai, Vue render hua, DOM manipulation possible
```

### Browser Storage

```
localStorage   → permanent, tab close ke baad bhi
sessionStorage → sirf current tab, close pe delete
Cookie         → server ko har request me jaata hai
Token          → manually bhejte hain header me
```

---

## 🧪 Practice Questions (Exam Style)

### Q1: Output kya hoga?
```javascript
let x = 2;
function op(x) {
    x *= 3;
    x += 4;
    x -= 1;
}
op(); op();
console.log(x);
```
<details>
<summary>Answer dekhne ke liye click karo</summary>

**Answer: 2**  
Kyunki `op(x)` me `x` local parameter hai. Global `x = 2` kabhi change nahi hua.  
`op()` bina argument ke call kiya → `x = undefined` inside function.
</details>

---

### Q2: Output kya hoga?
```javascript
function outer() {
    let count = 0;
    return function() { return count++; };
}
const f1 = outer(), f2 = outer();
console.log(f1(), f1(), f2());
```
<details>
<summary>Answer dekhne ke liye click karo</summary>

**Answer: 0, 1, 0**  
f1 aur f2 ke count alag-alag hain.
</details>

---

### Q3: Vue Slot Output kya hoga?
```html
<!-- Child -->
<slot name="title"><h1>Default Title</h1></slot>
<slot>Default Body</slot>

<!-- Parent -->
<my-comp>
    <template v-slot:title><h1>Custom Title</h1></template>
</my-comp>
```


---

 
*📅 Last Updated: July 2026*
