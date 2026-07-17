**Topics Covered:** Callback Functions, Map, Set, WeakMap, Destructuring, Generators

---

### 1. Callback Functions

**Explanation (Concept-Level):**  
Callback function wo function hota hai jo ek dusre function ko as an argument pass kiya jata hai, aur wo function baad mein call hota hai (jab koi specific task complete ho jaye). JavaScript single-threaded hai aur asynchronous operations (jaise API calls, timers, file I/O) handle karne ke liye callbacks bahut important hain. Yeh **"Do this, and when you're done, call this function"** ka pattern hai.  

**Kyun Important Hai?**  
- Event-driven programming ka backbone (click, scroll, etc.)  
- Asynchronous code ko manage karne ka basic tareeka (before Promises/Async-Await)  
- Higher-order functions (functions that take/return functions) ka core concept  
- **Callback Hell** (Pyramid of Doom) se bachna seekhna padta hai exam mein.

**Key Terms & Definitions:**  
- **Callback**: A function passed as an argument to another function.  
- **Synchronous Callback**: Turant execute hota hai (e.g., Array.forEach).  
- **Asynchronous Callback**: Future mein execute hota hai (e.g., setTimeout, fetch).  
- **Higher-Order Function**: Function jo callback accept karta hai.

**Practical Example + Code Snippet:**
```javascript
// Asynchronous Callback Example (Real-world: API call simulation)
function fetchData(url, callback) {
    console.log("Data fetching started...");
    setTimeout(() => {
        const data = { id: 1, name: "Rahul" };
        callback(data);  // Callback ko baad mein call kiya
    }, 2000);
}

fetchData("https://api.example.com", (result) => {
    console.log("Data received:", result);
});
```

**Relatable Analogy:**  
Jaise tu dost ko bolta hai "Pizza order kar, jab ready ho jaye to mujhe call kar dena" — yahan pizza wala callback hai.

**⭐ Exam Important:**  
- Difference between synchronous vs asynchronous callbacks.  
- Callback Hell aur usko solve karne ke tareeke (Promises, async/await).  
- Event Loop mein callbacks ka role (Microtask vs Macrotask).

**Text-based Flow (Callback Execution):**
```
Main Thread → Higher Order Fn() → [Async Operation starts]
                          ↓ (later)
                   Callback Queue → Event Loop → Execute Callback
```

**Quick Recap:**
- Callback = function passed as argument.
- Handles async behavior in JS.
- Can lead to callback hell if nested deeply.
- Foundation for Promises and modern async code.

---

### 2. Map

**Explanation (Concept-Level):**  
**Map** ek built-in object hai jo key-value pairs store karta hai, jahan **keys kisi bhi data type** (primitive ya object) ki ho sakti hain. Normal Object mein keys sirf strings/symbols hoti hain, lekin Map flexible hai. Yeh insertion order maintain karta hai aur size property se size easily mil jata hai.

**Kyun Important Hai?**  
- Frequent key-value operations mein faster aur more reliable than plain objects.  
- Keys as objects rakh sakte ho (reference-based).  
- No prototype pollution issue jaise normal objects mein hota hai.

**Key Terms:**  
- **Key**: Can be any value (including NaN, objects, functions).  
- **Iteration**: Maintains insertion order.  
- **Methods**: `.set()`, `.get()`, `.has()`, `.delete()`, `.clear()`, `.size`.

**Practical Example + Code:**
```javascript
const userMap = new Map();

userMap.set(1, { name: "Amit", role: "Developer" });
userMap.set("admin", { privileges: ["read", "write"] });
userMap.set({}, "Anonymous User");  // Object as key

console.log(userMap.size);        // 3
console.log(userMap.get(1));      // {name: "Amit", ...}
console.log(userMap.has("admin")); // true
```

**Real-world Use:** Caching API responses with complex keys, or maintaining user sessions.

**Comparison Table: Map vs Plain Object**

| Feature              | Map                          | Plain Object {}             |
|----------------------|------------------------------|-----------------------------|
| Key Types            | Any (object, function, etc.) | Only String/Symbol          |
| Size                 | `.size` property             | Manual (`Object.keys().length`) |
| Insertion Order      | Guaranteed                   | Not reliable (pre-ES2015)   |
| Prototype Pollution  | No                           | Yes (possible)              |
| Iteration            | Easy (`for...of`)            | Possible but clunky         |

**Quick Recap (Map):**
- Keys of any type, maintains order.
- Better performance for frequent add/delete.
- `.size`, no prototype chain issues.
- Preferred over objects for key-value stores in modern JS.

---

### 3. Set

**Explanation (Concept-Level):**  
**Set** unique values ka collection hai. Duplicate values allow nahi karta. Values kisi bhi type ki ho sakti hain aur insertion order maintain karta hai. Yeh mathematical set concept pe based hai (union, intersection etc. possible via methods).

**Kyun Important Hai?**  
- Uniqueness guarantee (e.g., unique user IDs, tags).  
- Fast lookup (O(1) average).  
- Array se duplicates remove karne ka best tareeka.

**Key Terms:**  
- **Uniqueness**: By SameValueZero algorithm (NaN treated as duplicate).  
- **Methods**: `.add()`, `.has()`, `.delete()`, `.clear()`, `.size`.

**Practical Example:**
```javascript
const uniqueTags = new Set(["js", "react", "js", "node"]); // duplicates auto removed

uniqueTags.add("nextjs");
console.log(uniqueTags.size);     // 4
console.log(uniqueTags.has("js")); // true

// Convert to Array
const tagArray = [...uniqueTags];
```

