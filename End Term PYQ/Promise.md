$$
\boxed{\textbf{Promise}}
$$

# **Question:**

```js
new Promise((error, pass) => {
  if (5 === "5") error(5)
  else pass(8)
})
.then(d => {
  console.log("Checkpoint 4", d);
  throw new Error(20);
  return d * 5;
})
.then(d => {
  console.log("Checkpoint 2", d);
  return d;
}, d => {
  console.log("Checkpoint 5", d.message);
  return d.message * 2;
})
.catch(e => {
  console.log("Checkpoint 3", e.message);
  return e.message * 2;
})
.finally(d => {
  console.log("Checkpoint 1", d);
  return d * 5;
})
.then(d => {
  console.log("Checkpoint 6", d);
  return d * 5;
})
```

What will be the output of the above program?

**Options:**

- ✗ 

Checkpoint 4 5  
Checkpoint 5 20  
Checkpoint 1 undefined  
Checkpoint 6 40

- ✗ 

Checkpoint 4 5  
Checkpoint 3 20  
Checkpoint 1 undefined  
Checkpoint 6 NaN

- ✓

Checkpoint 5 undefined  
Checkpoint 1 undefined  
Checkpoint 6 NaN

## **Explanation:**

# Question 5: JavaScript Promise Chain 🎯

---

## 🧠 Pehle Concept Samjho — Promise Kya Hota Hai?

> **Real-life Analogy:**
>
> 🍕 **Promise = Zomato Order**
> - Order place hua → **Pending**
> - Order deliver hua → **Resolved/Fulfilled** ✅
> - Order cancel hua → **Rejected** ❌
>
> Chain ke har step pe decide hota hai — aage kya hoga!

---

## 🔥 SABSE BADA TRAP — Parameter Names! ⚠️

```javascript
new Promise((error, pass) => {
//           ↑       ↑
//    Naam = error   Naam = pass
//    
//    Lekin POSITION matter karta hai, NAAM nahi!
//    Position 1 = RESOLVE (chahe kuch bhi naam do)
//    Position 2 = REJECT  (chahe kuch bhi naam do)
```

```
NORMAL convention:         IS QUESTION MEIN:
new Promise((resolve, reject)   new Promise((error, pass)
             ↑        ↑                      ↑      ↑
           1st param  2nd param            1st=RESOLVE  2nd=REJECT

error = Actually RESOLVE hai! 😱
pass  = Actually REJECT hai!  😱
```

> 🎯 **Yahi hai question setter ki sabse badi chaal!** Student sochta hai `error(5)` reject karega aur `pass(8)` resolve karega — ULTA hota hai!

---

## 🔍 Code Ko Piece-By-Piece Samjho

```javascript
// ════════ FULL CODE ════════
new Promise((error, pass) => {
    if (5 === "5") error(5)      // Line A
    else pass(8)                  // Line B
}).then(d => {                    // Block 1
    console.log("Checkpoint 4", d);
    throw new Error(20);
    return d * 5;
})
.then(d => {                      // Block 2 — onFulfilled
    console.log("Checkpoint 2", d);
    return d;
}, d => {                         // Block 2 — onRejected
    console.log("Checkpoint 5", d.message);
    return d.message * 2;
}).catch(e => {                   // Block 3
    console.log("Checkpoint 3", e.message);
    return e.message * 2;
}).finally(d => {                 // Block 4
    console.log("Checkpoint 1", d);
    return d * 5;
}).then(d => {                    // Block 5
    console.log("Checkpoint 6", d);
    return d * 5;
})
```

---

## 🗺️ Step-by-Step Execution Trace

### STEP 1: Promise Constructor

```javascript
new Promise((error, pass) => {
    if (5 === "5") error(5)   // ← 5 === "5" ?
    else pass(8)
})
```

