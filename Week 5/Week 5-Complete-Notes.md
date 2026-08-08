# Week 5 — Vue 2 + Flask + JS

## Table of Contents

1. [Introduction — Separation of Concerns](#1-introduction--separation-of-concerns)
2. [JavaScript — Synchronous vs Asynchronous](#2-javascript--synchronous-vs-asynchronous)
3. [Callbacks](#3-callbacks)
4. [JavaScript Event Loop](#4-javascript-event-loop)
5. [Promises](#5-promises)
6. [Async / Await](#6-async--await)
7. [Fetch API](#7-fetch-api)
   - GET Request
   - POST Request (JSON)
   - Form Data Submission
   - File Upload
   - Error Handling
   - Using `await` with Fetch
8. [Vue.js 2 Lifecycle Hooks](#8-vuejs-2-lifecycle-hooks)
   - beforeCreate
   - created
   - beforeMount
   - mounted
   - beforeUpdate / updated
   - beforeDestroy / destroyed
9. [Fetch API with Vue.js 2 + Flask](#9-fetch-api-with-vuejs-2--flask)
10. [Axios — Brief Overview](#10-axios--brief-overview)
11. [Important Comparisons](#11-important-comparisons)
12. [Important Definitions — Quick Revision](#12-important-definitions--quick-revision)
13. [Important Code Patterns](#13-important-code-patterns)
14. [Exam Preparation](#14-exam-preparation)
15. [Quick Revision Sheet](#15-quick-revision-sheet)
16. [One-Day-Before-Exam Revision](#16-one-day-before-exam-revision)

---

# 1. Introduction — Separation of Concerns

## Definition

> **Separation of Concerns** is a design principle for separating a computer program into distinct sections, such that each section addresses a separate concern — the frontend handles UI, and the backend handles data, business logic, and database operations.

## Explanation

ek badi application ko **do parts** mein todna:

- **Frontend (Client Side):** Jo browser mein run karta hai — Vue.js. Sirf UI dekhata hai aur user se interact karta hai.
- **Backend (Server Side):** Jo server pe run karta hai — Flask API. Database se data laata hai, business logic handle karta hai, authentication karta hai.

Dono parts **ek doosre se directly communicate nahi karte** — beech mein **HTTP Requests** aur **JSON** use hote hain.

## Why Do We Need It?

- Agar frontend aur backend ek saath hote, to ek change se dono break ho sakte hain
- Backend ko pata nahi hona chahiye ki UI kaisi dikhti hai
- Frontend ko directly database access nahi karni chahiye
- Clean architecture ke liye zaroori hai

## Architecture Diagram

```
+--------------------+         HTTP Request          +--------------------+
|                    |  --------------------------->  |                    |
|   Vue.js Frontend  |    GET /api/notes (JSON)       |   Flask Backend    |
|   (Browser)        |                                |   (Server)         |
|                    |  <---------------------------  |                    |
+--------------------+       JSON Response            +--------------------+
                                                              |
                                                              v
                                                     +------------------+
                                                     |    Database      |
                                                     +------------------+
```

## Important Rules for Clean Separation

| Rule | Explanation |
|------|-------------|
| Backend never knows UI | Flask sirf JSON return karta hai, HTML nahi |
| No direct DB access from frontend | Vue database ko directly nahi touch karta |
| Neutral data format | JSON preferred hai aaj kal |
| Data input via URLs / form data | Backend URL-based APIs expose karta hai |

> 🎯 **Exam Point:** "Backend should never know what UI looks like" — ye line exam mein important hai.

---

# 2. JavaScript — Synchronous vs Asynchronous

## Definition

> **Synchronous execution** means code runs line by line, one at a time, blocking further execution until the current operation completes. **Asynchronous execution** means an operation can be started and the program continues without waiting for it to finish.

## Explanation

**Synchronous (Blocking):**
Socho ek restaurant mein ek hi waiter hai. Wo ek customer ka order leta hai, kitchen mein jaata hai, wait karta hai, khana laata hai — tab next customer ke paas jaata hai. Baaki sab wait karte rehte hain!

**Asynchronous (Non-Blocking):**
Ab waiter order leta hai, kitchen mein ticket deta hai, aur **bina wait kiye** next customer ke paas chala jaata hai. Jab khana ready hota hai, tab deliver kar deta hai.

## JavaScript Default Behavior

JavaScript is **single-threaded** — ek time pe sirf ek operation execute hoti hai.

```javascript
// Synchronous Example — line by line chalti hai
console.log("before");
let wish = say_hello();   // blocks here till done
console.log(wish);
console.log("after");

// Output: before → hello → after (in order)
```

## Kab Asynchronous zaroori hai?

Real applications mein ye situations aati hain jo time lete hain:
- **Network requests** — server se data maangna
- **Database reads/writes** — data fetch karna
- **File operations** — disk se padhna/likhna
- **User events** — click, hover, keypress

Agar ye sab synchronous hote, to browser **hang** ho jaata — user kuch nahi kar sakta!

## Async ka faida

```
Without Async:                    With Async:
-----------                       -----------
Task 1 starts                     Task 1 starts (sent to background)
Task 1 waits... (3 sec)           Task 2 starts immediately!
Task 1 ends                       Task 3 starts immediately!
Task 2 starts                     Task 1 returns (after 3 sec)
...                               All done faster!
```

> 🔥 **Very Important:** JavaScript ek **single thread** pe chalta hai, lekin browser ke paas special APIs hain jo asynchronous operations allow karti hain. Main thread block nahi hoti.

## Comparison Table

| Feature | Synchronous | Asynchronous |
|---------|-------------|--------------|
| Execution | Line by line, blocking | Non-blocking, background |
| Browser behavior | Hangs during wait | Stays responsive |
| Use case | Simple computations | Network calls, I/O |
| JavaScript default | Yes | Via async/await, callbacks, promises |

---

# 3. Callbacks

## Definition

> A **callback** is a function passed as an argument to another function, which is then invoked (called back) when a particular operation completes or a specific event occurs.

## Explanation

Callback matlab — "Jab kaam ho jaaye, mujhe call kar lena."

Jaise pizza order karte ho aur delivery boy ko bolte ho: "Jab pizza ready ho, ghar aa jaana." Tu apna kaam karta rehta hai — pizza ready hone pe callback (delivery) hoti hai.

## Code Example

```javascript
// Ye ek "callback" pattern hai

function doSomething(successCB, failureCB) {
    let result = doLongComputation(); // ye kuch time leta hai
    
    if (result) {
        successCB(); // success hone pe ye function call hoga
    } else {
        failureCB(); // fail hone pe ye function call hoga
    }
}

// Use karna:
doSomething(
    function() { console.log("Success!"); },   // successCB
    function() { console.log("Failed!"); }     // failureCB
);
```

## Code Explanation

- `successCB` aur `failureCB` dono **functions** hain jo parameter ke roop mein pass kiye gaye
- Ye functions tab call hote hain jab operation complete hota hai
- Ye ek "Higher-order function" pattern hai — functions ko functions pass karna

## Problem with Callbacks

Jab multiple callbacks nest ho jaate hain — ise **"Callback Hell"** kehte hain:

```javascript
// Callback Hell — deeply nested, unreadable
doSomething(function(result1) {
    doSomethingElse(result1, function(result2) {
        doMoreStuff(result2, function(result3) {
            // Aur gehri hoti rehti hai... 😱
        });
    });
});
```

> ⚠️ **Important:** Callbacks ka sabse bada problem hai "Callback Hell" — code padhna aur debug karna mushkil ho jaata hai. Isliye Promises aaye.

---

# 4. JavaScript Event Loop

## Definition

> The **JavaScript Event Loop** is a mechanism that continuously checks the **call stack** and the **callback queue**. When the call stack is empty, it picks up tasks from the callback queue and executes them, enabling asynchronous behavior in a single-threaded environment.

## Explanation

**Call Stack** — ek stack (pile) jisme current executing functions hoti hain. Jab function call hoti hai, add hoti hai; complete hone pe remove hoti hai.

**Callback Queue** — ye ek line (queue) hai jisme asynchronous operations ke complete hone ke baad unke callbacks wait karte hain.

**Event Loop** — ek security guard ki tarah jo continuously check karta hai: "Kya call stack khali hai? Agar haan, toh callback queue se kaam uthao!"

## How It Works

```
Call Stack (LIFO)            Callback Queue (FIFO)
+--------------+             +------------------+
|  console.log |             |  networkCallback |
|  fetch()     |   ------>   |  timerCallback   |
|  main()      |             |  clickCallback   |
+--------------+             +------------------+
        ^                            |
        |                            |
        +------- Event Loop ---------+
                (keeps checking)
```

## Real Example — Why "after" prints before "async hello"

```javascript
async function say_hello() {
    // setTimeout 2 seconds — background mein jaata hai
    return new Promise((resolve) => {
        setTimeout(() => resolve("async hello"), 2000);
    });
}

console.log("before");
let wish = say_hello();   // promise return karta hai, wait nahi karta
wish.then(v => console.log(v));  // callback queue mein jaata hai
console.log("after");

// Output:
// before        ← immediate
// after         ← immediate (wish ka wait nahi kiya)
// async hello   ← 2 seconds baad (callback queue se)
```

> 💡 **Easy Way to Remember:** Call Stack > Background Operations > Callback Queue > Event Loop picks it up.

---

# 5. Promises

## Definition

> A **Promise** is a JavaScript object representing the eventual completion (or failure) of an asynchronous operation and its resulting value. A promise can be in one of three states: **pending**, **fulfilled**, or **rejected**.

## Explanation

**Promise** ka matlab exactly wahi jo Hindi mein hota hai — "Promise!"

Socho koi tumse promise karta hai: "Main kal result bataunga."

Abhi result nahi pata (pending). Kal ya toh batayega (fulfilled) ya bata dega ki pata nahi chala (rejected).

JavaScript mein bhi yehi hota hai:
- `pending` — kaam chal raha hai, abhi answer nahi aaya
- `fulfilled` — kaam successfully complete ho gaya, value mili
- `rejected` — kaam fail ho gaya, error mila

## Promise States Diagram

```
                  +----------+
                  | PENDING  |
                  +----------+
                  /           \
       Resolve   /             \  Reject
               /               \
     +-----------+         +----------+
     | FULFILLED |         | REJECTED |
     +-----------+         +----------+
     (.then runs)          (.catch runs)
```

## Creating a Promise

```javascript
async function say_hello() {
    return new Promise((resolve, reject) => {
        // Koi async kaam karo (jaise network call)
        setTimeout(function() {
            resolve("async hello"); // Success!
            // Ya agar fail hota: reject("Some error");
        }, 2000);
    });
}
```

### Code Explanation

- `new Promise(...)` — ek naya promise object banata hai
- `resolve` — jab kaam successful ho, isse call karo with result
- `reject` — jab kaam fail ho, isse call karo with error
- `setTimeout` — 2000ms = 2 seconds baad resolve hoga

## Using a Promise with `.then()`

```javascript
let wish = say_hello(); // Returns a Promise

wish
    .then(function(value) {
        // Fulfilled hone pe — value yahan milti hai
        console.log(value); // "async hello"
    })
    .catch(function(error) {
        // Rejected hone pe — error yahan milta hai
        console.log("Error:", error);
    });
```

## Promise Chaining

```javascript
// Promises chain kiye ja sakte hain
fetch("/api/notes")
    .then(response => response.json())   // 1st promise resolve
    .then(data => {                       // 2nd promise resolve
        console.log(data);
    })
    .catch(error => {                     // Kisi bhi stage pe error
        console.log("Error:", error);
    });
```

> 🔥 **Very Important:** `.then()` aur `.catch()` dono khud ek **Promise** return karte hain — isliye chaining possible hai.

## Advantages of Promises over Callbacks

| Feature | Callbacks | Promises |
|---------|-----------|----------|
| Readability | Nested, messy | Chained, clean |
| Error handling | Har level pe manually | Single `.catch()` |
| Multiple async | "Callback hell" | Chain karo |
| Debugging | Difficult | Easier |

---

# 6. Async / Await

## Definition

> **`async`** is a keyword used to declare an asynchronous function that automatically returns a Promise. **`await`** is a keyword used inside an `async` function to pause execution until a Promise is resolved, making asynchronous code appear synchronous.

## Explanation

**Analogy:** Socho online khana order kar rahe ho.

**Without await (Promise only):** Order diya aur khana aane se pehle hi TV dekhne laga. Jab khana aaya, tab notification aaya.

**With await:** Order diya aur **wait kiya** — jab tak khana nahi aaya, aage kuch nahi kiya. Synchronous jaisa lagta hai lekin actually background mein async hai!

`await` basically **Promise ke resolve hone ka wait karta hai** — lekin browser block nahi hota!

## Basic Syntax

```javascript
// async function — ye automatically ek Promise return karta hai
async function say_hello() {
    return "hello";
}

// Isse use karna:
async function greetings() {
    console.log("before");
    
    let wish = await say_hello(); // Yahan wait karega
    
    console.log(wish);  // "hello" — promise resolved value
    console.log("after");
    
    return wish;
}

greetings();
// Output: before → hello → after (synchronous order mein!)
```

## Code Explanation — Line by Line

```javascript
async function greetings() {   // async keyword — ye promise return karega
    console.log("before");     // immediate execute

    // await lagane se:
    // - say_hello() call hota hai
    // - Promise pending hota hai
    // - Execution RUKTI hai (lekin browser nahi ruka)
    // - Jab Promise resolve hota hai, wish ko value milti hai
    let wish = await say_hello();

    console.log(wish);  // resolved value print hoti hai
    console.log("after");
}
```

## Comparison: `.then()` vs `async/await`

```javascript
// Approach 1: Promise with .then()
say_hello()
    .then(v => console.log(v))
    .catch(e => console.log(e));

// Approach 2: async/await (same kaam, saaf code)
async function main() {
    try {
        let v = await say_hello();
        console.log(v);
    } catch(e) {
        console.log(e);
    }
}
```

## ⚠️ CRITICAL RULE: await sirf async function ke andar!

```javascript
// ❌ WRONG — await cannot be in regular function
function greetings() {
    let wish = await say_hello(); // SyntaxError!
}

// ✅ CORRECT — await must be inside async function
async function greetings() {
    let wish = await say_hello(); // Works!
}
```

> ⚠️ **Important:** `await` sirf `async` function ke andar use ho sakta hai. Warna `SyntaxError: await is only valid in async functions, async generators and modules` aayega.

## Error Handling with async/await

```javascript
async function greetings() {
    try {
        // Attempt the async operation
        let wish = await say_hello();
        console.log(wish);
        console.log("after");
        return wish;
    } catch(error) {
        // Agar promise reject ho toh yahan aao
        console.log("GOT ERROR");
        console.log(error);
        return null;
    }
}
```

## Dry Run — With Delay

```javascript
async function say_hello() {
    return new Promise((resolve, reject) => {
        setTimeout(() => resolve("async hello"), 2000);
    });
}

async function greetings() {
    console.log("before");       // Step 1: immediate
    let wish = await say_hello(); // Step 2: 2 seconds wait
    console.log(wish);            // Step 3: "async hello"
    console.log("after");         // Step 4: "after"
}

greetings();
```

| Step | Time | Console Output |
|------|------|----------------|
| 1 | 0ms | `before` |
| 2 | 0ms | (waiting for promise...) |
| 3 | 2000ms | `async hello` |
| 4 | 2000ms | `after` |

## Important Points

> 💡 **Easy Way to Remember:**
> - `async` function → always returns a Promise
> - `await` → pause karo jab tak Promise resolve na ho
> - `await` sirf `async` ke andar chalta hai
> - Error handling ke liye `try/catch` use karo

---

# 7. Fetch API

## Definition

> The **Fetch API** provides a JavaScript interface for making HTTP requests to servers. It returns a **Promise** that resolves to the **Response** object representing the response to the request.

## Explanation

**Fetch API** browser ka built-in tool hai jo tumhe server se data maangne aur bhejne deta hai — bilkul jaise post office se letter bhejna aur reply paana.

Pehle `XMLHttpRequest` use hota tha, jo bahut verbose tha. Fetch zyada clean aur modern hai.

## Why Fetch API?

- Browser mein built-in hai — koi library install nahi
- Promise-based hai — `.then()` aur `async/await` ke saath kaam karta hai
- GET, POST, PUT, DELETE sab support karta hai
- JSON data easily handle karta hai

## Basic Syntax

```javascript
fetch(url, options)
    .then(response => response.json())  // Response ko JSON mein convert karo
    .then(data => {
        console.log(data);              // Data use karo
    })
    .catch(error => {
        console.log("Error:", error);   // Error handle karo
    });
```

## ⚠️ CRITICAL: Fetch Promise Rejection Rules

> ⚠️ **Important:** Fetch Promise sirf **network error** pe reject hota hai — jaise no internet, DNS failure.

> HTTP errors jaise **404 (Not Found)** ya **505** pe fetch **reject nahi karta** — promise **resolve** hota hai lekin `response.ok` = `false` hota hai!

```
Network Error  → Promise REJECTS  → .catch() mein jaao
HTTP 404/505   → Promise RESOLVES → response.ok check karo manually
```

---

## 7.1 GET Request (Data Fetch Karna)

### Code Example

```javascript
fetch("https://httpbin.org/get")
    .then(response => response.json())   // 1st promise — response body ko JSON mein parse karo
    .then(data => {
        console.log(data);               // 2nd promise — actual data
    })
    .catch(error => {
        console.log("Error:", error);    // Network errors yahan aate hain
    });
```

### Line-by-Line Explanation

- `fetch("URL")` → Server ko GET request bhejta hai (default method = GET), ek Promise return karta hai
- `.then(response => response.json())` → Pehla `.then()`: response object milta hai. `response.json()` bhi Promise return karta hai
- `.then(data => ...)` → Doosra `.then()`: actual JSON data milta hai
- `.catch(error => ...)` → Agar network error ho

### Using async/await with GET

```javascript
async function fetchData() {
    let response = await fetch("https://httpbin.org/get");  // 1st await
    let data = await response.json();                        // 2nd await
    console.log(data);
}

fetchData();
```

> 🔥 **Very Important:** Fetch mein **do promises** hain — ek `fetch()` ka, ek `response.json()` ka. Dono ke liye `await` lagate hain!

---

## 7.2 POST Request (JSON Data Bhejna)

```javascript
let data = {
    "name": "Thejesh",
    "city": "Bengaluru"
};

fetch("https://httpbin.org/post", {
    method: "POST",                         // HTTP method
    headers: {
        "Content-Type": "application/json" // Server ko batao — JSON aa raha hai
    },
    body: JSON.stringify(data)             // Object ko JSON string mein convert karo
})
.then(response => response.json())
.then(data => {
    console.log("Success:", data);
})
.catch(error => {
    console.log("Error:", error);
});
```

### Code Explanation

| Part | Explanation |
|------|-------------|
| `method: "POST"` | HTTP method specify karo |
| `headers: {...}` | Request headers — server ko format batate hain |
| `"Content-Type": "application/json"` | Server ko batana zaroori hai ki body mein JSON hai |
| `body: JSON.stringify(data)` | JS object ko JSON string mein convert karo |

> ⚠️ **Common Mistake:** `Content-Type: application/json` bhoolna! Flask isko parse nahi kar payega.

> ⚠️ **Common Mistake:** `body` mein direct object mat daalo — `JSON.stringify()` use karo!

---

## 7.3 Form Data Submission

```html
<!-- HTML Form -->
<form id="my-form">
    <input type="text" name="name">
    <input type="text" name="city">
    <button type="submit">Send</button>
</form>
```

```javascript
// FormData se form automatically capture hota hai
let form = new FormData(document.getElementById("my-form"));

fetch("https://httpbin.org/post", {
    method: "POST",
    body: form  // FormData directly body mein — Content-Type auto set hoti hai
})
.then(response => response.json())
.then(result => {
    console.log("Success:", result);
})
.catch(error => {
    console.log("Error:", error);
});
```

> 💡 **Easy Way to Remember:** FormData ke saath `Content-Type` manually set nahi karte — browser automatically `multipart/form-data` set karta hai!

---

## 7.4 File Upload

```html
<form id="my-form">
    <input type="text" name="name">
    <input type="text" name="city">
    <input type="file" name="profile_picture"> <!-- File input -->
    <button type="submit">Upload</button>
</form>
```

```javascript
let form = new FormData(document.getElementById("my-form"));

fetch("https://httpbin.org/put", {
    method: "PUT",   // Ya POST bhi ho sakta hai
    body: form       // File automatically include ho jaati hai
})
.then(response => response.json())
.then(result => {
    console.log("Uploaded:", result);
})
.catch(error => {
    console.log("Error:", error);
});
```

> 💡 **Easy Way to Remember:** File upload mein bhi same FormData pattern — file automatically base64 mein convert hokar jaati hai.

---

## 7.5 Error Handling — HTTP Errors

```javascript
fetch("https://httpbin.org/status/404")   // 404 URL
    .then(response => {
        // Pehle response.ok check karo
        if (!response.ok) {
            // HTTP error hai — manually throw karo
            console.log("Response not okay");
            throw new Error("HTTP error, status = " + response.status);
        }
        return response.json();           // Sab theek hai, data lo
    })
    .then(data => {
        console.log("Got data:", data);
    })
    .catch(function(error) {
        // Yahan aao: network error OR throw kiya hua error
        console.log("In catch Error:", error);
    });
```

### Error Types Summary

```
Fetch Request
     |
     |--- Network Error (no internet, DNS fail)
     |         → Promise REJECTS → .catch() automatically
     |
     |--- HTTP Error (404, 500, etc.)
               → Promise RESOLVES with response.ok = false
               → Manually check response.ok
               → throw new Error() to go to .catch()
```

---

## 7.6 Fetch with async/await — Full Pattern

```javascript
async function getApiData(url) {
    try {
        // Do awaits — one for fetch, one for json()
        let response = await fetch(url);
        
        if (!response.ok) {
            throw new Error("HTTP error! status: " + response.status);
        }
        
        let data = await response.json();
        return data;
        
    } catch(error) {
        console.log("Error:", error);
        return null;
    }
}

// Use karna
getApiData("https://api.example.com/notes")
    .then(data => console.log(data));
```

## Fetch Parameters Summary

```javascript
fetch(url, {
    method: "GET" | "POST" | "PUT" | "DELETE",  // HTTP Method
    headers: {
        "Content-Type": "application/json",       // Request headers
    },
    body: JSON.stringify(data),                   // Request body (GET mein nahi hota)
    mode: "cors" | "no-cors" | "same-origin",    // CORS mode
    cache: "default" | "no-store" | "reload"     // Cache policy
})
```

## Response Methods

| Method | Returns | Use Case |
|--------|---------|----------|
| `response.json()` | Promise → Object | JSON APIs |
| `response.text()` | Promise → String | Plain text |
| `response.blob()` | Promise → Blob | Images, files |
| `response.formData()` | Promise → FormData | Form responses |
| `response.arrayBuffer()` | Promise → ArrayBuffer | Binary data |

> ⚠️ **Important:** Ye sab methods bhi **Promise** return karti hain! Isliye `.then()` ya `await` lagana padta hai.

---

## 7.7 Reusable Headers and Request Objects

```javascript
// Headers object — reuse ke liye
let myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");

// Request object — URL + options ek saath
let myRequest = new Request("https://httpbin.org/post", {
    method: "POST",
    headers: myHeaders,
    mode: "cors",
    cache: "default"
});

// Use karna
fetch(myRequest)
    .then(response => response.json())
    .then(data => console.log(data));
```

> 💡 **Easy Way to Remember:** Agar ek hi type ke bahut requests karne hain (same headers, same method), toh pehle se `Headers` aur `Request` object banao aur reuse karo.

---

# 8. Vue.js 2 Lifecycle Hooks

## Definition

> The **Vue.js Component Lifecycle** is the sequence of stages a Vue component goes through from its creation to its destruction. **Lifecycle hooks** are special functions that are automatically called at specific stages of this lifecycle, allowing developers to run custom code at the right time.

## Explanation

**Analogy:** Socho ek student ka school mein ek din — wake up → school → class → homework → sleep → repeat. Har stage pe kuch kaam hota hai.

Isi tarah Vue component ka ek "life" hoti hai — create hone se lekar destroy hone tak. Har important stage pe Vue **lifecycle hooks** call karta hai — hum wahan apna code run kar sakte hain.

## Vue 2 Lifecycle Diagram

```
new Vue()
    |
    v
+------------------+
| Init Events &    |   beforeCreate hook called
| Lifecycle        |
+------------------+
    |
    v
+------------------+
| Init Injections  |   Data, methods, computed set up
| & Reactivity     |
+------------------+
    |
    v
+------------------+
| created          |   created hook called ← DATA AVAILABLE HERE
+------------------+
    |
    v
+------------------+
| Has 'el' option? |
+------------------+
    |YES                         NO
    v                            |
Has 'template'?              when vm.$mount(el)
    |                        is called
    |YES            NO
    |               |
Compile          Compile el's
template         outerHTML as
into render      template
function
    |
    v
+------------------+
| beforeMount      |   beforeMount hook called
+------------------+
    |
    v
+------------------+
| Create vm.$el &  |   'el' replaced by virtual DOM
| replace 'el'     |
+------------------+
    |
    v
+------------------+
| mounted          |   mounted hook called ← DOM AVAILABLE HERE
+------------------+
    |
    |   Data changes?
    |         YES
    |          v
    |  +---------------+
    |  | beforeUpdate  |   beforeUpdate hook
    |  +---------------+
    |          |
    |          v
    |  +------------------+
    |  | Virtual DOM      |
    |  | re-render &      |
    |  | patch            |
    |  +------------------+
    |          |
    |          v
    |  +---------------+
    |  | updated        |   updated hook
    |  +---------------+
    |          |
    |          +--- loop continues when data changes
    |
    |   vm.$destroy() called?
    |         YES
    |          v
    |  +----------------+
    |  | beforeDestroy  |   beforeDestroy hook
    |  +----------------+
    |          |
    |          v
    |  Teardown: watchers, child components, event listeners
    |          |
    |          v
    |  +----------+
    |  | destroyed |   destroyed hook
    |  +----------+
```

## All Lifecycle Hooks — Code Template

```javascript
var app = new Vue({
    el: "#app",
    data: {
        grand_total: 0
    },
    methods: {
        add_grand_total: function() {
            console.log("in grand_total");
            this.grand_total = this.grand_total + 1;
        }
    },

    // ===== LIFECYCLE HOOKS =====
    
    beforeCreate: function() {
        console.log("beforeCreate");
        // this.grand_total → UNDEFINED here!
        // No data access yet
    },
    
    created: function() {
        console.log("created");
        console.log(this.grand_total); // 0 — data accessible!
        // fetch data from backend yahan kar sakte hain
    },
    
    beforeMount: function() {
        console.log("beforeMount");
        // Template compile ho gayi, DOM replace hone wala hai
    },
    
    mounted: function() {
        console.log("mounted");
        console.log(this.$el); // DOM element accessible!
        // fetch data from backend yahan karna BEST PRACTICE hai
    },
    
    beforeUpdate: function() {
        console.log("beforeUpdate");
        // Data change ho gaya, DOM abhi update nahi hua
    },
    
    updated: function() {
        console.log("updated");
        // DOM update ho gaya
    },
    
    beforeDestroy: function() {
        console.log("beforeDestroy");
        // Event listeners, watchers band karo
        // this.$el aur data abhi bhi accessible hai
    },
    
    destroyed: function() {
        console.log("destroyed");
        // Sab kuch destroy ho gaya
        // Kisi ko notify karo ki ye component gaya
    }
});
```

---

## 8.1 `beforeCreate` Hook

### When Called?
Component initialize hone ke turant baad — lekin data/events setup hone se PEHLE.

### Key Points
- Data (this.data) → **NOT available** — `undefined` milega
- Event watchers → **NOT set up**
- Synchronously called immediately after instance is initiated

```javascript
beforeCreate: function() {
    console.log("beforeCreate");
    console.log(this.grand_total); // UNDEFINED — data nahi mili abhi
}
```

### Use Case
Bahut rare use hai. Plugins inject karne ke kaam aata hai.

> 🎯 **Exam Point:** `beforeCreate` mein data access karne ki koshish karo → `undefined` milega.

---

## 8.2 `created` Hook

### When Called?
Instance create hone ke baad — data, computed, methods, watchers sab set ho jaate hain.

### Key Points
- Data (**this.data**) → **AVAILABLE** ✅
- Methods → **AVAILABLE** ✅
- `this.$el` (DOM) → **NOT available** ❌
- Synchronously called

```javascript
created: function() {
    console.log("created");
    console.log(this.grand_total); // 0 — data accessible!
    // fetch data from backend here!
}
```

### Use Case
- API se data fetch karna (backend se)
- User ko show karne se pehle data ready karna

> 🎯 **Exam Point:** `created` mein data available hai lekin DOM (`$el`) nahi.

---

## 8.3 `beforeMount` Hook

### When Called?
Template compile ho gayi hai, lekin DOM mein inject hone se PEHLE.

### Key Points
- Template ready hai
- DOM abhi replace nahi hua
- `this.$el` → virtual element form mein

```javascript
beforeMount: function() {
    console.log("beforeMount");
    // Rarely used — mostly beforeCreate ya created prefer karo
}
```

---

## 8.4 `mounted` Hook ⭐ (Most Important!)

### When Called?
Component DOM mein mount ho gaya — `el` ko `vm.$el` se replace kar diya.

### Key Points
- `this.$el` → **AVAILABLE** ✅ (actual DOM element)
- Data → **AVAILABLE** ✅
- **Best place for API calls / data fetching**
- Server-side rendering mein call nahi hota

```javascript
mounted: function() {
    console.log("mounted");
    console.log(this.$el); // Actual DOM element!
    
    // ✅ BEST PLACE to fetch data
    this.fetchNotes();
}
```

> 🔥 **Very Important:** `mounted` API calls ke liye **most recommended** place hai kyunki:
> 1. DOM ready hai
> 2. Data ready hai
> 3. Async/await use kar sakte ho — rendering block nahi hogi

> 💡 **Easy Way to Remember:** `mounted` = everything is ready to go!

---

## 8.5 `beforeUpdate` and `updated` Hooks

### When Called?
Jab bhi `data` change hota hai → Virtual DOM re-render karta hai → ye hooks call hote hain.

```javascript
beforeUpdate: function() {
    console.log("beforeUpdate");
    // Data change ho gaya, lekin DOM abhi purana hai
},

updated: function() {
    console.log("updated");
    // DOM ab new data ke saath update ho gaya
}
```

### Demo
```javascript
// Vue DevTools mein manually grand_total = 1 set karo
// Ya button click karo
// Console mein dekhoge: "beforeUpdate" → "updated"
```

> 🎯 **Exam Point:** `beforeUpdate` aur `updated` tab call hote hain jab **data change** se **virtual DOM update** hota hai.

---

## 8.6 `beforeDestroy` and `destroyed` Hooks

### When Called?
Jab `vm.$destroy()` call hota hai ya component unmount hota hai (SPA mein page change pe).

```javascript
beforeDestroy: function() {
    // Abhi bhi el, data sab accessible hai
    // Cleanup karo:
    // - Event listeners band karo
    // - Timers clear karo (clearInterval, clearTimeout)
    // - Child components ko notify karo
    console.log("beforeDestroy");
},

destroyed: function() {
    // Sab kuch gone! No access to el, data, etc.
    // Main app ya doosre components ko notify karo
    console.log("destroyed");
}
```

> 🎯 **Exam Point:** `beforeDestroy` mein cleanup hota hai (event listeners off, watchers clear). `destroyed` mein notification bhejte hain.

---

## Lifecycle Hooks Summary Table

| Hook | Data Available? | DOM ($el) Available? | Best Use |
|------|----------------|----------------------|---------|
| `beforeCreate` | ❌ | ❌ | Plugin injection |
| `created` | ✅ | ❌ | Fetch data, initialize |
| `beforeMount` | ✅ | ❌ (virtual) | Rarely used |
| `mounted` | ✅ | ✅ | **API calls, DOM manipulation** |
| `beforeUpdate` | ✅ | ✅ (old) | Before re-render logic |
| `updated` | ✅ | ✅ (new) | After DOM update logic |
| `beforeDestroy` | ✅ | ✅ | Cleanup (listeners, timers) |
| `destroyed` | ❌ | ❌ | Send notifications |

## ⚠️ CRITICAL RULE: Arrow Functions Mat Use Karo!

```javascript
// ❌ WRONG — arrow function 'this' ko instance se bind nahi karta
mounted: () => {
    console.log(this.grand_total); // Error! 'this' wrong hai
}

// ✅ CORRECT — regular function use karo
mounted: function() {
    console.log(this.grand_total); // Works!
}
```

> ⚠️ **Important:** Vue 2 lifecycle hooks mein **arrow functions nahi** — `function()` likho. Arrow functions apna `this` context bind nahi karte, jo Vue ke reactive `this` se alag hai.

---

## Where to Fetch Data? (`created` vs `mounted`)

```
 created                              mounted
    |                                    |
    |-- Data available                   |-- Data + DOM both available
    |-- Good for:                        |-- Good for:
    |   - Non-DOM data operations        |   - DOM-dependent operations
    |   - Data needed before render      |   - Third-party DOM libraries
    |-- Cannot access DOM                |-- async/await won't block render
    |                                    |-- Most recommended for API calls
```

> 💡 **Easy Way to Remember:** Debate hai — lekin **most suggested: `mounted`** because:
> 1. Sab kuch ready hai
> 2. Server-side rendering ke saath bhi kaam karta hai
> 3. `async/await` use kar sakte hain without blocking

---

# 9. Fetch API with Vue.js 2 + Flask

## The Complete Flow

```
Database
    ^
    |
Flask API (Backend)
    ^
    |   HTTP (JSON)
    |
Fetch API (Bridge)
    ^
    |
Vue Component (Frontend)
```

## Flask Backend Setup

```python
# app.py
from flask import Flask, jsonify, request

app = Flask(__name__)

notes = [
    {"id": 1, "title": "Learn Flask", "content": "Create REST APIs"},
    {"id": 2, "title": "Learn Vue",   "content": "Use Vue CDN"}
]

# GET endpoint — all notes
@app.route("/api/notes", methods=["GET"])
def get_notes():
    return jsonify(notes)

# POST endpoint — add a note
@app.route("/api/notes", methods=["POST"])
def add_note():
    data = request.json
    new_note = {
        "id": len(notes) + 1,
        "title": data["title"],
        "content": data["content"]
    }
    notes.append(new_note)
    return jsonify(new_note)

if __name__ == "__main__":
    app.run(debug=True)
```

## Vue.js 2 Frontend (CDN)

```html
<!DOCTYPE html>
<html>
<head>
    <title>Notes App</title>
    <!-- Vue 2 CDN -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
</head>
<body>
    <div id="app" class="container mt-4">
        <h2>My Notes</h2>
        
        <!-- Notes list display -->
        <div v-for="note in notes" class="card mt-3">
            <div class="card-body">
                <h5>{{ note.title }}</h5>
                <p>{{ note.content }}</p>
            </div>
        </div>
        
        <!-- Add Note Form -->
        <button @click="addNote" class="btn btn-primary mt-3">Add Note</button>
    </div>

    <script>
        new Vue({
            el: "#app",
            data: {
                notes: []  // Initially khali array
            },
            methods: {
                // GET — all notes fetch karna
                fetchNotes: function() {
                    fetch("/api/notes")
                        .then(response => response.json())
                        .then(data => {
                            this.notes = data;  // Vue automatically UI update karta hai
                        })
                        .catch(error => {
                            console.log("Error:", error);
                        });
                },
                
                // POST — naya note add karna
                addNote: function() {
                    fetch("/api/notes", {
                        method: "POST",
                        headers: {
                            "Content-Type": "application/json"
                        },
                        body: JSON.stringify({
                            title: "Fetch API",
                            content: "Learning API communication"
                        })
                    })
                    .then(response => response.json())
                    .then(data => {
                        this.notes.push(data); // Naya note list mein add karo
                    })
                    .catch(error => {
                        console.log("Error:", error);
                    });
                }
            },
            
            // mounted — BEST PLACE for initial data fetch
            mounted: function() {
                this.fetchNotes();
            }
        });
    </script>
</body>
</html>
```

## Step-by-Step Flow

### When Page Loads

```
1. new Vue({...}) → Vue instance create
2. mounted() called → this.fetchNotes() invoke
3. fetch("/api/notes") → GET request to Flask
4. Flask returns JSON
5. .then(data => { this.notes = data }) → notes array update
6. Vue automatically re-renders UI with new data
```

### When Adding a Note

```
1. User clicks "Add Note" button
2. addNote() method calls fetch() with POST
3. JSON body mein title aur content bheja
4. Flask receives, creates new note, returns it
5. .then(data => { this.notes.push(data) }) → array mein add
6. Vue automatically updates the list in UI
```

## Code Explanation — Key Lines

```javascript
// this.notes = data; → Vue REACTIVE hai
// Jab bhi notes change hoga, UI automatically update hogi
// 'v-for' directive list render karta hai
// Koi manual DOM manipulation nahi!
```

## Important Points for MAD-2

> 🔥 **Very Important List:**
> 1. Vue sirf **UI management** ke liye responsible hai
> 2. Flask **API** provide karta hai data operations ke liye
> 3. Fetch API **bridge** hai frontend aur backend ke beech
> 4. API responses **JSON format** mein hoti hain
> 5. Vue **automatically** UI update karta hai jab data variables change hote hain
> 6. Vue directly **database connect nahi karta**
> 7. Saare database operations **Flask APIs ke through** hone chahiye

---

# 10. Axios — Brief Overview

## Definition

> **Axios** is a popular, third-party JavaScript HTTP client library that provides a simpler API for making HTTP requests compared to the native Fetch API.

## Explanation

Axios ek **npm library** hai jo fetch se zyada features provide karti hai. Jaise ek fancy post office jo automatically receipts, tracking aur error notifications deta hai.

## Fetch vs Axios Comparison

| Feature | Fetch API | Axios |
|---------|-----------|-------|
| Source | Browser built-in | Third-party library |
| JSON parsing | Manual (`response.json()`) | Automatic |
| Error handling | Manual (`response.ok` check) | Automatic HTTP errors |
| Request/Response interceptors | No | Yes |
| Timeout | Manual | Built-in |
| Browser support | Modern browsers | Good backward compatibility |
| Node.js support | No (by default) | Yes |

## Axios Basic Syntax

```javascript
// GET
axios.get("/api/notes")
    .then(response => {
        console.log(response.data); // JSON auto-parsed!
    });

// POST
axios.post("/api/notes", {
    title: "Fetch API",
    content: "Learning"
});
```

> 📝 **Note:** Is course mein **Fetch API** use karte hain (built-in hai, fundamentals samajh aate hain). Axios production projects mein zyada use hota hai.

---

# 11. Important Comparisons

## 11.1 GET vs POST

| Feature | GET | POST |
|---------|-----|------|
| Purpose | Data retrieve karna | Data create/send karna |
| Body | Nahi hota | JSON/FormData body hoti hai |
| URL | Parameters URL mein | Parameters body mein |
| Idempotent | Yes | No |
| Caching | Browser cache karta hai | Nahi |
| Security | URL mein visible | Body mein hidden |
| Example | `fetch("/api/notes")` | `fetch("/api/notes", {method:"POST",...})` |

## 11.2 Synchronous vs Asynchronous

| Feature | Synchronous | Asynchronous |
|---------|-------------|--------------|
| Execution | Blocking | Non-blocking |
| Browser | Hang ho sakta hai | Responsive rehta hai |
| JS nature | Default | Via async/await, Promises |
| Use case | Simple operations | Network/IO operations |

## 11.3 Promise vs Callback vs Async-Await

| Feature | Callback | Promise | Async/Await |
|---------|----------|---------|-------------|
| Readability | Nested, messy | Chained | Synchronous-looking |
| Error handling | Each level | `.catch()` | `try/catch` |
| Debugging | Difficult | Moderate | Easy |
| Syntax | Old style | ES6 | ES2017 |

## 11.4 Vue Lifecycle Hooks — When to Use

| Hook | Use Case |
|------|---------|
| `beforeCreate` | Plugin initialization |
| `created` | Initial data fetch (no DOM needed) |
| `mounted` | **API calls, DOM libraries** (most common) |
| `beforeUpdate` | Pre-render computations |
| `updated` | Post-render DOM operations |
| `beforeDestroy` | Cleanup (event listeners, timers) |
| `destroyed` | Notify parent/siblings |

## 11.5 `created` vs `mounted` for Data Fetching

| | `created` | `mounted` |
|--|-----------|-----------|
| Data available? | ✅ | ✅ |
| DOM available? | ❌ | ✅ |
| Async safe? | Partially | ✅ Yes |
| SSR compatible? | ✅ | ❌ |
| Recommended? | Sometimes | **Most of the time** |

## 11.6 Fetch vs XMLHttpRequest

| Feature | Fetch | XMLHttpRequest |
|---------|-------|----------------|
| API style | Promise-based | Callback-based |
| Code | Clean, modern | Verbose, old |
| Abort | AbortController | `.abort()` |
| Default | GET | GET |
| Browser support | Modern | All |

---

# 12. Important Definitions — Quick Revision

### 1. Separation of Concerns
> A design principle that divides a program into distinct sections, each handling a specific responsibility — frontend manages UI, backend manages data and logic.

### 2. Synchronous Execution
> Code executes line-by-line in a blocking manner; the next operation waits until the current one completes.

### 3. Asynchronous Execution
> Code execution that does not block the main thread; operations run in the background and notify via callbacks, promises, or async/await when complete.

### 4. Callback
> A function passed as an argument to another function, invoked when a particular operation completes or an event occurs.

### 5. Event Loop
> JavaScript's mechanism that continuously monitors the call stack and callback queue, executing queued callbacks when the call stack is empty.

### 6. Promise
> A JavaScript object representing the eventual completion or failure of an asynchronous operation, having three states: pending, fulfilled, or rejected.

### 7. async
> A keyword that declares a function as asynchronous; such a function always returns a Promise automatically.

### 8. await
> A keyword used inside an `async` function that pauses execution until a Promise resolves, making asynchronous code appear synchronous. Can only be used inside `async` functions.

### 9. Fetch API
> A modern, built-in JavaScript interface for making HTTP network requests, returning a Promise that resolves to a Response object.

### 10. Lifecycle Hook (Vue.js)
> A special function in Vue.js automatically called at specific stages of a component's lifecycle (creation, mounting, updating, destruction), allowing developers to execute custom code at the right moment.

### 11. `mounted` Hook
> A Vue.js 2 lifecycle hook called after the component has been mounted to the DOM; `this.$el` is available; the recommended place for initial API calls.

### 12. `created` Hook
> A Vue.js 2 lifecycle hook called after the instance is created; reactive data and methods are available but the DOM is not yet mounted.

### 13. Virtual DOM
> An in-memory representation of the real DOM in Vue.js; changes to data update the virtual DOM first, which is then efficiently reconciled with the real DOM.

### 14. JSON (JavaScript Object Notation)
> A lightweight, text-based data format used for exchanging data between frontend and backend; human-readable and language-independent.

### 15. REST API
> An architectural style for networked applications using stateless HTTP communication and standard methods (GET, POST, PUT, DELETE) to perform CRUD operations.

---

# 13. Important Code Patterns

## Pattern 1: Basic Fetch GET

```javascript
fetch("/api/endpoint")
    .then(response => response.json())
    .then(data => {
        console.log(data);
    })
    .catch(error => console.log("Error:", error));
```

## Pattern 2: Fetch POST with JSON

```javascript
fetch("/api/endpoint", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ key: "value" })
})
.then(response => response.json())
.then(data => console.log(data));
```

## Pattern 3: async/await with Fetch

```javascript
async function getData() {
    try {
        let response = await fetch("/api/endpoint");
        if (!response.ok) throw new Error("HTTP Error: " + response.status);
        let data = await response.json();
        return data;
    } catch(error) {
        console.log(error);
        return null;
    }
}
```

## Pattern 4: Vue mounted + fetchData

```javascript
new Vue({
    el: "#app",
    data: { items: [] },
    methods: {
        fetchItems: function() {
            fetch("/api/items")
                .then(r => r.json())
                .then(data => { this.items = data; });
        }
    },
    mounted: function() {
        this.fetchItems(); // Page load pe data fetch karo
    }
});
```

## Pattern 5: Vue + POST (Add Item)

```javascript
addItem: function() {
    fetch("/api/items", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ title: "New Item" })
    })
    .then(response => response.json())
    .then(newItem => {
        this.items.push(newItem); // Reactively add to list
    });
}
```

## Pattern 6: Flask GET API

```python
@app.route("/api/notes", methods=["GET"])
def get_notes():
    return jsonify(notes)
```

## Pattern 7: Flask POST API

```python
@app.route("/api/notes", methods=["POST"])
def add_note():
    data = request.json
    new_note = {"id": len(notes)+1, "title": data["title"], "content": data["content"]}
    notes.append(new_note)
    return jsonify(new_note)
```

## Pattern 8: Vue Lifecycle All Hooks

```javascript
new Vue({
    beforeCreate() { /* data undefined */ },
    created()      { /* data available, no DOM */ },
    beforeMount()  { /* template ready, no DOM */ },
    mounted()      { /* data + DOM available — BEST FOR API */ },
    beforeUpdate() { /* data changed, old DOM */ },
    updated()      { /* new DOM */ },
    beforeDestroy(){ /* cleanup */ },
    destroyed()    { /* notify */ }
});
```

---

# 14. Exam Preparation

## 14.1 Important Short Questions (with Answers)

### Q1: What is the Fetch API?

**Answer:** The Fetch API is a modern, built-in JavaScript interface used to make HTTP requests from the browser. It returns a Promise that resolves to a Response object.

**Hinglish:** Fetch API ek browser built-in feature hai jo server se data maangne aur bhejne ke liye use hota hai. Ye Promise return karta hai.

---

### Q2: What are the three states of a Promise?

**Answer:** 
1. **Pending** — Operation chal rahi hai, result nahi aaya
2. **Fulfilled** — Operation successful, value mili
3. **Rejected** — Operation failed, error mila

---

### Q3: What is the difference between `created` and `mounted` in Vue 2?

**Answer:**

| | `created` | `mounted` |
|--|-----------|-----------|
| Data available | ✅ | ✅ |
| DOM (`$el`) | ❌ | ✅ |
| Use | Initial data setup | API calls, DOM manipulation |

---

### Q4: Why can't `await` be used outside an `async` function?

**Answer:** `await` can only be used inside an `async` function because it needs the function to return a Promise. Using `await` outside causes a `SyntaxError: await is only valid in async functions`.

---

### Q5: Does Fetch API reject the promise on HTTP errors like 404?

**Answer:** No! Fetch only rejects on **network errors** (no internet, DNS failure). HTTP errors like 404 or 500 will **resolve** the promise but `response.ok` will be `false`. You must manually check `response.ok` and throw an error.

---

### Q6: What is Separation of Concerns in MAD-2 context?

**Answer:** In MAD-2, Separation of Concerns means:
- **Vue.js** handles only the UI (frontend)
- **Flask** handles data operations, business logic (backend)
- They communicate via **HTTP requests** using **JSON**
- Vue never directly accesses the database

---

### Q7: What is the purpose of `JSON.stringify()` in fetch POST?

**Answer:** `JSON.stringify()` converts a JavaScript object into a JSON string, because HTTP request bodies must be text. Flask then parses this JSON string back into Python dict using `request.json`.

---

## 14.2 Important Long Questions

### Q1: Explain the Vue.js 2 Component Lifecycle with diagram and hooks.

**Answer structure:**
1. What is lifecycle (definition)
2. Full diagram with stages
3. Each hook — when called, what's available, use case
4. Arrow function warning
5. Best place to fetch data

### Q2: Explain how Fetch API works with a POST request. Include code.

**Answer structure:**
1. What is Fetch API
2. Basic syntax
3. POST code with headers and JSON.stringify
4. Response handling
5. Error handling (response.ok + .catch)

### Q3: How does Vue.js frontend communicate with Flask backend using Fetch API?

**Answer structure:**
1. Separation of Concerns concept
2. Architecture diagram (Vue → Fetch → Flask → Database)
3. Flask GET and POST API code
4. Vue.js code with fetchNotes and addNote
5. Data flow explanation
6. Vue reactivity (this.notes = data → auto UI update)

---

## 14.3 Coding Questions

### CQ1: Write a Vue 2 component that fetches data on mount and displays it.

```javascript
new Vue({
    el: "#app",
    data: { notes: [] },
    methods: {
        fetchNotes: function() {
            fetch("/api/notes")
                .then(r => r.json())
                .then(data => { this.notes = data; });
        }
    },
    mounted: function() { this.fetchNotes(); }
});
```

### CQ2: Write a Flask API that accepts POST request and adds data.

```python
@app.route("/api/notes", methods=["POST"])
def add_note():
    data = request.json
    note = {"id": len(notes)+1, "title": data["title"], "content": data["content"]}
    notes.append(note)
    return jsonify(note)
```

### CQ3: Write async/await fetch with error handling.

```javascript
async function fetchNotes() {
    try {
        let res = await fetch("/api/notes");
        if (!res.ok) throw new Error("HTTP error: " + res.status);
        let data = await res.json();
        return data;
    } catch(e) {
        console.log("Error:", e);
    }
}
```

---

## 14.4 Viva Questions

1. What happens if you use an arrow function in a Vue lifecycle hook?
2. What is `response.json()` and why does it return a Promise?
3. Can you call `await` directly in the browser console? Why?
4. What is the difference between a network error and an HTTP error in Fetch?
5. Why is `mounted` preferred over `created` for API calls?
6. What does `JSON.stringify()` do and when is it needed?
7. What are the two Promises in a typical fetch call?
8. What is the Event Loop and how does it enable async JavaScript?
9. What happens to `this.grand_total` if you access it in `beforeCreate`?
10. What does `this.$el` give you in the `mounted` hook?

---

## 14.5 Common Mistakes (Exam Traps)

> ⚠️ **Trap 1:** Arrow functions in lifecycle hooks → `this` undefined

> ⚠️ **Trap 2:** Forgetting `Content-Type: application/json` in POST → Flask can't parse

> ⚠️ **Trap 3:** Not using `JSON.stringify()` in body → sends `[object Object]`

> ⚠️ **Trap 4:** Using `await` outside `async` function → SyntaxError

> ⚠️ **Trap 5:** Assuming 404 rejects the Promise → It doesn't! Check `response.ok`

> ⚠️ **Trap 6:** Accessing `this.$el` in `created` → undefined (DOM not yet mounted)

> ⚠️ **Trap 7:** Forgetting second `await` for `response.json()` → gets Promise object instead of data

---

# 15. Quick Revision Sheet

## Keywords

| Keyword | Meaning |
|---------|---------|
| `async` | Function ko asynchronous banao, Promise return karta hai |
| `await` | Kisi Promise ke resolve hone ka wait karo |
| `fetch()` | HTTP request bhejo, Promise return karta hai |
| `response.ok` | HTTP success (200-299) hai ya nahi |
| `response.json()` | Response body ko JSON parse karo |
| `JSON.stringify()` | JS object → JSON string |
| `mounted` | Best Vue lifecycle hook for API calls |
| `this.$el` | Current Vue component ka DOM element |
| `this.data` | Vue reactive data |

## HTTP Methods Quick Ref

| Method | Purpose | Flask Decorator |
|--------|---------|-----------------|
| GET | Data fetch karna | `methods=["GET"]` |
| POST | Naya data create karna | `methods=["POST"]` |
| PUT | Existing data update | `methods=["PUT"]` |
| DELETE | Data delete karna | `methods=["DELETE"]` |

## Vue Lifecycle — 1-Line Summary

```
new Vue() → beforeCreate → created → beforeMount → mounted
                                                       |
                                              (Data changes)
                                       beforeUpdate → updated
                                                       |
                                               (Destroy)
                                       beforeDestroy → destroyed
```

## Fetch — 2 Promises Always

```javascript
// Promise 1 → fetch() itself
// Promise 2 → response.json()
let response = await fetch(url);        // Promise 1
let data     = await response.json();   // Promise 2
```

## Error Handling Cheat Sheet

```
Network Error → catch() automatically
HTTP Error    → check response.ok → manually throw → catch()
await error   → try/catch block
.then() error → .catch() chaining
```

---

# 16. One-Day-Before-Exam Revision

## 🔥 Top 10 Most Important Things

1. **Fetch Promise** never rejects on HTTP errors (404, 500) — only on network errors
2. **`await` sirf `async` ke andar** — warna SyntaxError
3. **Vue `mounted`** = best place for API calls — data + DOM both available
4. **`beforeCreate` mein data undefined** — `created` mein pehli baar data milta hai
5. **`JSON.stringify()`** — POST body mein JS object bhejne se pehle convert karo
6. **`Content-Type: application/json`** — POST request mein header zaroori hai
7. **Arrow functions lifecycle hooks mein mat use karo** — `this` nahi milega
8. **Two promises in fetch** — `fetch()` + `response.json()` — dono await karo
9. **Vue Vue never directly accesses database** — sab kuch Flask API ke through
10. **Separation of Concerns** — Frontend = UI only, Backend = data + logic

## 🎯 Diagram — Frontend to Backend Flow

```
[User Action]
     |
     v
[Vue Method Called]
     |
     v
[fetch("/api/endpoint")]
     |
     | HTTP Request (JSON)
     v
[Flask Route Handler]
     |
     v
[Database Operation]
     |
     v
[jsonify(result)]
     |
     | JSON Response
     v
[response.json() in Vue]
     |
     v
[this.data = result]
     |
     v
[Vue auto-updates UI] ✅
```

## 📝 One-Page Code Summary

```javascript
// === ASYNC/AWAIT PATTERN ===
async function doWork() {
    try {
        let res = await fetch("/api/data");
        if (!res.ok) throw new Error(res.status);
        let data = await res.json();
        return data;
    } catch(e) { console.log(e); }
}

// === VUE + FETCH PATTERN ===
new Vue({
    el: "#app",
    data: { items: [] },
    methods: {
        fetchItems() {
            fetch("/api/items")
                .then(r => r.json())
                .then(d => { this.items = d; });
        },
        addItem() {
            fetch("/api/items", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify({ name: "test" })
            })
            .then(r => r.json())
            .then(item => this.items.push(item));
        }
    },
    mounted() { this.fetchItems(); }
});

// === FLASK API PATTERN ===
@app.route("/api/items", methods=["GET"])
def get_items():
    return jsonify(items)

@app.route("/api/items", methods=["POST"])
def add_item():
    data = request.json
    item = {"id": len(items)+1, "name": data["name"]}
    items.append(item)
    return jsonify(item)
```

## 🚀 Important Differences — One-Liner

- **Sync vs Async:** Sync = blocks browser; Async = browser responsive rehta hai
- **Callback vs Promise:** Callback = nested hell; Promise = clean chaining
- **Promise vs Async-Await:** Same concept, Async-Await = synchronous-looking code
- **created vs mounted:** created = data only; mounted = data + DOM
- **beforeDestroy vs destroyed:** beforeDestroy = cleanup; destroyed = notify
- **fetch vs axios:** fetch = built-in; axios = library with extra features
- **Network error vs HTTP error:** Network = fetch rejects; HTTP = fetch resolves but response.ok = false

---

> 📚 **Resource Links:**
> - MDN Fetch API: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API
> - Vue 2 Lifecycle: https://v2.vuejs.org/v2/guide/instance.html#Lifecycle-Diagram
> - Axios: https://axios-http.com/
> - Using Promises: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises

---

last updated: 2026-08-08