**Analogy:** College fest mein unique entry passes — ek hi banda multiple baar entry nahi kar sakta.

**⭐ Exam Important:** Array se unique elements nikaalne ka common question. Set operations (union via spread).

**Quick Recap (Set):**
- Only unique values.
- Maintains insertion order.
- Fast membership testing.
- Great for removing duplicates from arrays.

---

### 4. WeakMap

**Explanation (Concept-Level):**  
**WeakMap** Map jaisa hi hai lekin keys sirf **objects** (non-primitive) ho sakte hain aur **weakly held** hote hain. Matlab garbage collector keys ko remove kar sakta hai agar koi aur reference na ho. Values bhi koi bhi ho sakti hain.

**Kyun Important Hai?**  
- Memory leaks se bachne ke liye (private data, DOM nodes, etc.).  
- Keys ko automatically cleanup hota hai jab object garbage collected ho jaye.

**Key Differences with Map:**

| Feature             | Map                     | WeakMap                      |
|---------------------|-------------------------|------------------------------|
| Keys                | Any type                | Only Objects                 |
| Garbage Collection  | Strong reference        | Weak reference               |
| Iteration           | Possible                | Not iterable (no `.keys()`)  |
| `.size`             | Available               | Not available                |
| Use Case            | General key-value       | Private data, caching        |

**Practical Example (Private Data):**
```javascript
const privateData = new WeakMap();

class User {
    constructor(name) {
        privateData.set(this, { secret: "12345" });
    }
    
    getSecret() {
        return privateData.get(this).secret;
    }
}

const u = new User("Rahul");
console.log(u.getSecret());  // "12345"
// Agar u = null ho jaye to privateData automatically clean ho jayega
```

**⭐ Exam Important:** Memory management aur weak references ka concept bahut poochha jata hai.

**Quick Recap (WeakMap):**
- Keys must be objects, weakly referenced.
- No iteration, no size.
- Prevents memory leaks in private data scenarios.
- Used with DOM elements, class internals.

*(Note: WeakSet bhi similar hai values ke liye — exam mein poochh sakte hain comparison.)*

---

### 5. Destructuring

**Explanation (Concept-Level):**  
**Destructuring** assignment syntax hai jo arrays ya objects se values ko conveniently extract karke variables mein assign kar deta hai. Yeh ES6 feature hai jo code ko clean aur readable banata hai. Nested destructuring aur default values bhi support karta hai.

**Kyun Important Hai?**  
- Function parameters mein bahut use hota hai.  
- React props, API responses handling mein daily use.  
- Code length kam karta hai.

**Array Destructuring:**
```javascript
const numbers = [10, 20, 30, 40];
const [a, b, ...rest] = numbers;  // a=10, b=20, rest=[30,40]
const [x = 5] = [];               // Default value
```

**Object Destructuring:**
```javascript
const user = { name: "Priya", age: 25, address: { city: "Delhi" } };

const { name, age, address: { city } } = user;  // Nested
const { name: fullName = "Guest" } = user;      // Rename + Default
```

**Function Parameter Destructuring:**
```javascript
function greet({ name = "User", age }) {
    console.log(`Hello ${name}, you are ${age}`);
}

greet(user);
```

**Exam Angle:** Swap variables without temp variable, rest/spread with destructuring.

**Quick Recap (Destructuring):**
- Extracts values from arrays/objects in one line.
- Supports defaults, renaming, nesting, rest.
- Makes code cleaner, especially with function params.
- Powerful with React and API handling.

---

### 6. Generators

**Explanation (Concept-Level):**  
**Generator** ek special function hai jo `function*` syntax se banaya jata hai. Yeh normal functions se alag hai kyunki yeh pause aur resume ho sakta hai using `yield` keyword. Har `.next()` call pe agla yield tak chalta hai aur value return karta hai.

**Kyun Important Hai?**  
- Lazy evaluation (values on demand).  
- Infinite sequences bana sakte ho.  
- Async code ko sync style mein likhne mein madad (with async generators).  
- Iterators aur custom iterables implement karne ke liye best.

**Key Terms:**  
- `function*`: Generator function.  
- `yield`: Pause + return value.  
- `generator.next()`: Returns `{value, done}`.  
- `return()` / `throw()`: Early termination.

**Practical Example:**
```javascript
function* numberGenerator() {
    yield 1;
    yield 2;
    yield 3;
    return "Done";
}

const gen = numberGenerator();

console.log(gen.next());  // {value: 1, done: false}
console.log(gen.next());  // {value: 2, done: false}
console.log(gen.next());  // {value: 3, done: false}
console.log(gen.next());  // {value: "Done", done: true}
```

**Infinite Sequence Example:**
```javascript
function* infiniteCounter() {
    let i = 0;
    while (true) {
        yield i++;
    }
}

const counter = infiniteCounter();
console.log(counter.next().value); // 0
console.log(counter.next().value); // 1
```

**Text-based Flow:**
```
Call generator() → Returns Iterator
.next() → Execute till yield → Pause + return {value, done}
.next() again → Resume from previous yield
```



**Comparison: Generator vs Normal Function**

| Aspect            | Normal Function       | Generator                     |
|-------------------|-----------------------|-------------------------------|
| Execution         | Runs to completion    | Can pause/resume              |
| Return            | Single value          | Multiple via yield            |
| Memory            | All at once           | Lazy (on demand)              |
| Use with for..of  | No                    | Yes (iterable)                |