```
5 === "5"  →  Strict Equality Check!
              5   = number
             "5"  = string
             TYPE alag hai!
             
             RESULT = FALSE ❌

Toh: else pass(8) chalega
     pass = 2nd parameter = REJECT function
     
     ∴ Promise REJECTED with value = 8
```

```
Promise Status: ❌ REJECTED (value = 8)
```

---

### STEP 2: Block 1 — `.then(onFulfilled only)`

```javascript
.then(d => {
    console.log("Checkpoint 4", d);   // ← Chalega?
    throw new Error(20);
    return d * 5;
})
```

```
Promise abhi REJECTED hai
Block 1 mein sirf onFulfilled callback hai
REJECTED promise → onFulfilled SKIP hota hai!

Checkpoint 4 → ❌ PRINT NAHI HOGA!

Promise Status: ❌ REJECTED (value = 8) — same as before
```

---

### STEP 3: Block 2 — `.then(onFulfilled, onRejected)` ← KEY STEP!

```javascript
.then(d => {                         // onFulfilled
    console.log("Checkpoint 2", d);
    return d;
}, d => {                            // onRejected ← YEH CHALEGA!
    console.log("Checkpoint 5", d.message);
    return d.message * 2;
})
```

```
Promise REJECTED hai → onRejected callback chalega (2nd function)
d = 8 (rejected value)

console.log("Checkpoint 5", d.message)
                              ↑
                         d = 8 (number!)
                         8.message = undefined (numbers ka .message nahi hota!)

🖨️ PRINTS: "Checkpoint 5 undefined" ✅

return d.message * 2
→ undefined * 2
→ NaN

onRejected ne RETURN kiya (throw nahi kiya)
→ Promise ab RESOLVED ho gaya NaN ke saath!
```

```
Promise Status: ✅ RESOLVED (value = NaN)
```

---

### STEP 4: Block 3 — `.catch()`

```javascript
.catch(e => {
    console.log("Checkpoint 3", e.message);
    return e.message * 2;
})
```

```
Promise ab RESOLVED hai (NaN ke saath)
.catch() sirf REJECTED promise pe chalta hai

Checkpoint 3 → ❌ PRINT NAHI HOGA!

Promise Status: ✅ RESOLVED (value = NaN) — same
```

---

### STEP 5: Block 4 — `.finally()` ← IMPORTANT BEHAVIOR!

```javascript
.finally(d => {
    console.log("Checkpoint 1", d);
    return d * 5;
})
```

```
.finally() HAMESHA chalta hai — Resolved ho ya Rejected! ✅

LEKIN — .finally() ka ek special rule hai:
╔═══════════════════════════════════════════════════════╗
║  .finally() ke handler ko KOI VALUE NAHI MILTI!      ║
║  d = undefined HAMESHA                                ║
║                                                       ║
║  .finally() ka return value IGNORE hota hai!         ║
║  Pehle wali value (NaN) aage pass hoti hai!          ║
╚═══════════════════════════════════════════════════════╝

console.log("Checkpoint 1", d)
→ d = undefined (finally ko value nahi milti!)

🖨️ PRINTS: "Checkpoint 1 undefined" ✅

return d * 5 → undefined * 5 → NaN
Lekin yeh IGNORED hai! Promise NaN ke saath hi aage jaata hai.
```

```
Promise Status: ✅ RESOLVED (value = NaN) — finally ne change nahi kiya
```

---

### STEP 6: Block 5 — Last `.then()`

```javascript
.then(d => {
    console.log("Checkpoint 6", d);
    return d * 5;
})
```

```
Promise RESOLVED hai (NaN ke saath)
onFulfilled chalega, d = NaN

console.log("Checkpoint 6", NaN)

🖨️ PRINTS: "Checkpoint 6 NaN" ✅
```

---

## 🗺️ Complete Flow Diagram

