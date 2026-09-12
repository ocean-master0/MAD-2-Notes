# 📚 MAD 2 — Complete Theory Revision Notes
### Based on: Apr 2026 PYQ + Dec 2025 PYQ + Promise.md
> ⚡ Kal exam ke liye — Har concept covered, koi miss nahi!

---

## 📋 Table of Contents
1. [JavaScript — `this` Keyword](#1-javascript--this-keyword)
2. [JavaScript — call, apply, bind](#2-javascript--call-apply-bind)
3. [JavaScript — Arrow Functions vs Regular Functions](#3-javascript--arrow-functions-vs-regular-functions)
4. [JavaScript — ES6 Modules (import/export)](#4-javascript--es6-modules-importexport)
5. [JavaScript — Array Methods (reduce, filter, map)](#5-javascript--array-methods-reduce-filter-map)
6. [JavaScript — typeof, NaN, Equality](#6-javascript--typeof-nan-equality)
7. [JavaScript — Object Reference vs Value (Shallow/Deep Copy)](#7-javascript--object-reference-vs-value-shallowdeep-copy)
8. [JavaScript — Prototype Chain & Inheritance](#8-javascript--prototype-chain--inheritance)
9. [JavaScript — Browser Storage (localStorage vs sessionStorage)](#9-javascript--browser-storage-localstorage-vs-sessionstorage)
10. [Promises — Complete Guide](#10-promises--complete-guide)
11. [Async/Await + try/catch](#11-asyncawait--trycatch)
12. [Fetch API](#12-fetch-api)
13. [Microtask Queue vs Call Stack (Event Loop)](#13-microtask-queue-vs-call-stack-event-loop)
14. [Vue.js — Directives](#14-vuejs--directives)
15. [Vue.js — v-if vs v-show](#15-vuejs--v-if-vs-v-show)
16. [Vue.js — Lifecycle Hooks](#16-vuejs--lifecycle-hooks)
17. [Vue.js — Computed Properties](#17-vuejs--computed-properties)
18. [Vue.js — Watchers](#18-vuejs--watchers)
19. [Vue.js — Vue Router](#19-vuejs--vue-router)
20. [Vue.js — Component Communication ($emit, Props)](#20-vuejs--component-communication-emit-props)
21. [Vue.js — Vuex (State Management)](#21-vuejs--vuex-state-management)
22. [JWT Authentication](#22-jwt-authentication)
23. [Flask-JWT-Extended](#23-flask-jwt-extended)
24. [XSS (Cross-Site Scripting)](#24-xss-cross-site-scripting)
25. [CSRF & Cookie Security](#25-csrf--cookie-security)
26. [WebSockets](#26-websockets)
27. [Redis](#27-redis)
28. [Webhooks](#28-webhooks)
29. [Flask Caching (@cache.memoize vs @cache.cached)](#29-flask-caching-cachememoize-vs-cachecached)
30. [SPA — Client-Side vs Server-Side Routing](#30-spa--client-side-vs-server-side-routing)
31. [HTTP Status Codes](#31-http-status-codes)
32. [Git Commands](#32-git-commands)

---

## 1. JavaScript — `this` Keyword

### 4 Golden Rules

| Situation | `this` Kya Hoga? |
|---|---|
| Regular function — standalone call | `window` (browser) / `undefined` (strict mode) |
| Method call — `obj.func()` | Woh object jisne call kiya (`obj`) |
| Arrow function | **Enclosing scope ka `this`** (Lexical — freeze hota hai) |
| `new` keyword se | Naya bana hua object |

### Code Examples

```javascript
const user = {
  name: "Abhishek",
  
  // ✅ Regular Method — obj.method() → this = obj
  regularMethod: function() {
    console.log(this.name); // "Abhishek"
  },
  
  // ❌ Arrow as direct method — Anti-pattern!
  arrowMethod: () => {
    console.log(this.name); // undefined (window ka this liya!)
  },
  
  // ✅ Regular + Inner Arrow — Best Pattern!
  delayedGreet: function() {
    // this = user (method call)
    setTimeout(() => {
      // Arrow ne enclosing this (user) capture kiya!
      console.log(this.name); // "Abhishek" ✅
    }, 500);
  },
  
  // ❌ Regular + Inner Regular — Broken!
  delayedBroken: function() {
    setTimeout(function() {
      // Naya regular function = naya this = window
      console.log(this.name); // undefined ❌
    }, 500);
  }
};
```

### ⚠️ Exam Traps

- ❌ "Arrow function ka `this` = global object" → **GALAT** — enclosing scope ka hota hai
- ❌ "Regular function ka `this` = owner object" → **PARTIAL** — sirf method call mein, standalone call mein `window` hota hai
- ❌ "`this` always points to window in browsers" → **GALAT** — "always" = red flag!
- ✅ Arrow function ka `this` = **Lexical `this`** — jab define hua tab ka `this` **permanently freeze** ho jaata hai

---

## 2. JavaScript — call, apply, bind

### Comparison Table

| Method | Kya Karta Hai | Turant Execute? | Args Kaise? |
|---|---|---|---|
| `call(obj, a, b)` | `this = obj`, turant execute | ✅ Haan | Comma se individual |
| `apply(obj, [a, b])` | `this = obj`, turant execute | ✅ Haan | Array mein |
| `bind(obj, a)` | `this = obj` fix karta hai, **naya function return** | ❌ Nahi | Comma se (partial application) |

### Code Example (Dec 2025 PYQ Q1)

```javascript
const calculator = {
  multiplier: 5,
  calculate: function(a, b) {
    console.log(`Result: ${(a + b) * this.multiplier}`);
    return (a + b) * this.multiplier;
  }
};

const advancedCalc = { multiplier: 10, bonus: 20 };

// .call() — turant execute, this = advancedCalc
const operation1 = calculator.calculate.call(advancedCalc, 3, 5);
// (3+5) * 10 = 80 → prints "Result: 80"
// operation1 = 80

// .bind() — naya function return karta hai, turant execute NAHI karta
const operation2 = calculator.calculate.bind(advancedCalc, 4);
// a = 4 "pre-loaded"

operation2(8);
// b = 8, (4+8) * 10 = 120 → prints "Result: 120"

console.log(operation1);       // 80
console.log(typeof operation2); // "function" (bind function return karta hai!)
```

### Output:
```
Result: 80
Result: 120
80
function
```

### ⚠️ Exam Traps
- ❌ `bind()` ko `call()` jaisa turant-execute nahi karta — ye **function return** karta hai
- ❌ `typeof bind()` = `"object"` nahi, **`"function"`** hota hai
- ❌ `this.multiplier` = 5 nahi, `advancedCalc.multiplier = 10` (call/bind ne change kiya)
- **"Partial Application"** = `bind()` ka feature — kuch args pehle se fix karo, baaki baad mein

---

## 3. JavaScript — Arrow Functions vs Regular Functions

### Key Differences

| Feature | Regular Function | Arrow Function |
|---|---|---|
| Apna `this`? | ✅ Haan (runtime decide hota) | ❌ Nahi (lexical inherit) |
| `arguments` object | ✅ Available | ❌ Nahi |
| Constructor ke roop mein? | ✅ `new` se bana sakte | ❌ Error aayega |
| Method ke roop mein? | ✅ Recommended | ❌ Anti-pattern (this = window) |
| Callbacks mein? | ⚠️ this lose ho sakta | ✅ Perfect (this capture karta hai) |

### Arrow Function ka Lexical This — Deep Dive

```javascript
const obj = {
  num: 50,
  
  // outerMethod regular hai — obj.outerMethod() → this = obj
  outerMethod: function() {
    const innerArrow = () => {
      // Arrow ne enclosing this (obj) capture kiya!
      console.log(this.num); // 50 jab method se call ho
    };
    return innerArrow;
  }
};

// f1 — outerMethod ko METHOD call se run kiya → this = obj
const f1 = obj.outerMethod(); // this = obj, arrow ne obj capture kiya
f1(); // 50 ✅

// f2 — sirf reference, call nahi kiya
const f2 = obj.outerMethod;
// f2() — standalone call → this = window
// innerArrow ne window capture kiya!
f2()(); // undefined ❌ (window.num nahi hai)

// f3 — regular method, standalone call
const f3 = obj.anotherMethod;
f3(); // undefined ❌ (standalone → this = window)
```

> 🎯 **Key Insight:** Arrow function ka `this` **frozen at creation time** — jab bhi, jahan bhi call karo, same `this` milega

---

## 4. JavaScript — ES6 Modules (import/export)

### Export Types

```javascript
// ════ battingStats.js ════

// NAMED EXPORT — multiple ho sakte hain
export const strikeRate = (runs, balls) => (runs / balls) * 100;
export const playerName = "Virat Kohli";

// DEFAULT EXPORT — sirf ek per file
export default function calculator() { ... }
```

### Import Rules

```javascript
// Named exports → CURLY BRACES zaroori!
import { strikeRate, playerName } from "./battingStats.js";

// Default export → NO curly braces
import calculator from "./module.js";

// Default + Named (combo)
import calculator, { helper } from "./module.js";

// Namespace import (sab kuch as object)
import * as stats from "./battingStats.js";
// Usage: stats.strikeRate(...), stats.playerName

// CommonJS style — ES6 mein INVALID!
const m = require("./module.js"); // ❌ Browser ES6 mein nahi chalega!
```

### ⚠️ Exam Traps
- ❌ Named imports ke liye curly braces bhool jaana
- ❌ Comma se direct multiple imports: `import a, b from "..."` → **INVALID** (a = default, b = invalid)
- ❌ `import *` — kaam karta hai lekin `stats.` prefix ke saath access karna padega
- ❌ `require()` — CommonJS hai, ES Modules mein kaam nahi karta

---

## 5. JavaScript — Array Methods (reduce, filter, map)

### `reduce()` — Sabse Important

```javascript
// Syntax
array.reduce((acc, val, idx, array) => expression, initialValue)
//            └── running total  └── current element

// Example
let arr = [2, 3, 4];
let sum = arr.reduce((acc, val, idx, array) => acc + val, 0);
// Step 1: acc=0, val=2 → 0+2 = 2
// Step 2: acc=2, val=3 → 2+3 = 5
// Step 3: acc=5, val=4 → 5+4 = 9
console.log(sum); // 9
```

> **Trap:** `idx` aur `array` parameters dikhaye jaate hain to confuse karne ke liye — sirf `acc + val` hota hai, index add nahi hota!

### `filter()` — Condition-based Filtering

```javascript
const products = [{name: "Laptop", cat: "electronics"}, {name: "Novel", cat: "books"}];
const elec = products.filter(p => p.cat === "electronics");
// [{name: "Laptop", cat: "electronics"}]
```

---

## 6. JavaScript — typeof, NaN, Equality

### `typeof` Results

```javascript
typeof undefined    // "undefined"
typeof null         // "object" (⚠️ JS bug!)
typeof NaN          // "number" (⚠️ Trap!)
typeof []           // "object"
typeof function(){} // "function"
typeof 42           // "number"
typeof "hello"      // "string"
typeof true         // "boolean"
```

### NaN — Special Rules

```javascript
// NaN ka type "number" hai — counterintuitive!
typeof NaN === "number" // true

// NaN kabhi bhi kisi se equal nahi — khud se bhi nahi!
NaN === NaN  // false
NaN == NaN   // false

// Check karne ka sahi tarika:
isNaN(NaN)    // true
Number.isNaN(NaN) // true (better — type coerce nahi karta)
```

### Equality Rules

```javascript
// == (Loose) vs === (Strict)
undefined == null   // true (special rule!)
undefined == NaN    // false
undefined === NaN   // false (type bhi alag hai)

// undefined ka special rule:
// undefined sirf null ke saath loosely equal hota hai
// kisi aur cheez ke saath nahi — chahe type coercion allow ho!
```

### Apr 2026 PYQ Q10 Output:
```javascript
console.log(typeof undefined); // "undefined"
console.log(typeof NaN);       // "number"
console.log(undefined == NaN); // false
console.log(undefined === NaN);// false
```

---

## 7. JavaScript — Object Reference vs Value (Shallow/Deep Copy)

### Core Concept

| Type | Copy Behavior |
|---|---|
| **Primitives** (number, string, boolean) | By **Value** — independent copies |
| **Objects/Arrays** | By **Reference** — same memory address |

### Three Types of Assignment/Copy

```javascript
const item = { name: "Sword", stats: { damage: 15 } };

// ─── Type 1: Reference Assignment (NO COPY) ───
const ref1 = item;
// ref1 aur item = SAME object (Box A)
// ref1.name = "Axe" → item.name bhi "Axe" ho jayega!

// ─── Type 2: Shallow Copy (Spread Operator) ───
const ref2 = { ...item };
// ref2 = NAYA outer object (Box C) ✅
// ref2.name alag hai ("Sword" copy)
// ref2.stats = item.stats ka SAME reference! ⚠️
// ref2.stats.damage = 25 → item.stats.damage bhi 25 ho jayega!

// ─── Type 3: Deep Copy (Nested Spread) ───
const ref3 = { name: item.name, stats: { ...item.stats } };
// ref3.stats = BILKUL NAYA object (Box D) ✅
// ref3.stats.damage = 35 → item pe koi effect NAHI!
```

### Visual Memory Diagram

```
item ──► Object#A { name:"Sword", stats: ──► Object#B{damage:15} }
              ↑                                      ↑
ref1 ─────────┘                           (SAME, shared)
                                                     ↑
ref2 ──► Object#C { name:"Sword", stats: ────────────┘ } (SHALLOW!)

ref3 ──► Object#E { name:"Sword", stats: ──► Object#D{damage:15} } (DEEP!)
```

### Dec 2025 PYQ Q20 — Object Reassignment

```javascript
let originalConfig = { apiUrl: 'https://api.example.com', timeout: 5000 };
let configCopy = originalConfig; // SAME object!

configCopy.timeout = 10000; // Object#A modify hua → originalConfig bhi affect!

configCopy = { apiUrl: 'https://api2.example.com', timeout: 3000 }; // NAYA object → configCopy alag
// originalConfig ABHI BHI Box A ko point karta hai — UNAFFECTED!

console.log(originalConfig.timeout); // 10000 ✅
```

### ⚠️ Exam Traps
- ❌ `{...obj}` = "poori tarah independent copy" → **GALAT** — sirf top-level!
- ❌ Nested objects reference share karte hain spread ke baad
- ❌ Variable reassignment (`=`) vs property mutation (`.prop =`) ka difference

---

## 8. JavaScript — Prototype Chain & Inheritance

### ES6 Class Syntax

```javascript
class BaseComponent {
  constructor(name) {
    this.name = name;
  }
}

class AdvancedComponent extends BaseComponent {
  constructor(name, role) {
    super(name); // Parent constructor call ZAROORI hai!
    this.role = role;
  }
}

const comp = new AdvancedComponent('Alice', 'admin');
// comp = { name: 'Alice', role: 'admin' }
```

### `instanceof` — Prototype Chain Check

```javascript
comp instanceof AdvancedComponent  // true (direct)
comp instanceof BaseComponent      // true (ancestor chain mein hai!)
comp instanceof Object             // true (hamesha)
```

### Prototype Chain Visual

```
comp.__proto__ → AdvancedComponent.prototype
                          ↓ __proto__
               BaseComponent.prototype
                          ↓ __proto__
                    Object.prototype
                          ↓ __proto__
                         null
```

> **`instanceof` ka rule:** Poori chain check karta hai — sirf direct constructor nahi!

### ⚠️ Exam Traps
- ❌ `instanceof` sirf direct constructor match karta hai — **GALAT** — ancestors bhi check hote hain
- ❌ `class` ko prototype system se alag samajhna — class **prototype ke upar syntactic sugar** hai
- ❌ `__proto__` (instance) aur `.prototype` (constructor) ko confuse karna

---

## 9. JavaScript — Browser Storage (localStorage vs sessionStorage)

### Comparison

| Feature | `localStorage` | `sessionStorage` |
|---|---|---|
| Persistence | Permanently (browser clear na karo tab tak) | Tab close hone pe delete |
| Scope | Same origin — sab tabs mein | Sirf usi tab mein |
| Expiry | Manual clear ya code se delete | Tab close pe automatic |
| Capacity | ~5-10MB | ~5MB |

### Code

```javascript
// Set
localStorage.setItem('theme', 'dark');

// Get
const theme = localStorage.getItem('theme'); // "dark"

// Remove
localStorage.removeItem('theme');

// Clear all
localStorage.clear();
```

> **Exam Point:** Page refresh, tab close, browser restart — `localStorage` ka data **SURVIVE** karta hai. Sirf manual clear ya code se delete hoga.

### ⚠️ Exam Traps
- ❌ localStorage aur sessionStorage same hain — **ALAG** hain persistence mein
- ❌ Refresh se localStorage clear hota hai — **NAHI** hota!
- ❌ localStorage mein expiry hoti hai — **NAHI** hoti (cookies mein hoti hai)
- ⚠️ Incognito mode mein localStorage session end pe clear ho sakti hai

---

## 10. Promises — Complete Guide

### Promise States

```
Promise
├── Pending    → Abhi decide nahi hua
├── Fulfilled  → resolve() call hua ✅
└── Rejected   → reject() call hua ❌
```

### Golden Rules

1. **Promise ek baar settle hota hai** — pehla call (resolve ya reject) wins, baad wale **IGNORE** hote hain
2. **Executor function synchronously** chalta hai
3. **Position matter karta hai, naam nahi** — `new Promise((error, pass) => ...)` mein `error` = RESOLVE, `pass` = REJECT!

```javascript
const promise = new Promise((resolve, reject) => {
  reject('Failed!');
  resolve('Done!'); // ← IGNORE hoga! Promise already settled hai
});
promise.then(console.log).catch(console.log); // "Failed!"
```

### `.then()` ke Teen Forms

```javascript
// Form 1: Sirf onFulfilled
.then(fn)
// Chalega jab: ✅ Resolved
// Skip hoga jab: ❌ Rejected

// Form 2: onFulfilled + onRejected
.then(fn1, fn2)
// fn1 chalega: ✅ Resolved
// fn2 chalega: ❌ Rejected
// DONO KABHI SAATH NAHI CHALTE!

// Form 3: Sirf onRejected (rare)
.then(null, fn)
// = .catch(fn) jaisa hi hai
```

### `.catch()` — Shorthand for `.then(null, fn)`

```javascript
.catch(fn)
// = .then(null, fn)
// Sirf REJECTED promise pe chalta hai
// Agar catch mein return karo → Promise RESOLVED ho jaata hai!
```

### `.finally()` — HAMESHA Chalta Hai

```javascript
.finally(fn)
// ✅ Resolved pe bhi chalta hai
// ✅ Rejected pe bhi chalta hai
// ⚠️ fn ko KOI VALUE NAHI MILTI → d = undefined HAMESHA!
// ⚠️ finally ka return value IGNORE hota hai
// ✅ Pehle wali value aage pass hoti hai (unchanged)
```

### Promise Chain — Dec 2025 PYQ Q13

```javascript
const p = Promise.resolve(1)
  .then(v => { console.log("A", v); return v + 1; })
  // v=1, prints "A 1", returns 2 → RESOLVED(2)
  
  .then(v => { console.log("B", v); throw "err"; })
  // v=2, prints "B 2", throws → REJECTED("err")
  
  .catch(e => { console.log("C", e); return 10; })
  // e="err", prints "C err", returns 10 → RESOLVED(10)
  
  .then(v => console.log("D", v));
  // v=10, prints "D 10"

console.log("END"); // SYNC CODE — PEHLE CHALTA HAI!
```

**Output:**
```
END    ← Sync code pehle
A 1   ← then chain
B 2
C err
D 10
```

### Promise + Reversed Parameter Names (Apr 2026 — Promise.md)

```javascript
new Promise((error, pass) => {
  // error = RESOLVE (1st position)
  // pass  = REJECT  (2nd position)
  if (5 === "5") error(5)  // 5 === "5" → FALSE (strict equality, types alag!)
  else pass(8)              // REJECT(8) ho gaya!
})
.then(d => { /* skip — rejected */ })
.then(d => { /* fn1 skip */ }, d => {
  console.log("Checkpoint 5", d.message); // d=8 (number), 8.message=undefined
  return undefined * 2; // NaN → RESOLVED(NaN)
})
.catch(e => { /* skip — resolved */ })
.finally(d => {
  console.log("Checkpoint 1", d); // d=undefined (finally ko value nahi milti!)
  return d * 5; // ignored!
})
.then(d => {
  console.log("Checkpoint 6", d); // d=NaN (NaN pass hoti rahi)
});
```

**Output:**
```
Checkpoint 5 undefined
Checkpoint 1 undefined
Checkpoint 6 NaN
```

### Promise Chaining — Nested Promise (Dec 2025 PYQ Q19)

```javascript
const processString = (str) => {
  return new Promise((resolve) => {
    setTimeout(() => resolve(str.toUpperCase()), 100);
  });
};

Promise.resolve('hello')
  .then(processString)          // 'hello' → processString returns NEW Promise
                                // JS auto-unwraps → waits for it → 'HELLO'
  .then(result => result + ' WORLD') // 'HELLO' + ' WORLD' = 'HELLO WORLD'
  .then(console.log);           // "HELLO WORLD"
```

> **Key Rule:** Jab `.then()` ke andar se **Promise return** ho, JS **automatically wait** karta hai us Promise ke resolve hone ka (Promise Flattening/Unwrapping). Agla `.then()` resolved value milega, Promise object nahi.

### ⚠️ Critical Promise Exam Points

| Rule | Detail |
|---|---|
| Position = Role | (resolve, reject) mein naam nahi, position matter karta |
| First wins | reject ke baad resolve = ignored |
| catch return = resolve | `.catch()` mein return karo → next `.then()` chalega |
| finally gets undefined | `.finally(d => ...)` mein d hamesha undefined |
| finally return ignored | `.finally()` ka return value chain pe effect nahi |
| Nested Promise flattens | `.then()` mein Promise return → auto-unwrap |

---

## 11. Async/Await + try/catch

### Pattern

```javascript
async function runAllocation() {
  try {
    const result = await allocateSlots(); // Promise resolve hone ka wait
    console.log("Passed " + JSON.stringify(result));
  } catch (error) {
    // Agar await wala Promise REJECT ho → control yahan aata hai
    console.log("Failed " + JSON.stringify(error));
  }
}
```

> **Rule:** `await` kisi rejected Promise pe = **exception throw** → `catch` block mein jata hai. `reject()` value directly `catch(error)` mein milti hai.

### Apr 2026 PYQ Q14 Example

```javascript
function allocateSlots() {
  return new Promise((resolve, reject) => {
    let slots = [80, 80, 30]; // calculation sahi hai
    reject(slots);  // ← REJECT! Chahe value valid ho
  });
}

async function runAllocation() {
  try {
    const result = await allocateSlots();
    console.log("Passed " + JSON.stringify(result)); // skip!
  } catch (error) {
    console.log("Failed " + JSON.stringify(error)); // "Failed [80,80,30]"
  }
}
```

> ⚠️ **Golden Rule:** `resolve` ya `reject` — ye decide karta hai "passed" ya "failed", value kya hai ye nahi!

---

## 12. Fetch API

### Critical Rule — When Does `fetch()` Reject?

| Scenario | Promise Behavior |
|---|---|
| Network failure (no internet, DNS fail, CORS block) | **REJECTS** ❌ |
| Server responds with **ANY** HTTP status (200, 404, 500) | **RESOLVES** ✅ |

### `response.ok`

```javascript
response.ok = (status >= 200 && status < 300)
// true  → 200, 201, 204...
// false → 404, 500, 403, 401...
```

### Correct Error Handling Pattern

```javascript
fetch(url)
  .then(response => {
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return response.json();
  })
  .catch(error => console.log('Error:', error));
// Ab 404 bhi catch hoga!
```

### ⚠️ Exam Trap
- ❌ "404 pe Promise reject hota hai" — **GALAT!** Resolve hota hai, `response.ok = false`
- ❌ `.catch()` pe depend karna 404 ke liye — **GALAT!** manually check karo `response.ok`

### Parallel Fetch (Apr 2026 PYQ Q23)

```javascript
// Dono requests EK SAATH fire hoti hain (no await!)
const res1 = fetch('http://127.0.0.1:5000');         // 10s delay
const res2 = fetch('http://127.0.0.1:5000/profile'); // 30s delay

// 10s baad → res1 resolve hota hai, data print
res1.then(r => r.json()).then(data => console.log(data)); // prints first

// 30s baad → res2 resolve hota hai, data print
res2.then(r => r.json()).then(data => console.log(data)); // prints second
```

> **Key:** `fetch()` non-blocking hai — `await` ke bina dono requests simultaneously fire hoti hain!

---

## 13. Microtask Queue vs Call Stack (Event Loop)

### Execution Order

```
1st → Synchronous code (Call Stack) — HAMESHA PEHLE
2nd → Microtask Queue (Promise .then/.catch callbacks)
3rd → Macrotask Queue (setTimeout, setInterval, setImmediate)
```

### Why "END" Pehle Aata Hai

```javascript
Promise.resolve(1)
  .then(() => console.log("A")); // ← Microtask queue mein jaata hai

console.log("END"); // ← Sync code — TURANT CHAL JATA HAI
```

**Output:** `END` → `A`

> **Rule:** Chahe Promise already resolved ho, `.then()` callback **hamesha sync code ke BAAD** chalega!

---

## 14. Vue.js — Directives

### Complete Table

| Directive | Purpose | Use Case |
|---|---|---|
| `v-if` | Conditionally DOM se add/remove | Heavy elements jo rarely dikhaate ho |
| `v-else-if` / `v-else` | if-else chain | Conditions ke saath |
| `v-show` | CSS display:none toggle | Frequently toggle hone wale elements |
| `v-for` | List render karna | Arrays/objects loop |
| `v-bind` (`:`) | HTML attribute ko dynamically bind | `:href`, `:src`, `:class` |
| `v-model` | Two-way data binding | Input forms |
| `v-on` (`@`) | Event listener attach karna | `@click`, `@submit` |
| `v-html` | Raw HTML render karna | ⚠️ XSS risk hai! |
| `v-once` | Sirf ek baar render | Static content optimization |

### Apr 2026 PYQ Q15 — Match Karo

```
v-if   → B. Conditionally renders DOM elements
v-for  → D. Renders a list of items
v-bind → C. Binds HTML attributes dynamically
v-model→ A. Two-way binding between input and data
```

### Examples

```html
<p v-if="loggedIn">Welcome!</p>
<li v-for="user in users" :key="user.id">{{ user.name }}</li>
<img :src="imageUrl">        <!-- v-bind shorthand → : -->
<input v-model="username">   <!-- two-way binding -->
<button @click="submit">OK</button>  <!-- v-on shorthand → @ -->
```

---

## 15. Vue.js — v-if vs v-show

### Critical Difference

| Feature | `v-if` | `v-show` |
|---|---|---|
| DOM pe effect | Element **DOM se remove** hota hai | Element **DOM mein rahta** hai |
| CSS | Koi CSS nahi | `display: none` lagata hai |
| Lifecycle hooks | ✅ Trigger hote hain (create/destroy) | ❌ Trigger nahi hote |
| Performance | Heavy toggle → costly | Frequent toggle → better |
| Initial render | Lazy (sirf tab render jab condition true) | Hamesha render hota hai |

```html
<!-- v-if = DOM se remove/add -->
<div v-if="show">Hello</div>
<!-- show=false → Element DOM mein NAHI hai -->

<!-- v-show = CSS toggle -->
<div v-show="show">Hello</div>
<!-- show=false → <div style="display: none;">Hello</div> -->
```

### ⚠️ Exam Trap
- ❌ "v-show removes from DOM" → **GALAT** — sirf CSS change karta hai
- ❌ "v-if re-mounts lifecycle hooks every time" → ✅ Actually does! Creation/destruction hoti hai

---

## 16. Vue.js — Lifecycle Hooks

### Sequence

```
beforeCreate → created → beforeMount → mounted
                                          ↓
                              (Data changes)
                          beforeUpdate → updated
                                          ↓
                              (Component destroy)
                         beforeDestroy → destroyed
```

### Important Hooks

| Hook | Kab Chalta Hai |
|---|---|
| `created()` | Component instance bana, data/events ready. DOM nahi bana abhi. **First time sirf.** |
| `mounted()` | Component DOM mein insert ho gaya. DOM access possible. |
| `updated()` | Reactive data change + DOM re-render ke **baad**. |
| `destroyed()` | Component destroy hone ke baad cleanup. |

### Component Reuse (Vue Router ke saath)

```javascript
// ❗ Same route pattern pe navigate karne se component REUSE hota hai, recreate NAHI!
// /product/1 → /product/2 (same :id pattern)
// → created() NAHI chalta (naya component nahi bana)
// → updated() CHALTA HAI (reactive data = $route.params.id change hua)
```

### Apr 2026 PYQ Q32-Q33

```javascript
const Product = {
  template: `<p>Product ID: {{ $route.params.id }}</p>`,
  created()  { console.log("created hook triggered"); },
  updated()  { console.log("updated hook triggered"); }
};

// Page load on /product/1 → created() runs ✅, updated() nahi
// Navigate to /product/2 → created() NAHI (reuse!), updated() RUNS ✅
// WHY updated() runs: $route.params.id reactive hai, change hone se
//   Vue reactivity system → DOM re-render → updated() trigger!
```

> **Root Cause Chain:** Route param change → reactive value change → DOM update → `updated()` fires
> **URL change se directly nahi — reactive data change se!**

---

## 17. Vue.js — Computed Properties

### Theory

```
Computed Properties = Smart, Cached Properties
- Dependencies track karta hai automatically
- Sirf tab re-run hota hai jab dependencies change hoti hain
- Otherwise: CACHED value return karta hai
```

### Computed vs Methods

| | Computed | Methods |
|---|---|---|
| Cache hota hai? | ✅ Haan | ❌ Nahi |
| Kab run hota hai? | Dependency change pe | Har re-render pe |
| Use karo jab | Expensive calculation | Side effects, event handlers |

### Code Example

```javascript
new Vue({
  data: { x: 1, y: 2, time: 0 },
  
  computed: {
    bigValue() {
      console.log("computed run"); // Kitni baar chala? Track karo!
      let sum = 0;
      for (let i = 0; i < 10000000; i++) sum += i; // heavy!
      return sum + this.x + this.y; // Dependencies: x, y
    }
  },
  
  methods: {
    calculate() {
      this.time = Date.now(); // time change kiya
      // bigValue ka dependency = x, y (NOT time!)
      // Isliye bigValue DOBARA NAHI CHALEGA!
    }
  }
});
```

### Apr 2026 PYQ Q29 — Initial Render

```
Page load → bigValue() → 1 baar runs ("computed run" print)
→ Result cached
→ Calculate button click → time changes, bigValue dependency nahi
→ bigValue DOBARA NAHI CHALTA! ("computed run" dubara nahi)
```

### Apr 2026 PYQ Q30 — Dependency Change

```javascript
calculate() {
  this.y = this.y + 1; // y bigValue ki dependency hai!
}
// Button click → y changes → Vue invalidates bigValue cache
// → bigValue DOBARA CHALTA HAI!
```

### Computed + Route Params

```javascript
computed: {
  filteredProducts() {
    return this.allProducts.filter(
      p => p.category === this.$route.params.category // DEPENDENCY!
    );
  }
}
// Route change → $route.params.category change → computed re-evaluates!
```

---

## 18. Vue.js — Watchers

### Theory

```javascript
watch: {
  // Property ko string ke roop mein watch karo
  '$route.params.category': function(newVal, oldVal) {
    // Jab bhi category change ho, yeh callback chale
    this.displayedProducts = this.filteredProducts;
  },
  
  // Deep watch karne ke liye:
  someObject: {
    handler(newVal) { ... },
    deep: true,      // nested changes bhi track karo
    immediate: true  // component create hote hi ek baar run karo
  }
}
```

### Watcher vs Computed

| | Watcher | Computed |
|---|---|---|
| Side effects? | ✅ Suitable (API calls, mutations) | ❌ Avoid |
| Return value? | ❌ Nahi (side effects ke liye) | ✅ Haan |
| Kab use karo | Async ops, external changes on data change | Derived data |

### Initial Mount vs Navigation (Dec 2025 PYQ Q34 vs Q35)

```
Initial Mount (Q34):
- created() runs ✅
- Watcher: immediate:false → watch callback NAHI chalta pehli baar
- Computed: template mein use ho to evaluate hota hai ✅

Navigation (Q35):
- created() NAHI chalta (same component reused)
- Watcher: CHALTA HAI (actual change event hai!) ✅
- updated() CHALTA HAI ✅
- Computed: dependency change → auto re-evaluate ✅
```

---

## 19. Vue.js — Vue Router

### Basic Setup

```javascript
const Home = { template: '<div>Home</div>' };
const Contact = { template: '<div>Contact</div>' };

const routes = [
  { path: '/home', component: Home },
  { path: '/contact', component: Contact },
  { path: '*', redirect: '/contact' } // Wildcard/Catch-all
];

const router = new VueRouter({ routes });
new Vue({ el: '#app', router });
```

### HTML Template

```html
<div id="app">
  <!-- Router link = navigation (nahi reload karta) -->
  <router-link to="/contact">Go to Contact</router-link>
  
  <!-- Router view = matched component yahan render hoga -->
  <router-view></router-view>
</div>
```

### Wildcard Route (Apr 2026 PYQ Q9)

```javascript
{ path: '*', redirect: '/contact' }
// * = Catch-all — koi bhi undefined route match karega
// /unknown → * matches → redirect to /contact → "Contact" dikhega
```

> **Matching Order:** Top to bottom — pehle exact match, phir wildcard

### Route Params

```javascript
const routes = [
  { path: '/product/:id', component: Product }
];

// Access karo:
this.$route.params.id // "1" for /product/1

// $route.params.id REACTIVE hai!
// → Template mein use karo → dependency track hogi
// → Param change → DOM update → updated() fire
```

### Component Reuse Rule

```
Same route PATTERN ke liye (/product/:id):
  /product/1 → /product/2
  → Component REUSE hota hai (created() NAHI chalta)
  → Only updated() chalta hai

DIFFERENT pattern ke liye:
  /home → /contact
  → Component DESTROY + CREATE hota hai (created() chalta hai)
```

---

## 20. Vue.js — Component Communication ($emit, Props)

### Parent → Child: Props

```javascript
// Parent template
<ChildComponent :message="parentMsg" :count="5" />

// Child component
props: ['message', 'count']
// Ya validation ke saath:
props: {
  message: { type: String, required: true },
  count: { type: Number, default: 0 }
}
```

### Child → Parent: $emit

```javascript
// Child mein:
this.$emit('update', 5); // 'update' event fire karo, payload = 5

// Parent template mein:
<ChildComponent @update="handleUpdate" />

// Parent methods:
methods: {
  handleUpdate(value) {
    // value = 5 (payload)
    console.log(value); // 5
  }
}

// Ya direct $event se:
<ChildComponent @update="parentData = $event" />
// $event = payload (5)
```

### Dec 2025 PYQ Q2 — True Statements

```javascript
childComponent.$emit('update', 5)
// ✅ Parent can listen with @update
// ✅ Parent receives payload via $event
// ❌ Child can directly modify parent state — GALAT (props read-only!)
// ❌ $emit triggers a DOM event — GALAT (custom Vue event, DOM event nahi)
```

---

## 21. Vue.js — Vuex (State Management)

### Four Core Concepts

```javascript
const store = new Vuex.Store({
  
  // STATE — Single source of truth
  state: {
    items: ['React', 'Angular']
  },
  
  // GETTERS — Computed properties for store
  getters: {
    itemCount: state => state.items.length
  },
  
  // MUTATIONS — Synchronously state change karte hain
  // SIRF YEH STATE CHANGE KAR SAKTE HAIN
  mutations: {
    addItem(state, item) {
      state.items.push(item); // Synchronous!
    }
  },
  
  // ACTIONS — Async operations handle karte hain
  // MUTATIONS ko COMMIT karte hain, khud state touch nahi karte
  actions: {
    addItemAsync({ commit }, item) {
      setTimeout(() => {
        commit('addItem', item); // 500ms baad mutation commit
      }, 500);
    }
  }
});
```

### Flow Diagram

```
Component
  │
  │ dispatch('addItemAsync', 'Vue')
  ↓
Action (async allowed)
  │
  │ commit('addItem', 'Vue')
  ↓
Mutation (synchronous only)
  │
  │ state.items.push('Vue')
  ↓
State Updated → Vue re-renders
```

### Usage from Component

```javascript
// Actions dispatch karo
this.$store.dispatch('addItemAsync', 'Vue');

// Mutations commit karo (directly possible lekin avoid karo)
this.$store.commit('addItem', 'NewItem');

// State access karo
this.$store.state.items;

// Getters access karo
this.$store.getters.itemCount;
```

### Vuex True Statements (Dec 2025 PYQ Q10)

```
State should be modified ONLY through mutations ✅
State should be modified only through actions ❌ (actions mutations commit karte hain)
State cannot contain nested objects ❌ (can contain complex data)
Getters modify state ❌ (getters sirf read karte hain)
```

### Apr 2026 PYQ Q8 — State After 2 Seconds

```javascript
// Initial: items = ['React', 'Angular']
// dispatch('addItemAsync', 'Vue') → setTimeout 500ms → commit('addItem', 'Vue')
// After 2 seconds (2000ms > 500ms):
// items = ['React', 'Angular', 'Vue'] ✅
```

---

## 22. JWT Authentication

### JWT Structure

```
eyJhbGciOiJIUzI1NiJ9 . eyJ1c2VyIjoiQWJoaSJ9 . SflKxwRJSMeKKF2QT4...
      │                          │                         │
   HEADER                    PAYLOAD                  SIGNATURE
(Algorithm info)         (User Claims: id,          (HMACSHA256 hash)
                          role, exp time)
```

### JWT vs Session-Based Auth

| Feature | Session-Based | JWT |
|---|---|---|
| Server Storage | ✅ DB/Memory mein session | ❌ Kuch store nahi (sirf secret key) |
| Stateful/Stateless | Stateful | **Stateless** |
| Scalability | Limited (sticky sessions) | Excellent |
| Token size | Small (session ID) | Larger (data inside) |
| Expiry | Server manages | Manually set karna padta hai |
| Revocation | Easy | Difficult (blacklist chahiye) |

### JWT Flow

```
Client → POST /login {username, password}
           ↓
Server → Verify creds → Create JWT → Sign with secret key
           ↓
Client ← {access_token: "eyJhbG..."}
           ↓
Client stores token (localStorage)
           ↓
Client → GET /profile
         Header: Authorization: Bearer eyJhbG...
           ↓
Server → Verify signature → Decode payload → Get user identity
           ↓
Client ← 200 OK {data}
```

### Apr 2026 PYQ Q3 — Main Advantage of JWT

```
✅ JWTs are stateless and eliminate the need for server-side sessions
❌ JWTs require server-side storage — GALAT (sessions ki zaroorat nahi!)
❌ JWTs are longer and contain more data — TRUE but NOT an advantage
❌ JWTs automatically handle token expiration — GALAT (manually karna padta hai!)
```

### Payload Contains (PYQ Q6)

```
✅ User claims such as identity and expiration time
❌ Encryption keys — Header mein algorithm hota hai, key store nahi
❌ The signing algorithm details — Header mein hota hai
❌ The token signature hash — Signature section mein hota hai
```

---

## 23. Flask-JWT-Extended

### Basic Setup

```python
from flask import Flask, jsonify, request
from flask_jwt_extended import JWTManager, create_access_token, \
    jwt_required, get_jwt_identity

app = Flask(__name__)
app.config["JWT_SECRET_KEY"] = "your-secret-key"
jwt = JWTManager(app)

# LOGIN — Token generate karo
@app.route("/login", methods=["POST"])
def login():
    username = request.json.get("username")
    access_token = create_access_token(identity=username)
    return jsonify(access_token=access_token)

# PROTECTED ROUTE — Token verify karo
@app.route("/profile")
@jwt_required()    # ← Ye decorator check karta hai Authorization header!
def profile():
    current_user = get_jwt_identity()  # Token se username nikalo
    return jsonify(message=f"Welcome {current_user}")
```

### How to Send Token

```http
GET /profile HTTP/1.1
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...
```

> **Token body mein NAHI, header mein jaata hai!**

### Apr 2026 PYQ Q24 — Flask-JWT

```
✅ /profile returns "Welcome Jassi" only if valid JWT in Authorization header
✅ If no token → unauthorized error (401)
❌ Token must be in POST body — GALAT (header mein jaata hai!)
```

### HTTP Decorators with Auth

```python
@app.route('/api/resource')
@auth_required     # Pehle authentication check
@roles_required('admin')  # Phir authorization check
def resource():
    return jsonify(data="secret data")

# No valid token → 401 Unauthorized
# Valid token but wrong role → 403 Forbidden
```

---

## 24. XSS (Cross-Site Scripting)

### Definition

XSS = Attacker **malicious JavaScript inject** karta hai website mein, jo dusre users ke browser mein **execute** ho jaata hai → cookie steal, session hijack, data theft.

### 3 Types of XSS

| Type | Kaise Hota Hai |
|---|---|
| **Stored XSS** | Malicious script DB mein save, har page load pe execute |
| **Reflected XSS** | Script URL/query param se aata hai, response mein reflect |
| **DOM-based XSS** | Client-side JS khud vulnerable code DOM mein likhta hai |

### Prevention — Core Principle

> "User input ko kabhi **code** ki tarah treat mat karo — hamesha **data** samjho!"

### Safe vs Unsafe Code

```html
<!-- ❌ DANGEROUS — v-html XSS ka cause hai! -->
<div v-html="userComment"></div>
<!-- agar userComment = "<script>stealCookies()</script>" → EXECUTE HOGA! -->

<!-- ✅ SAFE — Vue {{ }} automatically escape karta hai -->
<div>{{ userComment }}</div>
<!-- <script> tag TEXT ki tarah dikhega, execute nahi hoga -->
```

```python
# Backend — Python mein sanitize karo
import html
safe_output = html.escape(user_input)  # < → &lt;, > → &gt;
```

### Dec 2025 PYQ Q8 — How to Stop XSS

```
✅ Backend validation + ensure user-provided text not executed as code
❌ Store JWT in cookies — ye XSS solution nahi, storage decision hai
❌ Use v-html for user data — YEH KHUD XSS CAUSE HAI!
❌ Send cookies with every request — CSRF se related, XSS nahi
```

---

## 25. CSRF & Cookie Security

### CSRF — Cross-Site Request Forgery

```
User logged in hai bank.com pe
Evil-site.com pe jaata hai
Evil-site → bank.com pe hidden form submit karta hai user ki cookie ke saath
→ Unauthorized transfer!
```

### Cookie Security Flags

| Flag | Kya Karta Hai |
|---|---|
| `HttpOnly` | JavaScript se cookie access nahi ho sakti → XSS protection |
| `Secure` | Sirf HTTPS pe cookie bhejta hai → MITM protection |
| `SameSite=Strict` | Cross-site requests mein cookie nahi jaati → CSRF protection |
| `SameSite=Lax` | GET requests mein cross-site allow, POST mein nahi |

```python
# Flask mein secure cookie:
response.set_cookie(
    'session',
    value=session_token,
    httponly=True,       # JS access block
    secure=True,         # HTTPS only
    samesite='Strict'    # CSRF protection
)
```

### Dec 2025 PYQ Q5 — Secure Session Cookie

```
✅ httponly=True → JS se steal nahi ho sakti
✅ secure=True   → HTTPS only, MITM se safe
✅ samesite='Strict' → Cross-site requests mein cookie nahi jaati → CSRF blocked
```

---

## 26. WebSockets

### Definition

WebSocket = **Persistent, Full-duplex (two-way) communication** protocol between client aur server.

### Normal HTTP vs WebSocket

```
HTTP:            Client → Request → Server → Response (connection close)

WebSocket:       Client ←──── Persistent Connection ────► Server
                       Dono independently message bhej sakte hain!
```

### Handshake Process (HTTP GET se shuru)

```http
// Client bhejta hai:
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket        ← Protocol upgrade request
Connection: Upgrade
Sec-WebSocket-Key: ...
Sec-WebSocket-Version: 13

// Server respond karta hai:
HTTP/1.1 101 Switching Protocols   ← IMPORTANT: 101 status!
Upgrade: websocket
Connection: Upgrade
```

### Apr 2026 PYQ Q4 — WebSocket Handshake Method

```
✅ GET — WebSocket handshake ke liye HTTP GET use hota hai
❌ POST — data bhejne ke liye, upgrade ke liye nahi
❌ PUT  — resource update ke liye
❌ DELETE — resource delete ke liye
```

> **101 Switching Protocols** = HTTP connection ko WebSocket mein upgrade kiya ja raha hai

### Use Cases

```
✅ Chat applications
✅ Live notifications
✅ Online games
✅ Live dashboards
✅ Real-time collaboration
✅ Live sports scores
```

---

## 27. Redis

### Definition

Redis = **In-memory Data Structure Store**
- Data RAM mein store hota hai (disk nahi) → **Extremely fast** (microseconds)
- NoSQL, key-value based
- SQL nahi use karta

### Redis Data Structures

```
Strings:      SET name "Abhishek" / GET name
Lists:        LPUSH queue "task1" / RPOP queue
Sets:         SADD users "user1" / SMEMBERS users
Hashes:       HSET user:1 name "Abhishek" age 20
Sorted Sets:  ZADD scores 100 "player1"
```

### 3 Main Use Cases

| Use Case | Kya Hai |
|---|---|
| **Caching** | Frequently accessed data temporarily store → DB pe load kam |
| **Pub/Sub (Message Broker)** | Real-time messages ek service se doosri tak |
| **Session Storage** | User sessions fast access ke liye |

### Redis Architecture

```
Request → API Server → Redis Cache Check
                              │
                    ┌─────────┴────────┐
                    │                  │
               Cache HIT          Cache MISS
               (data mila!)       (data nahi)
                    │                  │
              Turant return       Main DB Query
              (Fast!)             → Redis mein store
                                  → Response return
```

### Dec 2025 PYQ Q7 + Q9 — Redis Facts

```
✅ In-memory data structure store
✅ Can support publish-subscribe (pub/sub)
✅ Can be used as Celery result backend
✅ Redis is faster than disk-based databases (because in-memory!)
❌ Redis can only store strings — GALAT (multiple data structures!)
❌ Redis uses SQL — GALAT (NoSQL, key-value commands use karta hai)
❌ Redis is slower because in-memory — GALAT (in-memory = FASTER!)
```

> **"Complement" karta hai, "Replace" nahi:** Redis volatile hai (restart pe data ja sakta hai unless persistence configure karo) → Critical data ke liye traditional DB hona chahiye

---

## 28. Webhooks

### Definition

Webhook = **Reverse API** / **Event-driven HTTP callback**

```
Normal API (Pull):  Client → Server (data maangna)
Webhook (Push):     Server → Client (event hone pe khud batata hai)
```

### Webhook Flow

```
Event occurs (Order created)
       ↓
Your Server sends HTTP POST request
       ↓
Destination Server receives it
       ↓
Processes the event
```

### Key Facts (Dec 2025 PYQ Q25)

```
✅ Webhooks use HTTP POST requests to communicate events asynchronously
✅ Redis is suitable for caching (fast in-memory access)
❌ Webhooks guarantee exact processing order — GALAT (network latency → out-of-order)
❌ Webhooks automatically retry if service down — GALAT (not a default behavior!)
```

> **No Auto-Retry:** Retry mechanism developer ko manually implement karna padta hai (exponential backoff, message queues)

> **No Order Guarantee:** Multiple webhooks alag-alag timing se pahunch sakte hain — out-of-order possible!

---

## 29. Flask Caching (@cache.memoize vs @cache.cached)

### Difference

| | `@cache.cached` | `@cache.memoize` |
|---|---|---|
| Arguments consider karta hai? | ❌ Nahi (ek hi cache key) | ✅ Haan (har unique arg combo = alag key) |
| Use case | Route/function output cache | Function + arguments based cache |

### Apr 2026 PYQ Q13 — Memoize Example

```python
@cache.memoize(timeout=300)  # 300 seconds = 5 minutes
def fetch_result(id):
    time.sleep(3)  # expensive operation
    return f"Result for {id}"

# 3 minutes ke andar:
fetch_result(1)  # Cache MISS → 3 sec (stores {1: result})
fetch_result(2)  # Cache MISS → 3 sec (different arg! stores {2: result})
fetch_result(1)  # Cache HIT  → ~0 sec (180s < 300s timeout)

# Total time = 3 + 3 + 0 = 6 seconds
```

> **Key Rule:** `memoize` argument-aware hai → `fetch_result(1)` aur `fetch_result(2)` = **do alag cache entries**

---

## 30. SPA — Client-Side vs Server-Side Routing

### Comparison

| Feature | Server-Side Routing (Traditional) | Client-Side Routing (SPA) |
|---|---|---|
| Har page change pe | Server se poora HTML mangna | JS DOM update karta hai |
| Page reload? | ✅ Full reload | ❌ Koi reload nahi |
| Backend load | High (har page request) | Low (sirf API/JSON) |
| UX | Slower, flickering | Fast, app-like feel |
| SEO (by default) | ✅ Better | ❌ Worse (empty HTML shell) |
| Initial load | Fast | Slow (poora JS bundle) |

### Apr 2026 PYQ Q16 — SPA Benefits

```
✅ (A) Seamless page transitions without full reloads
✅ (B) Reduced load on the backend server
❌ (C) Slower navigation — GALAT (FASTER hota hai!)
❌ (D) Better SEO by default — GALAT (SPA mein SEO by default worse hai!)
```

> **SEO Trap:** SPA mein search engine crawlers ko **empty HTML shell** milta hai — content JavaScript se baad mein inject hota hai. SSR (Server-Side Rendering) ya prerendering se fix hota hai.

---

## 31. HTTP Status Codes

### Essential Codes for Exam

| Code | Name | Kab Aata Hai |
|---|---|---|
| `200` | OK | Success |
| `201` | Created | Resource create hua |
| `204` | No Content | Success, no body |
| `400` | Bad Request | Client ki request galat |
| `401` | Unauthorized | **Authentication fail** (token nahi ya invalid) |
| `403` | Forbidden | **Authorization fail** (token valid but permissions nahi) |
| `404` | Not Found | Resource exist nahi |
| `405` | Method Not Allowed | GET bheja PUT chahiye tha |
| `500` | Internal Server Error | Server side kuch toota |
| `101` | Switching Protocols | WebSocket upgrade! |

### 401 vs 403 — Critical Difference!

```
401 Unauthorized = "Kaun ho tum? Login karo pehle"
               → Authentication missing/invalid
               → @auth_required fail hua

403 Forbidden   = "Main jaanta hun tum kaun ho, par permission nahi"
               → Authentication OK, but role/permission missing
               → @roles_required fail hua
```

---

## 32. Git Commands

### Merge Workflow (Dec 2025 PYQ Q6)

```bash
# Currently on branch: appdev2
# Main ke changes appdev2 mein laane hain:

git merge main           # main ke changes appdev2 mein merge
git commit -m "Merged main into appdev2"

# Agar appdev2 pe nahi ho pehle:
git checkout appdev2     # ya: git switch appdev2
git merge main
git commit -m "Merged main into appdev2"
```

**Rule:** Jis branch mein changes chahiye → us branch pe jao → source branch merge karo

```
main → appdev2 (changes laane hain)
→ checkout appdev2 (destination pe jao)
→ merge main (source ko merge karo)
```

---

## 🎯 Last-Minute Quick Revision Table

| Concept | Ek Line Mein |
|---|---|
| Arrow `this` | Lexical — creation time ka this freeze hota hai |
| `bind()` | New function return karta hai (execute nahi) |
| Promise settle | Pehla call (resolve/reject) wins, baad wale ignore |
| `.finally()` | Hamesha chalta, d=undefined, return ignore |
| `fetch()` reject | Sirf network fail pe, HTTP errors pe resolve hota |
| `response.ok` | 200-299 = true, baaki = false |
| Sync before async | `console.log("END")` hamesha Promise .then se pehle |
| `v-if` vs `v-show` | if = DOM add/remove, show = CSS display:none |
| Vuex flow | Action → commit → Mutation → State change |
| JWT advantage | Stateless, no server session storage |
| XSS prevention | Input validate + output escape (never v-html with user data) |
| CSRF prevention | SameSite=Strict cookie |
| WebSocket handshake | HTTP GET + Upgrade header → 101 status |
| Redis | In-memory, fast, caching/pub-sub/celery backend |
| Webhook | HTTP POST event, no auto-retry, no order guarantee |
| `memoize` | Argument-aware cache (diff args = diff keys) |
| SPA SEO | By default WORSE (empty shell problem) |
| 401 vs 403 | 401=who are you, 403=I know you but NO permission |
| Shallow copy `...` | Sirf top-level copy, nested objects share hote hain |
| `instanceof` | Poori prototype chain check karta hai |
| Vue lifecycle reuse | Same route param → component reuse → no created(), updated() fires |
| Computed cache | Sirf dependency change pe re-run, otherwise cached value |
| `$route.params` | Reactive property — change hone se computed/updated trigger |

---

## 🚨 Top 10 Most Common Exam Traps

1. **`typeof NaN === "number"`** — NaN ka type number hai! (counterintuitive)
2. **Arrow function ka `this` = global nahi hota hamesha** — enclosing scope ka this hota hai
3. **`fetch()` 404 pe reject nahi karta** — resolve karta hai, `response.ok = false`
4. **`bind()` turant execute nahi karta** — function return karta hai
5. **Promise reject ke baad resolve ignore hota hai** — pehla call wins
6. **`.finally()` ko value nahi milti** — d = undefined hamesha
7. **`{...obj}` shallow copy hai** — nested objects share hote hain!
8. **SPA mein SEO by default worse hota hai** — "better SEO" option = TRAP
9. **Vuex action khud state change nahi karta** — sirf mutation ko commit karta hai
10. **v-html XSS ka cause hai** — user data ke saath kabhi mat use karo

---


*Notes compiled from: MAD2 Apr 2026 PYQ + Dec 2025 PYQ + Promise.md | Last updated: Sep 12, 2026*