```
new Promise((error, pass) => ...)
           │
           │  5 === "5" → FALSE
           │  pass(8) → REJECT(8)
           ▼
    ❌ REJECTED (8)
           │
           ▼
    Block 1: .then(onFulfilled)
           │  REJECTED → SKIP ⏭️
           ▼
    ❌ REJECTED (8) — same
           │
           ▼
    Block 2: .then(fulfilled, rejected)
           │  REJECTED → onRejected runs!
           │  d = 8
           │  d.message = undefined
           │  🖨️ "Checkpoint 5 undefined"
           │  return undefined * 2 = NaN
           │  onRejected returned → now RESOLVED!
           ▼
    ✅ RESOLVED (NaN)
           │
           ▼
    Block 3: .catch()
           │  RESOLVED → SKIP ⏭️
           ▼
    ✅ RESOLVED (NaN) — same
           │
           ▼
    Block 4: .finally()
           │  ALWAYS RUNS! ✅
           │  d = undefined (finally gets no value!)
           │  🖨️ "Checkpoint 1 undefined"
           │  return ignored → NaN passes through
           ▼
    ✅ RESOLVED (NaN) — same
           │
           ▼
    Block 5: .then(onFulfilled)
           │  RESOLVED → runs!
           │  d = NaN
           │  🖨️ "Checkpoint 6 NaN"
           ▼
         DONE!
```

---

## 📊 Execution Summary Table

| Step | Block | Status In | Chalega? | Output | Status Out |
|---|---|---|---|---|---|
| 1 | Constructor | — | ✅ | — | ❌ REJECTED(8) |
| 2 | `.then()` Block 1 | ❌ REJ | ❌ Skip | — | ❌ REJECTED(8) |
| 3 | `.then()` Block 2 | ❌ REJ | ✅ onRejected | **"Checkpoint 5 undefined"** | ✅ RESOLVED(NaN) |
| 4 | `.catch()` | ✅ RES | ❌ Skip | — | ✅ RESOLVED(NaN) |
| 5 | `.finally()` | ✅ RES | ✅ Always | **"Checkpoint 1 undefined"** | ✅ RESOLVED(NaN) |
| 6 | `.then()` Block 5 | ✅ RES | ✅ | **"Checkpoint 6 NaN"** | Done |

---

## ✅ Final Output

```
Checkpoint 5 undefined
Checkpoint 1 undefined
Checkpoint 6 NaN
```

**Answer: Option 3 (6406532868071) ✅**

---

## 💡 Key Rules — Yaad Rakhne Wali Baatein

```
┌─────────────────────────────────────────────────────┐
│              PROMISE CHAIN RULES                    │
├─────────────────────────────────────────────────────┤
│                                                     │
│  1. PARAMETER NAAM nahi, POSITION matter karta hai  │
│     (error, pass) → error=RESOLVE, pass=REJECT      │
│                                                     │
│  2. .then(onFulfilled) → sirf resolved pe chalta    │
│     .then(f, r)        → r sirf rejected pe chalta  │
│                                                     │
│  3. .catch() = .then(undefined, onRejected)         │
│     Sirf rejected promise pe chalta hai             │
│                                                     │
│  4. .finally() → HAMESHA chalta hai                 │
│     ❌ Value nahi milti (d = undefined)             │
│     ❌ Return value ignore hota hai                 │
│     ✅ Pehle wali value aage pass hoti hai          │
│                                                     │
│  5. onRejected handler agar return kare (throw na   │
│     kare) → Promise RESOLVED ho jaata hai!          │
│                                                     │
└─────────────────────────────────────────────────────┘
```

---





## Doubt: 
**ham code me .then(onFulfilled only) kaise hamko pata nahi karne aata hai or ye bahi .then(onFulfilled, onRejected) pata nahi karne aata hain**

# `.then()` ke Types Samjho — Once and For All! 🎯

---

## 🧠 Pehle Basic Samjho — `.then()` Kya Accept Karta Hai?

> **Real-life Analogy:**
>
> 🏥 **Doctor ke paas jaana = Promise**
> - **Test normal aaya (Resolved)** → Doctor treatment batata hai ✅
> - **Test abnormal aaya (Rejected)** → Doctor alag advice deta hai ❌
>
> Tum doctor ko bol sakte ho:
> - "Sirf normal result pe batao" → **One handler**
> - "Dono cases mein batao" → **Two handlers**

---

## 👁️ Visually Kaise Pehchano? — Sirf Yeh Dekho!

```javascript
// ════════════════════════════════════════════
// RULE: .then() ke andar kitne ARROW FUNCTIONS hain?
// ════════════════════════════════════════════

// TYPE 1: EK arrow function = onFulfilled ONLY
.then(d => {
    console.log(d);   // Sirf ek function!
})                    // Count karo: 1️⃣

// TYPE 2: DO arrow functions = onFulfilled + onRejected  
.then(d => {          // Pehla function (fulfilled)
    console.log(d);
}, d => {             // ← YEH COMMA + DOOSRA FUNCTION dekho!
    console.log(d);   // Doosra function (rejected)
})                    // Count karo: 2️⃣
```

### 🔑 Simple Rule:
```
.then(  [1 function]  )         → onFulfilled ONLY
.then(  [function] , [function] ) → onFulfilled + onRejected
              ↑
         COMMA hai?
         Haan → 2 handlers!
         Nahi → 1 handler!
```

---

## 🔍 Wahi Question Ka Code — Ab Clearly Dekho

```javascript
// ─────────────────────────────────────
// BLOCK 1: Sirf EK function → onFulfilled ONLY
// ─────────────────────────────────────
.then(d => {                     // ← Sirf 1️⃣ function
    console.log("Checkpoint 4", d);
    throw new Error(20);
    return d * 5;
})
// Koi comma nahi, koi doosra function nahi
// = onFulfilled ONLY
// = REJECTED aaya toh SKIP!


// ─────────────────────────────────────
// BLOCK 2: DO functions → onFulfilled + onRejected
// ─────────────────────────────────────
.then(d => {                     // ← Pehla function 1️⃣ (onFulfilled)
    console.log("Checkpoint 2", d);
    return d;
}, d => {                        // ← COMMA! Phir doosra function 2️⃣
    console.log("Checkpoint 5", d.message);
    return d.message * 2;
})
// Comma ke baad doosra function hai!
// = onFulfilled + onRejected DONO hain!
```

---

## 📊 Teeno Types Ek Table Mein

```
.then() ke TEEN forms hote hain:
```

| Form | Syntax | Kab Chalta Hai |
|---|---|---|
| **Only Fulfilled** | `.then(fn)` | Sirf ✅ Resolved pe |
| **Both Handlers** | `.then(fn, fn)` | Resolved → 1st fn, Rejected → 2nd fn |
| **Only Rejected** | `.then(null, fn)` | Sirf ❌ Rejected pe (rare) |

---

## 💻 Live Examples — Khud Dekho Kya Hota Hai

```javascript
// ════════════════════════════════════
// CASE 1: .then(onFulfilled ONLY)
// ════════════════════════════════════

Promise.resolve(10)        // ✅ Resolved with 10
  .then(d => {             // 1️⃣ Sirf ek function
    console.log("Chala!", d);  // ✅ PRINT HOGA: "Chala! 10"
  });

Promise.reject("Error!")   // ❌ Rejected
  .then(d => {             // 1️⃣ Sirf ek function
    console.log("Chala!", d);  // ❌ SKIP! Print nahi hoga
  });
//       ↑
// Rejected aaya → onFulfilled skip!


// ════════════════════════════════════
// CASE 2: .then(onFulfilled, onRejected)
// ════════════════════════════════════

Promise.resolve(10)        // ✅ Resolved with 10
  .then(
    d => {                 // 1️⃣ Pehla function
      console.log("Fulfilled!", d); // ✅ PRINT: "Fulfilled! 10"
    },
    d => {                 // 2️⃣ Doosra function (comma ke baad!)
      console.log("Rejected!", d);  // ❌ Skip hoga
    }
  );

Promise.reject("Oops!")    // ❌ Rejected
  .then(
    d => {                 // 1️⃣ Pehla function
      console.log("Fulfilled!", d); // ❌ Skip hoga
    },
    d => {                 // 2️⃣ Doosra function
      console.log("Rejected!", d);  // ✅ PRINT: "Rejected! Oops!"
    }
  );
```

---

## 🗺️ Decision Flow — Jab Bhi Code Dekho, Yeh Karo

```
.then() dekha?
      │
      ▼
Andar COMMA hai?
   /        \
 NAI         HAAN
  │            │
  ▼            ▼
Sirf 1      2 functions!
function!   /           \
  │      1st fn        2nd fn
  ▼      (onFulfilled) (onRejected)
onFulfilled    │              │
ONLY!          ▼              ▼
               ✅ Resolved   ❌ Rejected
               pe chalta     pe chalta
```

---

## 🎯 Quick Trick — Comma Counting Method

```javascript
// EXAM MEIN YEH KARO:
// .then() ka bracket open karo mentally
// Andar COMMA dhundo jo functions ko separate kare

.then(d => { ... })
//          ↑
//    Koi COMMA nahi functions ke beech
//    = 1 HANDLER (onFulfilled only)


.then(d => { ... }, d => { ... })
//                 ↑
//           COMMA hai! Functions ke beech!
//           = 2 HANDLERS (both!)
```

---

## 🧪 Ab Wahi Question Dobara Dekho — Crystal Clear!

```javascript
new Promise((error, pass) => {
    if (5 === "5") error(5)
    else pass(8)            // pass = REJECT → REJECTED(8)
})

// ┌─────────────────────────────────────────┐
// │ BLOCK 1: .then( ONE function )          │
// │ = onFulfilled ONLY                      │
// │ Comma hai? ❌ Nahi                      │
// │ Promise REJECTED hai → SKIP ⏭️          │
// └─────────────────────────────────────────┘
.then(d => {
    console.log("Checkpoint 4", d);
    throw new Error(20);
    return d * 5;
})

// ┌─────────────────────────────────────────┐
// │ BLOCK 2: .then( fn , fn )               │
// │ = BOTH handlers!                        │
// │ Comma hai? ✅ Haan!                     │
// │ Promise REJECTED hai → 2nd fn chala!    │
// └─────────────────────────────────────────┘
.then(d => {
    console.log("Checkpoint 2", d); // ← 1st fn (onFulfilled) SKIP
    return d;
}, d => {                           // ← COMMA = 2nd fn (onRejected)✅
    console.log("Checkpoint 5", d.message); // CHALA! d=8, d.message=undefined
    return d.message * 2;           // undefined*2 = NaN → now RESOLVED!
})
```

---

## 📝 Yaad Rakhne Ka Formula

```
╔═══════════════════════════════════════════════════╗
║                                                   ║
║  .then( singleFn )     → Fulfilled ONLY           ║
║                                                   ║
║  .then( fn1 , fn2 )   → Fulfilled + Rejected      ║
║              ↑                                    ║
║           COMMA = 2 handlers ka sign!             ║
║                                                   ║
║  .catch( fn )          → Rejected ONLY            ║
║                       = .then(null, fn) hi hai!   ║
║                                                   ║
║  .finally( fn )        → HAMESHA, but             ║
║                          value nahi milti!        ║
║                                                   ║
╚═══════════════════════════════════════════════════╝
```

---





### Doubt
```javascript
Promise.resolve(10)        // ✅ Resolved with 10
  .then(
    d => {                 // 1️⃣ Pehla function
      console.log("Fulfilled!", d); // ✅ PRINT: "Fulfilled! 10"
    },
    d => {                 // 2️⃣ Doosra function (comma ke baad!)
      console.log("Rejected!", d);  // ❌ Skip hoga
    }
  );
````

**is me Pehla function print hua to dusra funstion nahi chala agar Pehla function reject hota hai to dusra chalta hai**

# Bilkul Sahi Samjha! 🎯 — Ab Aur Deep Samjho

---

## ✅ Tera Understanding Correct Hai!

```
.then( fn1 , fn2 )

Promise RESOLVED → fn1 chalta hai, fn2 NAHI
Promise REJECTED → fn2 chalta hai, fn1 NAHI

Dono KABHI SAATH nahi chalte! Ek hi chalta hai!
```

> **Real-life Analogy:**
> 🏏 **Cricket Match = Promise**
> - India **Jeeta** (Resolved) → Celebration party 🎉 (fn1 chali)
> - India **Haara** (Rejected) → Post-mortem meeting 😢 (fn2 chali)
>
> **Dono ek saath nahi hoti! Ya party, ya meeting!**

---

## 💻 Clearly Dekho — 4 Cases

```javascript
// ════════════════════════════════════════
// CASE 1: Resolved → Pehla chala, Doosra NAHI
// ════════════════════════════════════════
Promise.resolve(10)          // ✅ Resolved!
  .then(
    d => {
      console.log("Fulfilled!", d);  // ✅ CHALA → "Fulfilled! 10"
    },
    d => {
      console.log("Rejected!", d);   // ❌ NAHI CHALA (skip!)
    }
  );

// Output: "Fulfilled! 10"


// ════════════════════════════════════════
// CASE 2: Rejected → Doosra chala, Pehla NAHI
// ════════════════════════════════════════
Promise.reject("Oops!")      // ❌ Rejected!
  .then(
    d => {
      console.log("Fulfilled!", d);  // ❌ NAHI CHALA (skip!)
    },
    d => {
      console.log("Rejected!", d);   // ✅ CHALA → "Rejected! Oops!"
    }
  );

// Output: "Rejected! Oops!"
```

---

## 🗺️ Visual Diagram — Ek Baar Mein Samjho

```
Promise
  │
  ├── ✅ RESOLVED (value)
  │         │
  │         ▼
  │    .then( fn1 , fn2 )
  │         │
  │         ▼
  │      fn1 ✅ CHALA
  │      fn2 ❌ SKIP
  │
  └── ❌ REJECTED (error)
            │
            ▼
       .then( fn1 , fn2 )
            │
            ▼
         fn1 ❌ SKIP
         fn2 ✅ CHALA
```

---

## 🔑 OR Gate Wala Logic — Exactly Yahi Hai!

```
.then(fn1, fn2) = SWITCH ki tarah kaam karta hai

        ┌─────────────────────┐
        │    PROMISE RESULT   │
        └─────────────────────┘
                  │
        ┌─────────┴─────────┐
        │                   │
   RESOLVED             REJECTED
        │                   │
        ▼                   ▼
      fn1() ✅           fn2() ✅
        │                   │
      fn2() ❌           fn1() ❌
       SKIP                SKIP
```

---

## 🧪 Ab Question Wala Code — Confirm Karo

```javascript
.then(
  d => {                              // fn1 = onFulfilled
    console.log("Checkpoint 2", d);
    return d;
  },
  d => {                              // fn2 = onRejected
    console.log("Checkpoint 5", d.message);
    return d.message * 2;
  }
)

// Promise REJECTED(8) tha → fn2 chali ✅
// fn1? → SKIP ❌
// 
// "Checkpoint 2" → NAHI AAYA output mein
// "Checkpoint 5" → AAYA output mein ✅
```

---

## ⚡ 3 Lines Mein Summary

```
╔══════════════════════════════════════════════════╗
║                                                  ║
║  RESOLVED  →  fn1 chalta hai, fn2 nahi           ║
║  REJECTED  →  fn2 chalta hai, fn1 nahi           ║
║  Dono kabhi SAATH nahi chalte — EVER!            ║
║                                                  ║
╚══════════════════════════════════════════════════╝
```

