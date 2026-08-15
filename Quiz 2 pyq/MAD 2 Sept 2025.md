$$
\boxed{\textbf{MAD 2 Sept 2025 Quiz 2 PYQ SOLUTION}}
$$

# **Question 1:**


**Consider the following Vue application with markup `index.html` and JavaScript file `app.js`.**

**`index.html`:**

```html
<div id="app">
    <card-component>
        <template #title>
            Welcome Message
        </template>

        <p>Main card content here</p>

        <template #default>
            Default slot content
        </template>

        <template #actions>
            <button>Click Me</button>
        </template>
    </card-component>
</div>

<script src="app.js"></script>
````

**`app.js`:**

```javascript
Vue.component("cardComponent", {
    template: `
        <div class="card">
            <h2>
                <slot name="title"></slot>
            </h2>

            <div class="content">
                <slot></slot>
            </div>

            <div class="footer">
                <slot name="footer"></slot>
            </div>
        </div>
    `,
});

const app = new Vue({
    el: "#app"
});
```

**Suppose you open the `index.html` file in a browser. What will be rendered by the browser?**

**Options :**

* **A** ✗

  * Welcome Message
  * Main card content here
  * Default slot content
  * Click Me

* **B** ✓

  * Welcome Message
  * Default slot content

* **C** ✗

  * Welcome Message
  * Main card content here
  * Default slot content

* **D** ✗

  * Welcome Message
  * Main card content here

* **E** ✗

  * Welcome Message
  * Default slot content
  * Click Me


### **Answer: B** ✅



## Pehle Basic Concept: Slots kya hote hain

Slots Vue mein ek "placeholder" hote hain jo child component define karta hai, aur parent us placeholder mein content **inject (bhar)** kar sakta hai.

```js
<slot name="title"></slot>     // named slot — parent yahan specific content bhejta hai "title" naam se
<slot></slot>                  // default slot (unnamed) — jo bhi content bina naam ke aaye, yahan jaata hai
```

Parent side pe:
```html
<template #slotname>...</template>   // #slotname = v-slot:slotname ka shorthand
```
Aur agar koi content `<template>` mein wrap **nahi** hai, to wo automatically **default slot** mein chala jata hai.

## Ab Child Component (`cardComponent`) ka structure dekho

```js
template: `
    <div class="card">
        <h2>
            <slot name="title"></slot>      <!-- Slot #1: naam "title" -->
        </h2>
        <div class="content">
            <slot></slot>                    <!-- Slot #2: koi naam nahi = "default" -->
        </div>
        <div class="footer">
            <slot name="footer"></slot>      <!-- Slot #3: naam "footer" -->
        </div>
    </div>
`
```

**Important cheez yaad rakho:** Child component mein sirf **3 slots** define hain — `title`, default (unnamed), aur `footer`. Bas itne hi placeholders exist karte hain jaha parent content daal sakta hai.

## Ab Parent side (index.html) line by line dekho

```html
<card-component>
    <template #title>
        Welcome Message
    </template>
```
→ Ye "title" naam ke slot ko target karta hai. Child mein `<slot name="title">` maujood hai, isliye ye **render hoga** → "Welcome Message" ✅

```html
    <p>Main card content here</p>
```
→ Ye **koi `<template>` wrapper mein nahi hai**, isliye Vue ise automatically **default slot** ka content maan lega (implicit default).

```html
    <template #default>
        Default slot content
    </template>
```
→ Ye **explicitly** default slot ko target karta hai (`#default` likh ke).

**Yahi hai is question ka twist point** 👇

## Conflict: Do baar Default Slot Define Karna

Ab humare paas **default slot ke liye do alag content sources** hain:
1. `<p>Main card content here</p>` (implicit — bina template wrapper ke)
2. `<template #default>Default slot content</template>` (explicit)

Jab Vue compile karta hai, andar ye ek object banata hai jisme har slot ka naam **key** hota hai:
```js
slots = {
  title: "Welcome Message",
  default: <p>Main card content here</p>,   // pehle assign hua
  default: "Default slot content"            // dobara "default" key assign hua → OVERWRITE!
}
```

Jaise JavaScript object mein agar same key do baar likho, to **baad wali value pehli ko overwrite (replace) kar deti hai** — waisa hi yahan hota hai. Vue ke internal slot-resolving mechanism mein jo **baad mein declare hua "default" slot hota hai, wahi final render hota hai**, pehla wala (`<p>Main card content here</p>`) **discard** ho jata hai.

Isliye **"Main card content here" screen par nahi dikhega**, sirf **"Default slot content"** dikhega.

```html
    <template #actions>
        <button>Click Me</button>
    </template>
</card-component>
```
→ Ye "actions" naam ke slot ko target kar raha hai. Lekin child component mein `<slot name="actions">` **exist hi nahi karta** (child ke paas sirf title, default, footer hain). Isliye ye content **kahin bhi render nahi hoga** — bina destination ke content simply **discard** ho jata hai.

Aur `<slot name="footer">` — child mein hai, lekin parent ne **koi content "footer" ke liye diya hi nahi**, isliye footer div **empty** rahega.

## Final Render Result

| Slot | Kya render hua | Kyun |
|---|---|---|
| `title` | "Welcome Message" | Direct match mila |
| default (`content` div) | "Default slot content" | Last-declared default ne pehle wale ko overwrite kar diya |
| `footer` | (kuch nahi) | Koi content assign nahi kiya gaya |
| "actions" | (kuch nahi render hua) | Child mein aisa slot exist hi nahi karta |

Isliye final answer **Option B**: 
- Welcome Message
- Default slot content

## Baaki Options kyun galat hain

- **A** — "Main card content here" aur "Click Me" dono ko include karta hai, jo ki galat hai (overwrite aur missing-slot ke wajah se render nahi hote)
- **C** — "Main card content here" ko bhi include kar raha hai, jo overwrite ho chuka hai
- **D** — "Default slot content" hi missing kar diya, jabki wahi actually final render hota hai
- **E** — "Click Me" ko include kar raha hai jo ki "actions" slot destination na hone ki wajah se render hi nahi hoga

## Exam ke liye Key Takeaway 🎯

> Jab ek hi named slot (jaise `default`) ko **implicit** (bina template wrapper) aur **explicit** (`<template #default>`) dono tariko se content diya jaye, to **jo baad mein (source code mein niche) likha hota hai, wahi final render hota hai** — pehle wala silently overwrite/discard ho jata hai.

Aur dusra important point: **agar parent kisi aise slot-name ko target kare jo child mein define hi nahi hai**, to wo content kahin bhi nahi dikhta — silently ignore ho jata hai (koi error nahi aata).

# **Question 2:**


**Consider the below JavaScript program.**

```javascript
new Promise((resolve, reject) => {
    if (10 > 5) reject(15)
    else resolve(25)
})
.then(d => {
    console.log("Success handler", d);
    return d * 2;
}, d => {
    console.log("Error handler", d);
    throw new Error(30);
})
.catch(e => {
    console.log("Catch block", e.message);
    return e.message / 2;
})
.then(d => {
    console.log("Next then", d);
    return d + 10;
})
.finally(d => {
    console.log("Finally block", d);
    return d * 100;
})
.then(d => {
    console.log("Final then", d);
    return d * 3;
})
````

**What will be the output of the above program?**

**Options :**

* **A.** ✗

  ```text
  Error handler 15
  Catch block 30
  Next then 15
  Finally block 30
  Final then 25
  ```

* **B.** ✗

  ```text
  Success handler 25
  Next then 50
  Finally block undefined
  Final then 60
  ```

* **C.** ✗

  ```text
  Error handler 15
  Catch block 30
  Next then 15
  Finally block 25
  Final then 75
  ```

* **D.** ✓

  ```text
  Error handler 15
  Catch block 30
  Next then 15
  Finally block undefined
  Final then 25
  ```


### **Answer: D** ✅

 Ye question mainly **3 concepts** test karta hai: `.then()` ka dual-handler, `.catch()` mein error object, aur **`.finally()` ka special behavior**.

## Step 1: Promise Create Hona

```javascript
new Promise((resolve, reject) => {
    if (10 > 5) reject(15)
    else resolve(25)
})
```

- `10 > 5` → **true** hai
- Isliye `reject(15)` call hota hai — Promise **rejected** state mein chala gaya, reason = `15`

**Yaad rakho:** Ye ek common trap hai — log condition dekhe bina assume kar lete hain `resolve` chalega, lekin yahan `reject` hi chalega kyunki `10 > 5` sach hai.

## Step 2: Pehla `.then()` — Dual Handler Concept

```javascript
.then(d => {
    console.log("Success handler", d);
    return d * 2;
}, d => {
    console.log("Error handler", d);
    throw new Error(30);
})
```

`.then()` **do arguments** le sakta hai:
- **1st argument** = success handler (jab promise resolve ho)
- **2nd argument** = error handler (jab promise reject ho) — ye `.catch()` jaisa hi kaam karta hai

Chunki Promise **rejected(15)** hai, sirf **2nd handler (error handler)** chalega:
```javascript
console.log("Error handler", 15);   // Output: "Error handler 15"
throw new Error(30);
```

`throw new Error(30)` — ye ek **Error object** banata hai. Important: `Error(30)` mein `30` ek **number hai but Error constructor use ise string mein convert kar deta hai** — `error.message` hamesha **string** hota hai (`"30"`, not `30`).

Jab andar `throw` hota hai, to us `.then()` se return hone wala **naya promise reject ho jata hai** us thrown Error ke sath.

## Step 3: `.catch()` — Yahan Error Object Milta Hai

```javascript
.catch(e => {
    console.log("Catch block", e.message);
    return e.message / 2;
})
```

- Pichle step se jo Error throw hua tha, `.catch()` usko catch karta hai
- `e` = poora Error object hai (na ki sirf 30)
- `e.message` = `"30"` (**string**, kyunki Error constructor number ko string bana deta hai)

```javascript
console.log("Catch block", "30");   // Output: "Catch block 30"
return e.message / 2;               // "30" / 2
```

Yahan JavaScript ka **type coercion** kaam karta hai — `"30" / 2` mein JS automatically string `"30"` ko number `30` mein convert kar deta hai (kyunki `/` operator sirf numbers ke sath kaam karta hai), to result = **15**.

**Important concept:** `.catch()` agar error ko **throw nahi karta**, balki koi normal value **return** karta hai, to chain wapas **resolved state** mein chali jaati hai. Isliye aage wale `.then()` normal success handler ki tarah chalenge.

## Step 4: Doosra `.then()`

```javascript
.then(d => {
    console.log("Next then", d);
    return d + 10;
})
```

- `d = 15` (pichle catch se aaya)
```javascript
console.log("Next then", 15);   // Output: "Next then 15"
return 15 + 10;                  // returns 25
```

## Step 5: `.finally()` — Sabse Bada Trap 🎯

```javascript
.finally(d => {
    console.log("Finally block", d);
    return d * 100;
})
```

Ye is **poore question ka sabse important concept** hai:

> **`.finally()` ka callback KOI bhi argument receive nahi karta — chahe pichla promise resolve hua ho ya reject.** `.finally()` sirf ye janne ke liye use hota hai ki promise "settle" ho gaya (chahe success ho ya fail), **data ke sath uska koi lena dena nahi hai**.

Isliye:
```javascript
d = undefined    // finally ko koi parameter milta hi nahi
console.log("Finally block", undefined);   // Output: "Finally block undefined"
```

**Doosra important rule:** `.finally()` ke andar jo bhi tum `return` karo, wo **ignore** ho jata hai (sirf ek exception hai — agar tum yahan se **error throw** karo ya **rejected promise return** karo, tabhi wo aage effect dalta hai). Normal return value pass-through ho jaati hai.

Matlab `return d * 100` (jo ki `NaN` banega kyunki `undefined * 100 = NaN`) — ye **completely ignore** ho jayega. Chain mein wahi purani value (**25**, jo pichle `.then()` se aayi thi) **as-it-is aage chali jaati hai**.

## Step 6: Aakhri `.then()`

```javascript
.then(d => {
    console.log("Final then", d);
    return d * 3;
})
```

- `.finally()` ne value change nahi ki thi, isliye `d` abhi bhi **25** hai (finally se pehle wali value)
```javascript
console.log("Final then", 25);   // Output: "Final then 25"
```

(Iske aage `d * 3 = 75` return hota hai, lekin koi `.then()` aage nahi hai jo ise console karega, isliye wo output mein nahi dikhega.)

## Final Output

```
Error handler 15
Catch block 30
Next then 15
Finally block undefined
Final then 25
```

Yahi hai **Option D**. ✅

## Exam ke liye 3 Golden Rules yaad rakho

1. **`.then(successFn, errorFn)`** — dusra argument reject case handle karta hai, exactly `.catch()` jaisa hi, bas isi `.then()` ke andar
2. **`Error(x).message` hamesha STRING hota hai**, chahe `x` number ho — isliye aage arithmetic mein type coercion dhyan se dekho
3. **`.finally()` na kuch receive karta hai, na value change kar sakta hai** (jab tak throw ya rejected promise return na kare) — ye sirf "side-effect" ke liye hota hai (jaise loading spinner band karna), data flow se **completely disconnected** hota hai

# **Question 3:**

**Consider the below Vue Component .**
```html
<template>
  <div>
    <p>{{ counter }}</p>
    <button @click="incrementCounter">Increment</button>
  </div>
</template>
```
```js
<script>
export default {
  data() {
    return {
      counter: 0
    };
  },
  methods: {
    incrementCounter() {
      this.counter = this.counter + 2;
      this.counter = this.counter * 2;
      this.counter = this.counter - 5;
    }
  }
};
</script>
```

**What will be displayed in the `<p>` element after the button with text "Increment" is clicked twice?**

**Options:**

* **A.** ✗ $0$
* **B.** ✗ $-1$
* **C.** ✗ $-5$
* **D.** ✓ $-3$
* **E.** ✗ $2$

### **Answer: D** ✅



## Concept: Vue Method Kaise Kaam Karta Hai

```js
methods: {
    incrementCounter() {
      this.counter = this.counter + 2;
      this.counter = this.counter * 2;
      this.counter = this.counter - 5;
    }
}
```

**Important baat:** Jab button click hota hai, to `incrementCounter()` method **poora ka poora ek hi baar mein, synchronously (upar se niche)** chalta hai. In teeno lines ke beech mein Vue **kuch reset nahi karta** — har line pichli line ki **updated value** ko use karti hai. Vue sirf itna karta hai ki jab method poora khatam ho jata hai (ya reactively har change par), to `{{ counter }}` ko naye value ke sath **re-render** kar deta hai screen par.

Isliye galti mat karo ye sochne mein ki har click par counter **sirf ek baar "reset" hokar** calculate hota hai — nahi, har click par ye **teeno operations sequentially chalte hain, pichli final value ko carry forward karte hue**.

## Ab Step-by-Step Trace Karo

**Starting value:** `counter = 0`

### 🖱️ Click 1:

| Line | Operation | Calculation | Naya `counter` |
|---|---|---|---|
| Line 1 | `counter + 2` | `0 + 2` | `2` |
| Line 2 | `counter * 2` | `2 * 2` | `4` |
| Line 3 | `counter - 5` | `4 - 5` | `-1` |

**Click 1 ke baad:** `counter = -1`
Screen par abhi dikhega: **-1**

### 🖱️ Click 2:

Yahan **sabse important cheez** yaad rakho: Click 2 shuru hota hai **counter = -1** se (jo pichle click ka result hai), **na ki wapas 0 se**! Kyunki `data()` mein `counter: 0` sirf **initial value** hai — jab tak component destroy na ho, `counter` apni updated state hi maintain karta hai.

| Line | Operation | Calculation | Naya `counter` |
|---|---|---|---|
| Line 1 | `counter + 2` | `-1 + 2` | `1` |
| Line 2 | `counter * 2` | `1 * 2` | `2` |
| Line 3 | `counter - 5` | `2 - 5` | `-3` |

**Click 2 ke baad:** `counter = -3`

## Final Answer

Do baar click karne ke baad `<p>` element mein dikhega: **-3**

Yahi hai **Option D**. ✅

## Common Galti Jo Log Karte Hain (Exam mein isse bacho)

❌ **Galat approach:** Ye sochna ki har click independent hai, aur dobara `0` se calculation start hoti hai:
```
0+2=2, 2*2=4, 4-5=-1   → agar dono click ka sirf yehi answer maan liya
```
Isse tum **-1** (Option B) select kar loge, jo **galat** hai — kyunki ye sirf **1 click ka** result hai.

✅ **Sahi approach:** Har click **state ko carry forward** karta hai — Vue ka `data` object jab tak explicitly reset na ho, apni **current value hi retain karta hai** across multiple method calls.

## Exam ke liye Key Takeaway 🎯

> Jab bhi koi question "button ko **do baar / teen baar click** karne ke baad kya hoga" puche, to hamesha yaad rakho: **har click, pichle click ki final (updated) state se hi shuru hota hai** — data properties method calls ke beech mein apne aap reset nahi hoti. Poori method chain (saari lines) **ek hi click mein sequentially** execute hoti hai, top se bottom.

# **Question 4:**

**Which of the following statement(s) is/are true about Vue.js event handling and communication?**

**Options:**

- **A.** ✓ The `$emit` method is used by child components to send custom events to parent components.
- **B.** ✓ Event modifiers like `.prevent` and `.stop` can be chained together on the same event listener.
- **C.** ✗ The `v-on` directive can only be used with native DOM events and cannot handle custom events.
- **D.** ✓ Parent components can listen to child component events using the `v-on` or `@` syntax in the template.

### **Answer: A, B, D** ✅

**Vue.js Event Handling + Parent–Child Communication** ka conceptual MCQ hai. Isme direct code dry run se zyada important hai ki tum **`$emit`, `v-on`, `@`, event modifiers aur custom events** ka relationship.

# ✅ Correct Statements: **A, B, D**

**C false hai.**



---

# 1. Pehle Core Concept: Vue me Event Communication

Vue me child component ko parent ko koi information/event bhejna ho, toh commonly:

```text
Child
  │
  │ $emit()
  ▼
Parent
  │
  │ @event / v-on:event
  ▼
Handler
```

### Golden Rule 🧠

> **Props = Parent → Child**
> **`$emit` = Child → Parent**

Isko yaad kar lo. Vue ke MCQs me bahut baar isi ko twist karke poocha jata hai.

---

# A. `$emit` method child → parent custom event bhejta hai

### Statement:

> The `$emit` method is used by child components to send custom events to parent components.

### ✅ TRUE

Example:

### Child:

```vue
<button @click="$emit('login-success')">
  Login
</button>
```

Child ne event emit kiya:

```text
login-success
```

### Parent:

```vue
<Login @login-success="handleLogin" />
```

Ab flow:

```text
Child
  │
  │ $emit('login-success')
  ▼
Parent
  │
  ▼
handleLogin()
```

### Exam shortcut:

```text
$emit → Child → Parent
```

✅ **A = TRUE**

---

# B. `.prevent` aur `.stop` chain ho sakte hain

Statement:

> Event modifiers like `.prevent` and `.stop` can be chained together on the same event listener.

### ✅ TRUE

Example:

```vue
<form @submit.prevent.stop="submitForm">
```

Yahan:

```text
.prevent
   +
.stop
```

dono modifiers ek hi listener par apply hue hain.

### `.prevent` kya karta hai?

Browser ka default behavior prevent karta hai.

Example:

```html
<a href="/home" @click.prevent="doSomething">
```

Normally link click:

```text
click
 ↓
/home par navigation
```

`.prevent` ke saath:

```text
click
 ↓
default navigation prevented
```

---

### `.stop` kya karta hai?

Event propagation ko stop karta hai.

Example:

```html
<div @click="parentClick">
    <button @click.stop="buttonClick">
        Click
    </button>
</div>
```

Without `.stop`:

```text
button click
   ↓
button handler
   ↓
parent handler
```

With `.stop`:

```text
button click
   ↓
button handler
   ↓
STOP
```

Parent ka click handler nahi chalega.

### Important

Modifiers chain kar sakte ho:

```vue
@click.prevent.stop="handler"
```

ya:

```vue
@click.stop.prevent="handler"
```

✅ **B = TRUE**

---

# C. `v-on` sirf native DOM events handle karta hai

Statement:

> The `v-on` directive can only be used with native DOM events and cannot handle custom events.

### ❌ FALSE

Ye question ka **main trap** hai.

`v-on` native DOM events bhi handle kar sakta hai aur Vue component ke **custom events** bhi.

---

## Native DOM event

```vue
<button @click="handleClick">
```

`click` ek native browser event hai.

---

## Custom component event

Child:

```vue
<!-- Child -->
<button @click="$emit('login')">
  Login
</button>
```

Parent:

```vue
<Login @login="handleLogin" />
```

Yahan:

```text
@login
```

custom component event ko listen kar raha hai.

So:

```text
v-on / @
      │
      ├── Native events
      │     └── click
      │
      └── Component custom events
            └── login
```

Therefore statement C clearly false hai.

❌ **C = FALSE**

---

# D. Parent `v-on` / `@` se child events listen kar sakta hai

Statement:

> Parent components can listen to child component events using the `v-on` or `@` syntax in the template.

### ✅ TRUE

`@` actually `v-on:` ka shorthand hai.

These two are equivalent:

```vue
<Child v-on:login="handleLogin" />
```

and:

```vue
<Child @login="handleLogin" />
```

Flow:

```text
Child
  │
  │ $emit('login')
  ▼
Parent
  │
  │ @login
  ▼
handleLogin()
```

Therefore:

✅ **D = TRUE**

---

# 🔥 Sabse Important Relationship

Is diagram ko yaad kar lo:

```text
             PARENT
               │
               │ props
               ▼
             CHILD
               │
               │ $emit()
               ▼
             PARENT
```

Matlab:

### Parent → Child

```vue
<Child :name="username" />
```

Uses:

> **Props**

### Child → Parent

```js
this.$emit('login')
```

Parent:

```vue
<Child @login="handleLogin" />
```

Uses:

> **Custom Event + `$emit`**

---

# `v-on` vs `@` — Exam Trap ⚠️

Students kabhi-kabhi sochte hain:

```vue
@click
```

aur:

```vue
v-on:click
```

different hain.

❌ Nahi.

They are equivalent:

```vue
@click="handleClick"
```

=

```vue
v-on:click="handleClick"
```

So question me agar likha ho:

> `v-on` or `@`

toh dono ko same samjho.

---

# Options ko Fast Solve Kaise Karein?

Exam me option dekhte hi keywords pakdo:

### A

```text
$emit
child → parent
```

✅ TRUE

### B

```text
.prevent.stop
```

Multiple event modifiers?

✅ TRUE

### C

```text
v-on ONLY native events
```

"only" bahut important word hai. Custom events bhi handle kar sakta hai.

❌ FALSE

### D

```text
parent + @event
```

Child event listening?

✅ TRUE

Therefore:

# 🎯 **Answer: A, B and D**

---

# ⚠️ Question me "ONLY" ko hamesha pakdo

MCQ setters **absolute words** use karke trap karte hain:

```text
only
always
never
cannot
all
none
```

Option C me:

> **"can only be used with native DOM events"**

Yahi usko suspicious banata hai.

Agar statement hota:

> "`v-on` can be used to listen to events."

toh true hota.

Lekin:

> "`v-on` can **only** handle native DOM events."

❌ False.

---

# 🧠 10-Second Revision

| Concept         | Meaning                      |
| --------------- | ---------------------------- |
| `props`         | Parent → Child               |
| `$emit()`       | Child → Parent               |
| `v-on:`         | Event listener               |
| `@`             | `v-on` shorthand             |
| `.prevent`      | Default behavior stop        |
| `.stop`         | Event propagation stop       |
| `.prevent.stop` | Both modifiers together      |
| Custom event    | Child can emit using `$emit` |
| Parent listens  | `@event` / `v-on:event`      |

### Final Memory Formula:

```text
PARENT
  │
  │ props
  ▼
CHILD
  │
  │ $emit("event")
  ▼
PARENT
  │
  │ @event
  ▼
handler()
```

**A ✅ + B ✅ + C ❌ + D ✅ → Answer = A, B, D**.


# **Question 5:**

**Consider the below Vue class binding and select the most appropriate option(s).**

```html
<div v-bind:class="{ active: isActive, 'text-bold': isBold, hidden: !isVisible }"></div>
```

**Options:**

* **A.** ✓ The class `"active"` will be applied to the `div` element only when the variable `"isActive"` evaluates to true.
* **B.** ✗ The class `"text-bold"` will always be applied to the `div` element regardless of the value of `"isBold"`.
* **C.** ✓ The class `"hidden"` will be applied to the `div` element when the variable `"isVisible"` evaluates to false.
* **D.** ✗ All three classes will be applied simultaneously if `"isActive"`, `"isBold"`, and `"isVisible"` are all true.


### **Answer: A, C** ✅

Ye question Vue ke **Object Syntax for Class Binding** pe based hai — chalo concept aur har option ko detail se samjhte hain.

## Concept: `v-bind:class` Object Syntax

```html
<div v-bind:class="{ active: isActive, 'text-bold': isBold, hidden: !isVisible }"></div>
```

Jab `v-bind:class` (ya shorthand `:class`) ko ek **object** diya jata hai, to Vue ek simple rule follow karta hai:

> **Object ki KEY = CSS class ka naam**
> **Object ki VALUE = condition (true/false)** — agar value **truthy** hai, to us key wala class **element pe apply hoga**; agar **falsy** hai, to nahi hoga.

Yani ye ek **conditional class object** hai jaha har class **independently** apne condition ke basis par decide hoti hai ki lagegi ya nahi — bilkul JavaScript ke normal boolean logic jaisa.

Is object ko todke dekho:

```js
{
  active: isActive,        // class "active" tab lagega jab isActive === true
  'text-bold': isBold,     // class "text-bold" tab lagega jab isBold === true
  hidden: !isVisible       // class "hidden" tab lagega jab isVisible === false (kyunki ! ne isko ulta kar diya)
}
```

**Note:** Key jaise `'text-bold'` quotes mein isliye hai kyunki usme **hyphen (-)** hai — JavaScript object keys mein hyphen bina quotes ke invalid hota hai, isliye string ki tarah likhna padta hai.

## Ab Options ko Detail Se Check Karo

### ✅ Option A — TRUE
> "active" class tab lagega jab `isActive` true ho.

Ye bilkul sahi hai — object syntax ka basic rule yahi hai: **key: value** jaha value truthy ho to class add hoga. Direct mapping hai `active: isActive` — jitna simple utna hi correct.

### ❌ Option B — FALSE
> "text-bold" class **hamesha** lagega, `isBold` ki value ki parwah kiye bina.

Ye **galat** hai — ye option is poore concept ko hi ulta bata raha hai. `text-bold` class **sirf tab** lagega jab `isBold` **true** ho. Agar `isBold = false` hai, to ye class bilkul nahi lagega. "Hamesha lagega" wala statement object-syntax ke conditional nature ko contradict karta hai.

### ✅ Option C — TRUE
> "hidden" class tab lagega jab `isVisible` false ho.

Ye sahi hai, lekin isme thoda **logic ka twist** hai — dhyan se dekho:
```js
hidden: !isVisible
```
Yahan value `isVisible` nahi, balki **`!isVisible`** (negation) hai. Matlab:
- Agar `isVisible = false` → `!isVisible = true` → class **"hidden" lagega**
- Agar `isVisible = true` → `!isVisible = false` → class **"hidden" nahi lagega**

Isliye statement "hidden class isVisible false hone par lagega" — ye **sahi** hai, kyunki `!` operator condition ko invert kar deta hai.

### ❌ Option D — FALSE
> Agar `isActive`, `isBold`, aur `isVisible` teeno **true** hon, to **teeno classes** ek saath apply ho jayengi.

Yahan **trap** hai — is statement mein ek **conceptual galti** hai. Chalo teeno conditions true maan ke check karte hain:

| Variable | Value | Object mein kya likha hai | Result |
|---|---|---|---|
| `isActive` | `true` | `active: isActive` → `active: true` | ✅ class lagega |
| `isBold` | `true` | `'text-bold': isBold` → `'text-bold': true` | ✅ class lagega |
| `isVisible` | `true` | `hidden: !isVisible` → `hidden: !true` → `hidden: false` | ❌ class **nahi** lagega |

Matlab agar `isVisible = true` hai, to `hidden` class ki value `false` ban jaati hai (kyunki `!` negate kar deta hai) — isliye **"hidden" class apply hi nahi hoga**. Sirf `active` aur `text-bold` lagenge, **teeno nahi**.

Isliye Option D galat hai — "sabhi teeno classes simultaneously apply hongi" wala statement **`!isVisible` ke negation logic ko ignore** kar raha hai.

## Final Answer: **A aur C** ✅

## Exam ke liye Key Takeaway 🎯

Jab bhi `:class="{ }"` object syntax dikhe, to **har key ko individually check karo** — especially agar koi value ke aage **`!` (negation)** laga ho, to uska matlab hai ki **actual variable ki opposite condition par class apply hoga**. Ye ek common exam trap hota hai jaha log seedha variable name dekh ke assume kar lete hain, lekin `!` ko miss kar dete hain.

**Formula yaad rakho:**
```
:class="{ className: condition }"
→ condition truthy  = class lagega
→ condition falsy   = class nahi lagega
```

# **Question 6:**
**Consider the below Vue router setup.**

```js
const User = {
  template: `<div><h1>User Profile</h1><router-view></router-view></div>`
};

const UserPosts = {
  template: `<div><h2>Posts by {{ $route.params.username }}</h2></div>`
};

const UserSettings = {
  template: `<div><h2>Settings</h2></div>`
};

const routes = [
  {
    path: '/user/:username',
    component: User,
    children: [
      {
        path: 'posts',
        component: UserPosts
      },
      {
        path: 'settings',
        component: UserSettings
      }
    ]
  }
];

const router = new VueRouter({
  routes
});
```

**What will be the behavior when the user visits "/user/john/posts"?**

**Options :**

- **(A) ❌ Only the UserPosts component will be displayed with "Posts by john", and the User component will be ignored.**
- **(B) ❌ The User component will display "User Profile", but the UserPosts component will not render because <router-view> is missing in the User template.**
- **(C)✅ The User component will display "User Profile", and the UserPosts component will display "Posts by john" within the <router-view> of the User component.**
- **(D) ❌ A 404 error will be shown because child routes cannot access parent route parameters.**
- **(E) ❌ The User component will be displayed, but the UserPosts component will show "Posts by undefined" because child components cannot access $route.params.username.**


#### **Answer: C** ✅
Ye question Vue Router ke **Nested Routes (Child Routes)** concept pe based hai — chalo isko structure se samjhte hain.

## Concept: Nested Routes aur `<router-view>`

Vue Router mein jab tum kisi route ko `children` array deta ho, to ek **parent-child route relationship** ban jata hai. Iska matlab hai:

- **Parent route** ka component **screen ka base structure** render karta hai
- Parent component ke andar ek **`<router-view>`** hona **zaroori** hai — ye ek "placeholder slot" hai jaha **child route ka component** render hoga
- Jab URL child route se match karta hai, to Vue **parent component ko render karta hai, aur uske andar wale `<router-view>` mein child component ko "nest" (ghusa) kar deta hai**

Isko is tarah samjho: 
> **Parent = outer frame**, **Child = us frame ke andar ka content**. Dono **ek saath** screen par dikhte hain, ek doosre ko replace nahi karte.

## Ab Code Ko Line-by-Line Dekho

```js
const routes = [
  {
    path: '/user/:username',
    component: User,
    children: [
      { path: 'posts', component: UserPosts },
      { path: 'settings', component: UserSettings }
    ]
  }
];
```

- Parent route: `/user/:username` → component = `User`
- Child route: `posts` (relative path) → chunki ye `children` ke andar hai, iska **full path** ban jata hai: `/user/:username/posts`
- Similarly, `settings` ka full path: `/user/:username/settings`

**Important concept:** Child route ka `path` hamesha parent ke path ke sath **automatically jud jata hai** — tumhe khud se `/user/:username/posts` likhne ki zaroorat nahi, Vue Router khud prepend kar deta hai parent ka path.

## URL Match Karo: `/user/john/posts`

Ye URL match karta hai:
- Parent segment: `/user/:username` → `username = "john"`
- Child segment: `posts`

Dono match ho gaye, isliye **dono components activate honge** — parent bhi, child bhi.

## Ab `User` Component Ka Template Dekho

```js
const User = {
  template: `<div><h1>User Profile</h1><router-view></router-view></div>`
};
```

Yahan **`<router-view>` maujood hai** parent ke andar — ye bohot important hai. Ye keh raha hai: "Jo bhi child route match ho, uska component **yahan** render karo."

Agar ye `<router-view>` **missing hota**, tab child component render **hi nahi hota** (Option B ka scenario) — lekin yahan clearly present hai, isliye ye scenario apply nahi hota.

## Ab `UserPosts` Component Dekho

```js
const UserPosts = {
  template: `<div><h2>Posts by {{ $route.params.username }}</h2></div>`
};
```

Yahan ek **important concept** hai: **`$route.params` child components ko bhi accessible hota hai**, chahe wo param **parent route** mein define hua ho.

Kyun? Kyunki Vue Router mein **`$route` object poori matched route chain ke liye shared/common hota hai** — jab bhi koi nested route match hota hai, uska **saara URL segments ka combined params object** (`:username` jo parent mein tha) **har nested component ko available hota hai**, na ki sirf jisne define kiya usko.

Isliye:
```js
$route.params.username = "john"   // UserPosts component ko bhi ye milega
```

Aur template render karega: **"Posts by john"**

## Final Render Result

Poora screen structure aisa dikhega:

```
User Profile          ← User component se (h1)
  Posts by john        ← UserPosts component se (h2), User ke <router-view> ke andar nested
```

**Dono components ek sath, nested form mein render honge** — koi bhi doosre ko replace ya ignore nahi karta.

Isliye sahi answer hai **Option C**: 
> User component "User Profile" dikhayega, aur UserPosts component "Posts by john" dikhayega, User ke `<router-view>` ke andar.

## Baaki Options Kyun Galat Hain

- **A ❌** — "Only UserPosts dikhega, User ignore hoga" — Galat, kyunki nested routing mein parent **hamesha** render hota hai jab tak match ho raha hai; child sirf uske andar "insert" hota hai, parent ko replace nahi karta.
- **B ❌** — "router-view missing hai" — Galat, code mein clearly `<router-view>` present hai User ke template mein.
- **D ❌** — "404 error, child routes parent params access nahi kar sakte" — Galat, ye poori tarah galat concept hai. Child routes **hamesha** parent ke params access kar sakte hain via shared `$route.params`.
- **E ❌** — "Posts by undefined dikhega" — Galat, kyunki `$route.params.username` properly `"john"` resolve hoga — child components params access kar hi sakte hain, ye statement khud confusing/wrong claim hai.

## Exam Ke Liye Key Takeaways 🎯

1. **Nested routes mein parent aur child dono ek sath render hote hain** — child, parent ke `<router-view>` ke andar "nest" hota hai
2. **`<router-view>` na ho to child route render hi nahi hoga**, chahe URL match kyun na ho raha ho
3. **`$route.params` poori matched chain ke liye shared hota hai** — child components bhi parent route ke dynamic segments (jaise `:username`) ko directly access kar sakte hain
4. Child route ka `path` (jaise `'posts'`) automatically parent path ke sath **concatenate** ho jata hai final URL banane ke liye

# **Question 7:**


**Consider the following Flask application configuration for Flask-Security, with the User and Role models properly defined with flask-sqlalchemy.**

```python
class ProductionConfig(Config):
    SQLALCHEMY_DATABASE_URI = "sqlite:///userdb.sqlite3"
    DEBUG = False
    SECRET_KEY = "production-secret-key"
    SECURITY_PASSWORD_HASH = "argon2"
    SECURITY_PASSWORD_SALT = "production-salt-value"
    WTF_CSRF_ENABLED = True
    SECURITY_TOKEN_AUTHENTICATION_HEADER = "X-Auth-Token"
````

**A user successfully logs in to the application. Based on this configuration, which of the following statements is MOST ACCURATE?**

**Options:**

* **A.** ✓ The user's authentication token will be sent in the response header named `"X-Auth-Token"` and can be used for subsequent API requests.
* **B.** ✗ CSRF protection blocks cross-origin requests to protect against malicious applications accessing sensitive data.
* **C.** ✗ The authentication token will be generated using the `SECRET_KEY = "production-secret-key"`.
* **D.** ✗ The authentication token will be generated using the argon2 algorithm, since `SECURITY_PASSWORD_HASH` is set to `"argon2"`.


### **Answer: A** ✅

Ye question **Flask-Security** ke authentication configuration pe based hai — chalo har config option aur uske actual role ko samjhte hain, kyunki isme kaafi **common misconceptions** hain jo exam mein trap ban jate hain.

## Pehle Concept Samjho: Flask-Security Configuration Options

Is code mein kai alag-alag config keys hain, aur har ek ka **bilkul alag purpose** hai. Exam mein trap yahi hota hai ki log inka kaam mix-up kar dete hain. Chalo ek-ek karke dekhte hain:

```python
SQLALCHEMY_DATABASE_URI = "sqlite:///userdb.sqlite3"   # Database kaha store hoga
DEBUG = False                                            # Debug mode off
SECRET_KEY = "production-secret-key"                     # Session/cookie signing ke liye
SECURITY_PASSWORD_HASH = "argon2"                         # PASSWORD hash karne ka algorithm
SECURITY_PASSWORD_SALT = "production-salt-value"          # Password hashing mein salt add karne ke liye
WTF_CSRF_ENABLED = True                                    # Forms ke liye CSRF protection
SECURITY_TOKEN_AUTHENTICATION_HEADER = "X-Auth-Token"      # Auth TOKEN kis header mein bhejna hai
```

## Ab Har Option Ko Detail Se Check Karo

### ✅ Option A — TRUE

> User ka authentication token response header `"X-Auth-Token"` mein bhejа jayega, aur subsequent API requests ke liye use ho sakta hai.

Ye sahi hai. `SECURITY_TOKEN_AUTHENTICATION_HEADER` ka **specifically yehi kaam hota hai** — ye Flask-Security ko batata hai ki jab user authenticate ho (login kare), to uska **auth token kis HTTP header name mein bheja jaye**. Default value normally `"Authentication-Token"` hoti hai, lekin yahan explicitly `"X-Auth-Token"` set kiya gaya hai.

Isliye jab user login karta hai, to response mein ek header aayega:
```
X-Auth-Token: <generated-token-value>
```
Aur user is token ko **future API requests mein bhi isi header mein bhej ke** authenticate reh sakta hai (bina baar-baar password diye) — ye stateless token-based authentication ka standard pattern hai.

### ❌ Option B — FALSE

> CSRF protection **cross-origin requests ko block** karti hai malicious apps se data access rokne ke liye.

Ye statement **conceptually galat** hai kyunki ye CSRF ka kaam **CORS (Cross-Origin Resource Sharing)** ke sath confuse kar raha hai — ye do bilkul alag security concepts hain:

| | CSRF Protection | CORS |
|---|---|---|
| **Kya karta hai** | Verify karta hai ki request **legitimate form/user ne bheji hai**, na ki kisi malicious site ne user ke browser ko trick karke | Control karta hai ki **kaunse origins (domains)** se browser ko API access karne ki permission hai |
| **Kaise kaam karta hai** | Ek **secret token** generate karta hai jo form ke sath bheja jata hai; server verify karta hai ki token match karta hai | HTTP headers (`Access-Control-Allow-Origin`) ke through decide hota hai |
| **Kis cheez se bachata hai** | **CSRF attacks** — jaha attacker user ko trick karke unki authenticated session use karke unwanted action karwata hai | **Unauthorized cross-domain API calls** |

`WTF_CSRF_ENABLED = True` sirf itna karta hai ki **forms submission** ke time ek CSRF token verify ho — ye **cross-origin requests ko directly block nahi karta**, wo CORS ka kaam hai. Isliye ye statement galat hai.

### ❌ Option C — FALSE

> Authentication token **`SECRET_KEY`** use karke generate hoga.

Ye bhi ek **common trap** hai. `SECRET_KEY` Flask mein generally **session cookies ko sign karne**, **CSRF tokens generate karne** jaise cheezon ke liye use hota hai — lekin Flask-Security ka **authentication token specifically generate hota hai `SECURITY_PASSWORD_SALT` ke through**, na ki seedha `SECRET_KEY` se.

Flask-Security internally token generate karte waqt `SECURITY_PASSWORD_SALT` ko as a **signing salt** use karta hai (ek `itsdangerous` based serializer ke through). Isliye is statement mein jo **specific cause-effect relation** bataya gaya hai (`SECRET_KEY` se token banega), wo **technically inaccurate** hai — asal mechanism `SECURITY_PASSWORD_SALT` involve karta hai.

### ❌ Option D — FALSE

> Authentication token **argon2 algorithm** use karke generate hoga, kyunki `SECURITY_PASSWORD_HASH = "argon2"` hai.

Ye statement mein **do alag concepts ko mix** kar diya gaya hai:

1. **`SECURITY_PASSWORD_HASH`** — Ye control karta hai ki **user ka PASSWORD** database mein store karne se pehle **kaise hash** hoga (jaise `argon2`, `bcrypt`, etc.) — ye sirf **password storage security** ke liye hai.
2. **Authentication TOKEN** — Ye ek **alag cheez** hai, jo login ke baad generate hota hai taaki user baar-baar password na de. Iska generation mechanism **password hashing algorithm se bilkul unrelated** hai — ye `SECURITY_PASSWORD_SALT` aur serialization (`itsdangerous`) ke through banta hai, `argon2` se nahi.

Isliye "token argon2 se banega" — ye statement **password hashing** aur **token generation** ke concepts ko galat tarike se jodta hai. `argon2` sirf **password ko database mein store karne se pehle** hash karega, token banane mein iska koi role nahi.

## Final Answer: **Option A** ✅

## Exam Ke Liye Key Takeaway 🎯

Is tarah ke Flask-Security questions mein, teen cheezon ko **kabhi mix mat karo**:

| Config | Actual Purpose |
|---|---|
| `SECURITY_PASSWORD_HASH` | **Password** ko DB mein store karne se pehle hash karne ka algorithm |
| `SECURITY_PASSWORD_SALT` | Token generation aur password hashing dono mein "salt" ke roop mein involve (token ke liye signing key ki tarah) |
| `SECURITY_TOKEN_AUTHENTICATION_HEADER` | Login ke baad generate hue **auth token** ko response mein **kis header naam se bhejna hai** |
| `WTF_CSRF_ENABLED` | Sirf **form submissions** ke against CSRF attack se bachata hai, cross-origin requests block nahi karta (wo CORS ka kaam hai) |
| `SECRET_KEY` | Flask ki general session/cookie signing key — token generation ka direct source nahi |

> **Golden Rule:** Jab bhi koi option "X feature, Y config se generate/control hota hai" bole, to socho — kya ye config **specifically** us feature ke liye hi bana hai, ya tum do alag unrelated concepts ko जोड़ रहे ho? Flask-Security mein har config ka apna **narrow, specific purpose** hota hai.

# **Question 8:**

**When using Vue 2 via CDN in a plain HTML file, which of the following is the correct practice?**

**Options:**

- **A.** ✗ Include the Vue CDN `<script>` tag after the script that creates the Vue instance (`new Vue({...})`).
- **B.** ✓ Include the Vue CDN `<script>` tag before the script that creates the Vue instance (`new Vue({...})`).
- **C.** ✗ You must always use `.vue` single-file components when using Vue via CDN.
- **D.** ✗ The CDN build of Vue automatically compiles `.vue` files in the browser.

### **Answer: B** ✅
Ye question kaafi basic hai lekin exam mein isliye pucha jata hai kyunki ye ek **fundamental JavaScript loading concept** test karta hai jo Vue CDN setup ke liye zaroori hai. Chalo samjhte hain.

## Core Concept: Script Loading Order (Browser Kaise Kaam Karta Hai)

Jab browser ek HTML file parse karta hai, to wo `<script>` tags ko **top se bottom, sequentially (ek ke baad ek)** execute karta hai — jaise koi normal code line-by-line chalti hai.

**Golden Rule:**
> **Jis cheez ko tum use karna chahte ho, wo pehle "define/load" honi chahiye, tabhi use kar sakte ho.**

Vue.js jab CDN se load hota hai (jaise `<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>`), to ye script browser ke global scope mein ek `Vue` naam ka **object/constructor** create kar deta hai. Jab tak ye script **poori tarah load aur execute nahi ho jata**, tab tak `Vue` naam ki koi cheez exist hi nahi karti.

## Ab Dekho Kya Hota Hai Agar Order Galat Ho (Option A)

```html
<script>
  new Vue({ el: "#app", data: {...} })   // Vue abhi tak defined hi nahi hai!
</script>
<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>   // Vue yahan aake define hoga
```

Yahan jab browser **pehli line** execute karega, to `Vue` naam ki koi cheez uske paas hai hi nahi (kyunki CDN script abhi load nahi hui). Isliye ye error dega:

```
Uncaught ReferenceError: Vue is not defined
```

Ye bilkul waisa hi hai jaise tum kisi function ko call karo **usse define karne se pehle** — JavaScript ko pata hi nahi ki `Vue` kya hai.

## Sahi Tarika (Option B)

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2"></script>   <!-- Pehle Vue load hoga -->
<script src="app.js"></script>   <!-- Ab is file ke andar new Vue({...}) safely chalega -->
```

Ab jab dusri script chalti hai, `Vue` global object **already available hai** (pehli script ne define kar diya), isliye `new Vue({...})` bina kisi error ke chal jayega.

**Yahi wajah hai ki hamesha CDN library ki script tag ko, us library ko use karne wali apni custom script se PEHLE likhna chahiye.**

## Baaki Options Kyun Galat Hain

### ❌ Option C — ".vue files hamesha use karne padenge CDN ke sath"

Ye galat hai. `.vue` **Single File Components (SFCs)** ek special format hai jisme `<template>`, `<script>`, aur `<style>` sab ek hi file mein hote hain — lekin inko use karne ke liye **build tools** (jaise Vue CLI, Vite, Webpack) chahiye hote hain jo unhe compile karke normal JS/HTML mein convert karte hain.

Jab tum **CDN se directly Vue use karte ho** (plain HTML file mein, bina build step ke), to tum `.vue` files **use hi nahi kar sakte** — browser inhe samajhta hi nahi. Isiliye CDN approach mein hum **plain JavaScript objects** ke through components define karte hain, jaise humne pichle questions mein dekha:
```js
Vue.component('my-comp', {
    template: `<div>...</div>`,   // template yahan string ke roop mein
    ...
});
```
Isliye Option C ka statement **ulta** hai — CDN use karte waqt `.vue` files use **nahi** ki jaa sakti, use hi **nahi karni chahiye**.

### ❌ Option D — "CDN build browser mein .vue files ko automatically compile kar deta hai"

Ye bhi galat hai. **CDN build** (jo browser directly load karta hai) **sirf runtime Vue library** hai — iska kaam hai reactive data, components, directives, etc. ko **HTML/JS ke through** manage karna. Ye `.vue` files jaisa **special SFC format compile karne ka kaam bilkul nahi karta**.

`.vue` files ko compile karne ke liye **`vue-loader`** (Webpack ke sath) ya **Vite ka Vue plugin** jaisa **build-time tool** chahiye hota hai — ye process **development machine par, browser tak pahunchne se pehle** hota hai, na ki browser ke andar runtime par.

## Exam Ke Liye Key Takeaway 🎯

1. **Script order matters** — jo library use karni hai, uski `<script>` tag hamesha **pehle** aani chahiye, tumhare custom code se
2. **CDN-based Vue = No `.vue` files** — CDN approach **build-tool-free** hoti hai, isliye components plain JS objects (template as string) ke through banaye jate hain
3. **`.vue` SFCs sirf build-tool setups mein chalte hain** (Vue CLI / Vite) — browser khud `.vue` format ko samajh hi nahi sakta, use compile karke normal JS/CSS/HTML banana padta hai

**Simple memory trick:**
> "**Library load karo, phir use karo**" — jaise pehle bulb kharido, tab jaake usse jalaoge; ulta order karoge to kaam hi nahi karega.

# **Question 9:**

**Considering that in this code Vue.js 2 CDN is used.**

**`index.html`**

```html
<div id="app">
    <input v-model="username">
    <p>Hello, {{ username }}</p>
</div>

<script>
var app = new Vue({
    el: "#app",
    data: {
        username: "Guest"
    }
});
</script>
````

**Which of the following statements is/are true?**

**Options:**

* **A.** ✓ Typing in the input updates the paragraph instantly.
* **B.** ✗ This demonstrates one-way data binding.
* **C.** ✓ If the `username` value is changed from the console, the input field also updates.
* **D.** ✗ If the directive `v-model` is replaced with `v-bind`, the code will work the same way as it was working before.


### **Answer: A, C** ✅

Ye question Vue ke **`v-model` aur Two-Way Data Binding** concept pe based hai — chalo har option ko reasoning ke sath samjhte hain.

## Core Concept: `v-model` Kya Karta Hai

```html
<input v-model="username">
```

`v-model` Vue mein **two-way data binding** banata hai. Iska matlab hai data **dono directions** mein sync hota hai:

```
Input field  ⟷  data property (username)
   (typing)         (JS se change)
```

**"Two-way"** ka matlab:
1. **Data → View:** Agar `username` JS code se change ho, to input field **automatically update** ho jayega
2. **View → Data:** Agar user input field mein type kare, to `username` data property **automatically update** ho jayegi

Internally, `v-model` ek **syntactic sugar (shortcut)** hai jo do cheezon ko combine karta hai:
```html
<!-- v-model asal mein yehi karta hai internally -->
<input :value="username" @input="username = $event.target.value">
```
- `:value="username"` → data se input mein value bhejta hai (Data → View)
- `@input="username = $event.target.value"` → jab user type kare, to event listen karke data ko update karta hai (View → Data)

Yahi combination "**two-way binding**" banata hai.

## Ab Options Ko Detail Se Check Karo

### ✅ Option A — TRUE
> Input mein type karne se paragraph **instantly** update hota hai.

Ye sahi hai. Jaise hi user input box mein kuch type karta hai:
1. `v-model` ka internal `@input` listener trigger hota hai
2. `username` data property update hoti hai
3. Vue ki **reactivity system** turant detect kar leti hai ki `username` change hua hai
4. Jahan-jahan `{{ username }}` use hua hai (yahan `<p>` tag mein), wo **automatically re-render** ho jata hai — **bina page reload kiye, real-time mein**

Isliye typing karte hi paragraph turant update dikhta hai.

### ❌ Option B — FALSE
> Ye **one-way data binding** demonstrate karta hai.

Ye galat hai — humne upar dekha ki `v-model` **two-way binding** create karta hai, **one-way nahi**. 

**Difference samjho:**
- **One-way binding** hota `:value="username"` ya `{{ username }}` jaisa — sirf data se view mein jata hai, view se wapas data change nahi hoti automatically
- **Two-way binding** (`v-model`) — dono directions mein sync hota hai

Chunki input field aur `username` **dono taraf se sync** ho rahe hain (typing se data update, aur data change se input update — jo Option C prove karega), isliye ye definitely **two-way binding** hai, one-way nahi.

### ✅ Option C — TRUE
> Agar `username` ki value **console se** change ki jaye, to input field bhi update ho jayega.

Ye bhi sahi hai, aur yehi **two-way binding ka doosra half** hai jo isko prove karta hai.

Agar tum browser console mein likho:
```js
app.username = "Rahul"
```
To Vue ki reactivity system turant is change ko detect karegi, aur:
- `<p>` tag automatically "Hello, Rahul" dikhayega
- **Input field** bhi automatically apni value ko `"Rahul"` mein update kar dega (kyunki `v-model` ne `:value="username"` bhi bind kiya hua hai internally)

Ye exactly wahi **Data → View** direction hai jo maine upar explain kiya. Isse pata chalta hai ki binding sirf ek direction mein nahi, **dono directions mein** kaam kar rahi hai — jo two-way binding ki definition hai.

### ❌ Option D — FALSE
> Agar `v-model` ko `v-bind` se replace kar diya jaye, to code **pehle jaisa hi** kaam karega.

Ye **galat** hai, aur ye is question ka sabse important concept-testing option hai. Chalo dekhte hain kyun:

```html
<!-- Agar v-model ko v-bind se replace kar do -->
<input v-bind="username">
```

**`v-bind` sirf ek-directional (one-way) binding provide karta hai** — ye keval data ko element ke **attribute/property** se bind karta hai (jaise `value`, `class`, `src`, etc.), lekin **koi event listener attach nahi karta** jo user input ko wapas data mein sync kare.

Agar tum `v-bind="username"` likhoge (bina property specify kiye), to Vue ise **object syntax** ki tarah treat karega, jo yahan galat/invalid use hoga kyunki `username` ek string hai, object nahi — ye **error** dega ya kaam hi nahi karega.

Aur agar sahi syntax bhi likho jaise `v-bind:value="username"`:
```html
<input v-bind:value="username">
```
To ye sirf **data → input** direction kaam karega (initial value dikhega), lekin jab user **type karega**, to `username` data property **update nahi hogi** — kyunki koi `@input` listener nahi hai jo typing ko sun ke data ko update kare. Isliye paragraph bhi update nahi hoga jab tak tum manually `@input` handler na likho.

Isliye "code pehle jaisa hi kaam karega" — ye statement **poori tarah galat** hai, kyunki `v-model` → `v-bind` replace karne se **two-way binding, one-way (ya broken) binding mein badal jayega**, jo functionality completely different hai.

## Final Answer: **A aur C** ✅

## Exam Ke Liye Key Takeaway 🎯

| Directive | Binding Type | Kaam |
|---|---|---|
| `v-bind` (ya `:`) | **One-way** | Data → View (sirf data se element attribute update hota hai) |
| `v-model` | **Two-way** | Data ⟷ View (dono directions — typing se data update, data change se view update) |

> **Simple yaad rakhne ka formula:** `v-model = v-bind:value + v-on:input` — ye dono ko combine karke hi two-way binding banata hai. Agar tum sirf `v-bind` use karoge, to tumhe manually `v-on:input` (ya `@input`) bhi likhna padega taaki wahi two-way effect mile jo `v-model` automatically deta hai.


# **Question 10:**
**Which of the following is NOT true about Vuex compared to a Vue component?**

**Options:**

- **A.** ✗ Vuex stores data globally and can be accessed across components.
- **B.** ✗ Vuex requires actions, mutations, and state separation.
- **C.** ✗ Vuex avoids deeply nested prop-passing.
- **D.** ✓ Vuex makes every component completely independent, with no shared data.

### **Answer: D** ✅

Ye question **Vuex ka purpose aur core philosophy** test karta hai — chalo concept se samjhte hain ki Vuex kyun banaya gaya tha aur ye component-level state se kaise alag hai.

## Core Concept: Vuex Kyun Use Karte Hain

Normal Vue component mein data **sirf usi component tak local** hoti hai (`data()` ke andar). Jab tumhare paas **bade application** hote hain jisme **kai components ko same data chahiye hoti hai** (jaise logged-in user info, cart items, theme settings), to problem aati hai:

**Bina Vuex ke (Problem):**
```
App
 └─ ParentComponent (data yahan hai)
     └─ ChildComponent (props se data lena padega)
         └─ GrandchildComponent (phir se props se pass karna padega)
             └─ GreatGrandchild (yahan tak props chain banani padegi!)
```
Isse ek problem hoti hai jise **"prop drilling"** kehte hain — data ko har intermediate component ke through **manually pass karna** padta hai, chahe wo component us data ko use na bhi kare, sirf aage bhejne ke liye.

**Vuex ke sath (Solution):**
```
        ┌─────────────┐
        │  Vuex Store  │  ← Ek CENTRAL, GLOBAL data source
        └──────┬───────┘
      ┌─────────┼─────────┐
      ▼          ▼          ▼
  Component1  Component2  Component3   ← Koi bhi component DIRECTLY store se data access kar sakta hai
```

Vuex ek **centralized store** banata hai jaha application ka **shared/global state** rehta hai, aur **koi bhi component** (chahe wo kitna bhi deep nested ho) us data ko **directly access** kar sakta hai — bina kisi intermediate component se guzre.

## Ab Options Ko Detail Se Samjho

### ❌ Option A — Ye TRUE hai (Vuex ke baare mein sahi statement)
> Vuex data ko **globally store** karta hai aur ye components ke across accessible hota hai.

Ye bilkul sahi hai — yehi Vuex ka **pura purpose** hai. Store ek single, central jagah hai jaha state rehta hai, aur **koi bhi component** (top-level ho ya deeply nested) us state ko access kar sakta hai `this.$store.state.xyz` ke through, ya `mapState` helper se.

### ❌ Option B — Ye TRUE hai
> Vuex mein **actions, mutations, aur state ka separation** zaroori hota hai.

Ye bhi sahi hai — Vuex ka **architecture pattern** hi isi separation par based hai:

| Part | Kaam |
|---|---|
| **State** | Actual data jo store karna hai (jaise `count: 0`) |
| **Mutations** | State ko **synchronously** change karne ka **sirf ek tarika** (`this.$store.commit('increment')`) |
| **Actions** | Asynchronous operations (jaise API call) handle karte hain, aur phir mutation ko **commit** karte hain |
| **Getters** | State se computed values nikalne ke liye (jaise computed properties) |

Ye separation isliye zaroori hai taaki state changes **predictable aur traceable** rahein — direct state ko kahin se bhi change nahi kar sakte, sirf **mutations ke through** hi change ho sakta hai. Isse debugging aasan hoti hai (Vue DevTools mein har change track ho jata hai).

### ❌ Option C — Ye TRUE hai
> Vuex **deeply nested prop-passing ko avoid karta hai**.

Ye bhi sahi hai — jaise humne upar dekha, "prop drilling" ek real problem hai jab data ko kai levels deep components tak pass karna padta hai. Vuex isi problem ko **solve** karne ke liye bana hai — ab koi bhi component, chahe wo kitna bhi "deep" ho component tree mein, **directly store se data le sakta hai**, bina intermediate components ke through props pass kiye.

### ✅ Option D — Ye statement FALSE/galat hai (isliye ye hi "NOT true" wala answer hai)
> Vuex har component ko **completely independent** bana deta hai, **koi shared data nahi hota**.

Ye statement **poori tarah Vuex ke concept ko ulta** bata raha hai — ye asal mein **exact opposite** hai jo Vuex karta hai.

Vuex ka **poora maksad hi ye hai ki components ke beech DATA SHARE ho** — ek **centralized, shared state** provide karna jise multiple components **access aur modify** kar sakein. Agar components "completely independent" hote aur "koi shared data" nahi hota, to Vuex ki **zaroorat hi nahi padti** — tab to har component apne `data()` mein hi apna local state rakh leta, jaisa normal Vue components karte hain.

Vuex specifically un cases ke liye design kiya gaya hai jaha:
- Multiple components ko **same data** chahiye (jaise current user, shopping cart)
- Ek component mein change hone par **doosre components ko bhi turant pata chalna chahiye** (reactive sync)

Isliye "completely independent, no shared data" — ye statement Vuex ke **core purpose ko contradict** karta hai, isliye ye **galat/NOT true** hai.

## Final Answer: **Option D** ✅ (ye NOT true hai)

## Exam Ke Liye Key Takeaway 🎯

> **Vuex ka ek-line definition yaad rakho:** "Vuex ek **centralized state management pattern** hai jo application ke **shared data ko globally accessible** banata hai, taaki components ke beech **prop-drilling ki zaroorat na pade** aur data changes **predictable** (via mutations/actions) rahein."

Jab bhi koi option Vuex ko **"independent"**, **"isolated"**, ya **"no sharing"** jaisa bataye — turant samjho ki ye **galat** hai, kyunki Vuex ka **bunyaadi (fundamental) purpose hi "sharing/global access" hai**, uska ulta nahi.

# **Question 11:**


**Consider in this code Vue.js 2 CDN is used.**

**`index.html`**

```html
<div id="app">
    <ul>
        <li v-for="user in users">{{ user.name }}</li>
    </ul>

    <button @click="fetchUsers">Load Users</button>
</div>

<script>
new Vue({
    el: "#app",
    data: { users: [] },
    methods: {
        fetchUsers() {
            fetch("https://jsonplaceholder.typicode.com/users")
                .then(res => res.json())
                .then(data => { this.users = data; });
        }
    }
});
</script>
````

**Which of the following statements is/are correct, assuming the given API is working?**

**Options:**

* **A.** ✓ Clicking the button fetches users and updates DOM.
* **B.** ✓ Vue automatically re-renders the list when `users` changes.
* **C.** ✗ `v-for` is used for conditional rendering.
* **D.** ✓ The code demonstrates Vue's reactivity system.


#### **Answer: A, B, D** ✅

Ye question **Vue Reactivity + `v-for` + Async Data Fetching (Promises with arrow functions)** — teeno concepts ko combine karke test kar raha hai. Chalo code ko line-by-line samjhte hain, phir options.

## Code Ko Samjho — Step by Step

### HTML Part

```html
<ul>
    <li v-for="user in users">{{ user.name }}</li>
</ul>
```

- `v-for="user in users"` — Ye Vue ka **list rendering directive** hai. Iska kaam hai: `users` naam ke **array** ke andar jitne bhi items hain, unke liye ek `<li>` **automatically generate (loop)** karna.
- `users` array mein jitne objects honge, utne hi `<li>` tags render honge, aur har ek mein `{{ user.name }}` us particular user ka naam dikhayega.

**Important:** `v-for` sirf **list ko repeat/loop** karne ke liye hota hai — is question mein isko lekar ek trap option hoga, dhyan rakhna.

```html
<button @click="fetchUsers">Load Users</button>
```

- Button click hone par `fetchUsers` method call hoga.

### JavaScript Part

```js
data: { users: [] },
```

- Initially `users` ek **empty array** hai — isliye page load hote hi `<ul>` mein **kuch bhi nahi dikhega** (kyunki loop karne ke liye array mein items hi nahi hain).

```js
methods: {
    fetchUsers() {
        fetch("https://jsonplaceholder.typicode.com/users")
            .then(res => res.json())
            .then(data => { this.users = data; });
    }
}
```

Ye ek **standard Promise chain** hai jo API se data fetch karta hai:

1. **`fetch(url)`** — Ek network request bhejta hai given URL par, aur ek **Promise return karta hai**
2. **`.then(res => res.json())`** — Jab response aata hai, to `res.json()` uss raw response ko **JavaScript object/array mein parse** karta hai (ye bhi ek Promise return karta hai, kyunki parsing bhi async hoti hai)
3. **`.then(data => { this.users = data; })`** — Jab parsing complete ho jaati hai, to parsed data (jo ki users ka **array** hai) ko `this.users` mein assign kar diya jata hai

## Important Concept: Arrow Function aur `this` Binding

Dhyan do ki `.then(data => { this.users = data; })` mein **arrow function** use hui hai, na ki normal function.

> **Arrow functions apna khud ka `this` create nahi karte** — wo apne **surrounding (lexical) scope** se `this` ko "inherit" kar lete hain.

Yahan `.then()` callback **`fetchUsers` method ke andar** likha gaya hai, aur `fetchUsers` method ke andar `this` **Vue instance** ko refer karta hai. Isliye arrow function ke andar bhi `this` **wahi Vue instance** rahega, aur `this.users = data` **sahi tarike se Vue ke `data.users` ko update karega**.

*(Agar yahan normal `function(data) { this.users = data }` likha hota, to `this` **undefined ya window object** ban jata — ye ek common bug hai jo exam mein bhi pucha jata hai, but is code mein sahi tarike se arrow function use hui hai, isliye ye theek se kaam karega.)*

## Ab Poora Flow Samjho

1. Page load hoti hai → `users = []` → list khali dikhti hai
2. User "**Load Users**" button click karta hai → `fetchUsers()` call hota hai
3. `fetch()` API ko request bhejta hai
4. Response aane par, JSON parse hota hai
5. Parsed data `this.users` mein assign hota hai
6. **Yahin par Vue ki Reactivity System kaam karti hai** — jaise hi `users` array update hota hai, Vue automatically detect kar leta hai ki is property par depend karne wale saare parts of DOM ko re-render karna hai
7. `v-for="user in users"` wala loop **automatically dobara chalta hai** naye `users` array ke sath, aur `<li>` items **DOM mein add ho jate hain** — **bina manually kuch likhe, bina page reload kiye**

## Ab Options Ko Check Karo

### ✅ Option A — TRUE
> Button click karne se users fetch hote hain aur DOM update hota hai.

Bilkul sahi — humne upar poora flow trace kiya. Click → fetch → `this.users` update → DOM automatically re-render.

### ✅ Option B — TRUE
> Vue automatically list ko re-render kar deta hai jab `users` change hota hai.

Ye bhi sahi hai, aur ye **Vue Reactivity System** ka core feature hai. Jab reactive data property (`users`) change hoti hai, to Vue **khud** track kar leta hai ki DOM ke kaunse parts is data par depend karte hain, aur unhe **automatically update** kar deta hai — developer ko manually `document.getElementById` jaisa kuch karke DOM update nahi karna padta.

### ❌ Option C — FALSE
> `v-for` **conditional rendering** ke liye use hota hai.

Ye **galat** hai — ye classic exam trap hai jaha do alag directives ka kaam mix-up kiya jata hai:

| Directive | Actual Purpose |
|---|---|
| **`v-for`** | **List rendering** — array/object ko loop karke multiple elements generate karna |
| **`v-if` / `v-show`** | **Conditional rendering** — kisi condition ke true/false hone par element ko dikhana/chhupana |

`v-for` ka kaam hai **"repeat karna"**, na ki **"condition check karna"**. Isliye "v-for conditional rendering ke liye hota hai" — ye statement **completely galat** hai, iska kaam list ko iterate karna hai.

### ✅ Option D — TRUE
> Ye code Vue ki **reactivity system** ko demonstrate karta hai.

Ye sahi hai — poora example hi reactivity ka best demonstration hai:
- `users` ek **reactive data property** hai (kyunki wo `data()` ke andar defined hai)
- Jab is property ki value **change hoti hai** (async fetch complete hone ke baad), Vue **automatically** DOM ko sync kar deta hai
- Developer ne **kahin bhi manually DOM manipulate nahi kiya** (jaise `document.createElement` ya `innerHTML` use nahi kiya) — sab kuch Vue ne khud track karke update kiya

Yehi Vue ke reactivity system ka **poora point** hai: **data change karo, UI khud-ba-khud sync ho jayega.**

## Final Answer: **A, B, D** ✅ (C galat hai)

## Exam Ke Liye Key Takeaways 🎯

1. **`v-for` = List Rendering** (loop karna), **`v-if`/`v-show` = Conditional Rendering** (dikhana/chhupana) — inhe kabhi mix mat karo
2. **Arrow functions apna `this` nahi banate** — wo apne surrounding scope ka `this` use karte hain. Isliye `.then()` callbacks mein arrow function use karna **safe** hota hai jab tumhe Vue instance (`this`) ko andar access karna ho
3. **Vue Reactivity System** ka core idea: jab bhi koi `data()` property change hoti hai (chahe wo sync ho ya async fetch ke baad), Vue automatically un saare DOM parts ko re-render kar deta hai jo us property par depend karte hain — **manual DOM manipulation ki zaroorat nahi padti**

# **Question 12:**

**Which statements are correct about cross-browser testing in Vue.js applications?**

**Options:**

- **A.** ✓ Ensures UI works consistently in Chrome, Firefox, Safari, Edge, etc.
- **B.** ✓ Tools like Cypress, Selenium, and BrowserStack can be used.
- **C.** ✗ It is unnecessary since Vue handles all rendering.
- **D.** ✓ Some CSS/JavaScript features may behave differently across browsers.

#### **Answer: A, B, D** ✅

Ye question **Cross-Browser Testing** ke concept pe hai — thoda "theory/testing" type ka topic hai, na ki pure coding wala, lekin exam mein important hai. Chalo concept se samjhte hain.

## Core Concept: Cross-Browser Testing Kya Hai Aur Kyun Zaroori Hai

**Cross-browser testing** ka matlab hai apni web application ko **alag-alag browsers** (Chrome, Firefox, Safari, Edge, etc.) aur unke **alag-alag versions** mein test karna, taaki confirm ho sake ki application **sab jagah sahi tarike se** dikhe aur kaam kare.

**Yahan ek bahut important galatfehmi clear karni hai:**

> **Vue.js (ya koi bhi JavaScript framework) sirf tumhara "developer experience" aasan banata hai — components, reactivity, virtual DOM jaisi cheezein handle karta hai. Lekin FINAL OUTPUT jo hota hai, wo hamesha plain HTML, CSS, aur JavaScript hi hota hai, jise ANT KE BROWSER hi render karta hai.**

Matlab Vue **browser ke rendering engine ko replace nahi karta** — wo sirf ek **layer** hai jo tumhare code ko likhna aasan banata hai, lekin final rendering ka kaam to **har browser apne tarike se khud karta hai**.

## Kyun Alag Browsers Mein Farak Aata Hai

Har browser (Chrome ka Blink engine, Firefox ka Gecko engine, Safari ka WebKit engine) **apne tarike se** HTML/CSS/JS ko interpret karta hai:
- Kuch **naye CSS features** (jaise `gap` property, `:has()` selector) sab browsers mein ek sath support nahi hote
- Kuch **JavaScript APIs** purane browsers mein missing ho sakte hain
- **Rendering quirks** (jaise flexbox ka slightly different behavior) alag browsers mein dikh sakte hain

## Ab Options Ko Check Karo

### ✅ Option A — TRUE
> Cross-browser testing ensure karta hai ki UI **Chrome, Firefox, Safari, Edge** etc. mein consistently kaam kare.

Ye bilkul cross-browser testing ki **basic definition** hai — yehi to iska poora purpose hai: confirm karna ki application har major browser mein **same tarike se dikhe aur function kare**.

### ✅ Option B — TRUE
> Tools jaise **Cypress, Selenium, BrowserStack** use ho sakte hain.

Ye sahi hai — ye teeno real-world **industry-standard tools** hain jo cross-browser testing ke liye use hote hain:
- **Selenium** — Automated browser testing framework, multiple browsers mein scripts run kar sakta hai
- **Cypress** — Modern end-to-end testing tool, frontend applications (jaise Vue apps) ke liye popular
- **BrowserStack** — Cloud-based platform jo real devices aur browsers ka access deta hai testing ke liye (bina khud har browser install kiye)

Vue applications ke liye ye teeno tools commonly use hote hain taaki different browsers/devices mein app ko verify kiya ja sake.

### ❌ Option C — FALSE
> Cross-browser testing **unnecessary** hai kyunki Vue **sab rendering handle kar leta hai**.

Ye **galat concept** hai, aur yahi is question ka main trap hai. Jaise humne upar discuss kiya — **Vue rendering engine nahi hai**, ye sirf JavaScript framework hai jo tumhara code likhna easy banata hai. **Final rendering hamesha browser khud karta hai**, apne engine (Blink/Gecko/WebKit) ke through.

Isliye Vue use karne se ye **guarantee nahi milta** ki application har browser mein same dikhegi — browser-specific differences (CSS support, JS API support, rendering quirks) **abhi bhi exist karte hain**, chahe tum kitna bhi modern framework use kar lo. Isliye cross-browser testing **abhi bhi utni hi zaroori hai**, Vue use karne ke bawajood.

### ✅ Option D — TRUE
> Kuch **CSS/JavaScript features** alag browsers mein **differently behave** kar sakte hain.

Ye bhi sahi hai, aur yahi wajah hai ki cross-browser testing zaroori hoti hai. Different browsers **different rendering engines** use karte hain, aur naye CSS/JS features ko **support karne ka timeline alag-alag** hota hai har browser ka. Isliye jo feature Chrome mein perfectly kaam kare, wahi Safari ya purane Edge mein **break ho sakta hai** ya **differently render** ho sakta hai.

## Final Answer: **A, B, D** ✅ (C galat hai)

## Exam Ke Liye Key Takeaway 🎯

> **Golden Rule yaad rakho:** Koi bhi JavaScript framework (Vue, React, Angular) — chahe wo kitna bhi powerful ho — **browser ka rendering engine replace nahi karta**. Framework sirf **developer-side abstraction** hai; final HTML/CSS/JS ko interpret karna aur screen par dikhana **hamesha browser ka hi kaam** hota hai. Isliye **framework use karna cross-browser testing ki zaroorat ko kabhi khatam nahi karta** — ye dono **alag-alag layers** par kaam karte hain.

# **Question 13:**

**Consider this Vue 2 (CDN) method inside a component.**

```javascript
methods: {
    async submit() {
        const response = await fetch('/api/users', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ username: 'Asha' })
        });

        const data = response.json();
        localStorage.setItem('userId', data.id);
    }
}
````

**If the ID of the user “Asha” in the database is 7, What will be stored in localStorage under the key `"userId"` after `submit()` runs?**

**Options:**

* **A.** ✗ `"7"` (string)
* **B.** ✗ `7` (number)
* **C.** ✓ `"undefined"` (string)
* **D.** ✗ `"[object Promise]"` (string)

#### **Answer: C** ✅
Ye question **`async/await` ki ek common galti (missing `await`)** aur **`localStorage` ke type-conversion behavior** ko combine karke test kar raha hai. Ye ek **classic trap question** hai jo real-world mein bhi bohot common bug hai. Chalo line-by-line samjhte hain.

## Code Ko Dhyan Se Dekho

```javascript
async submit() {
    const response = await fetch('/api/users', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ username: 'Asha' })
    });

    const data = response.json();      // ⚠️ Yahan 'await' MISSING hai!
    localStorage.setItem('userId', data.id);
}
```

## Concept 1: `fetch()` aur `response.json()` Dono Async Hote Hain

Ye samajhna **sabse important** hai: `fetch()` API mein **do alag async steps** hote hain, aur **dono ko `await` karna zaroori hota hai**:

```javascript
const response = await fetch(url, options);   // Step 1: Server se response headers aate hain
const data = await response.json();            // Step 2: Response body ko JSON mein PARSE karna (ye bhi async hai!)
```

**Common galtfehmi:** Log sochte hain ki sirf `fetch()` ko `await` karna kaafi hai, lekin `.json()` method **khud bhi ek Promise return karta hai** (kyunki response body ko parse karna bhi time leta hai, ye synchronous operation nahi hai).

Is code mein:
```javascript
const response = await fetch(...)    // ✅ Ye sahi se await hua hai
const data = response.json();         // ❌ Yahan await MISSING hai!
```

## Concept 2: Jab `await` Missing Ho, To Kya Milta Hai

Jab tum `response.json()` ko **bina `await` ke** call karte ho, to tumhe uska **resolved value nahi milta** — balki tumhe **khud Promise object** mil jata hai (jo abhi **pending state** mein hai, kyunki JSON parsing complete hone mein thoda time lagta hai).

```javascript
const data = response.json();
// data = Promise { <pending> }    ← Ye ek Promise object hai, actual data nahi!
```

Matlab `data` variable mein **actual user object** (jaise `{ id: 7, username: "Asha" }`) nahi aaya — usme ek **unresolved Promise** aaya, jo abhi apni value ke sath "settle" nahi hua hai.

## Concept 3: Promise Object Par `.id` Access Karna

Ab dekho ye line:
```javascript
localStorage.setItem('userId', data.id);
```

Yahan `data` ek **Promise object** hai, na ki `{ id: 7, ... }` wala normal object. Aur ek **Promise object mein `.id` naam ki koi property exist hi nahi karti** — Promise ke paas apne khud ke internal methods hote hain (`.then()`, `.catch()`, `.finally()`), lekin `.id` jaisi koi custom property nahi hoti.

Isliye jab tum `data.id` access karte ho:
```javascript
data.id  →  undefined
```

Kyunki JavaScript mein agar tum kisi object ki **non-existent property** access karo, to error nahi aata — bas **`undefined` return hota hai**.

## Concept 4: `localStorage.setItem()` Ka Type-Conversion Behavior

Yahan ek aur important concept hai jo exam mein pucha jata hai:

> **`localStorage` sirf STRINGS store kar sakta hai** — chahe tum koi bhi type (number, boolean, undefined, object) pass karo, `setItem()` internally use **automatically string mein convert kar deta hai** (JavaScript ke `String()` conversion rule follow karke).

Toh jab tum likhte ho:
```javascript
localStorage.setItem('userId', undefined)
```

`localStorage` andar hi andar `undefined` ko **string "undefined"** mein convert kar deta hai (kyunki `String(undefined) === "undefined"`). Isliye jo actually store hota hai wo hai:

```javascript
localStorage.getItem('userId')  →  "undefined"    // Ye ek STRING hai, literal text "undefined"
```

**Ye bohot confusing point hai:** Ye JavaScript ka `undefined` value **nahi** hai — ye ek **string hai jisme letters "u-n-d-e-f-i-n-e-d" likhe hain**. Agar baad mein koi is value ko check kare:
```javascript
localStorage.getItem('userId') === undefined     // false! (kyunki ye string hai)
localStorage.getItem('userId') === "undefined"   // true (ye match karega)
```

## Final Answer

Isliye `submit()` chalne ke baad `localStorage` mein `"userId"` key ke against store hoga:
```
"undefined"   (string)
```

Yahi hai **Option C**. ✅

## Baaki Options Kyun Galat Hain

- **A (`"7"` string)** ❌ — Ye tab hota agar `data.id` sahi se resolve hota (matlab `await response.json()` sahi se likha hota). Lekin missing `await` ki wajah se `data` khud Promise hai, uspar `.id` nahi milta.
- **B (`7` number)** ❌ — Same reason, aur waise bhi `localStorage` kabhi number store nahi karta, hamesha string mein convert kar deta hai.
- **D (`"[object Promise]"`)** ❌ — Ye tab hota agar tum **poore `data` Promise object ko** directly `localStorage.setItem('userId', data)` karte (bina `.id` access kiye) — tab `String(Promise object)` "[object Promise]" banta. Lekin yahan tum `data.id` access kar rahe ho, na ki `data` ko seedha — aur Promise object par `.id` **`undefined`** deta hai, poora object string nahi banta.

## Exam Ke Liye Key Takeaways 🎯

1. **`fetch()` mein DO async steps hote hain** — `fetch(url)` aur `response.json()` — **dono ko `await` karna zaroori hai**, warna doosra step ek unresolved Promise return karega
2. **Missing `await` ka result:** Tumhe actual value ki jagah ek **Promise object** milta hai — us Promise object par direct property access (`.id`, `.name`, etc.) karne se **`undefined` milta hai** (kyunki wo property Promise object mein exist hi nahi karti)
3. **`localStorage` hamesha values ko STRING mein convert karta hai** — chahe input `number`, `undefined`, `boolean`, ya `object` ho, `setItem()` unhe `String()` conversion rule follow karke text mein badal deta hai
4. **`undefined` (actual value) vs `"undefined"` (string)** — ye do bilkul alag cheezein hain, aur `localStorage` sirf **string version** hi store kar sakta hai

**Simple debugging trick jo real projects mein bhi kaam aati hai:** Jab bhi `async/await` code mein koi unexpected `undefined` ya weird value mile, sabse pehle check karo — **kya koi Promise-returning function `await` ke bina call to nahi hua?**

# **Question 14:**
**Which one of the following statements about Vuex is correct?**

**Options:**

- **A.** ✗ Getters are used to modify the store's state and must be synchronous.
- **B.** ✓ Mutations are the only way to directly modify Vuex state, and they should be synchronous.
- **C.** ✗ Actions directly modify state and must not return Promises.
- **D.** ✗ You can call mutations asynchronously (e.g., use `setTimeout` inside a mutation) without side effects.

### **Answer: B** ✅

### ✅ Answer: **B**

**Core Vuex flow yaad rakho:**

> **Component → Action → Mutation → State**

### 4 Important Properties

| Vuex Part     | Kaam                                                       | Async?                 |
| ------------- | ---------------------------------------------------------- | ---------------------- |
| **State**     | Actual data store karta hai                                | —                      |
| **Getters**   | State se **derived/computed data** nikalta hai             | ❌ Normally synchronous |
| **Mutations** | State ko **modify/change** karta hai                       | ❌ **Synchronous only** |
| **Actions**   | Business logic/API calls; mutation ko `commit()` karta hai | ✅ Async allowed        |

### 🔥 B kyu correct hai?

```js
mutations: {
  increment(state) {
    state.count++
  }
}
```

State change karne ka proper Vuex mechanism:

```text
commit()
   ↓
Mutation
   ↓
State change
```

Mutation ko synchronous rakhna important hai, taaki Vuex ko clearly pata rahe ki **state kab aur kis mutation se change hui**.

---

### Options ka concept

**A ❌** Getters state ko modify nahi karte.
Getters = `computed properties` jaisa → **data read/derive** karte hain.

**B ✅** Mutations state modify karte hain aur synchronous hone chahiye.

**C ❌** Actions directly state modify nahi karte; normally `commit()` karke mutation chalate hain. Actions **async ho sakte hain** aur Promise return kar sakte hain.

**D ❌** Mutation me `setTimeout()` jaisa async code nahi karna chahiye. Async work **Actions** me hona chahiye.

### 🧠 Exam Shortcut

> **State = Data**
> **Getter = Read/Calculate**
> **Mutation = Change (Sync)**
> **Action = Logic/API (Async)**

# **Question 15:**


**Consider the following Vue SFC.**

```html
<template>
  <div>
    <p v-if="loading">Loading...</p>
    <p v-else>{{ data }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return { data: null, loading: true }
  },
  async mounted() {
    this.data = await Promise.resolve('Hello MAD2');
    this.loading = false;
  }
}
</script>
````

**What finally appears on the browser?**

**Options:**

* **A.** ✗ Nothing renders
* **B.** ✗ `"Loading..."` remains
* **C.** ✓ `Hello MAD2`
* **D.** ✗ Error (await in mounted)


#### **Answer: C** ✅
Ye question **Vue Lifecycle Hooks + `async/await` ka combination** test karta hai — chalo concept se samjhte hain ki `async mounted()` kaise kaam karta hai aur final state kya banti hai.

## Concept 1: Component Ka Initial Render

```javascript
data() {
    return { data: null, loading: true }
}
```

Jab component **pehli baar create** hota hai, to `data()` se initial values milti hain:
- `data = null`
- `loading = true`

Template mein:
```html
<p v-if="loading">Loading...</p>
<p v-else>{{ data }}</p>
```

Chunki initially `loading = true` hai, **`v-if` condition true hai**, isliye browser mein **sabse pehle** ye dikhega:
```
Loading...
```

Ye is component ka **"first paint"** hai — matlab jaise hi component screen par aata hai, ye state turant dikhti hai.

## Concept 2: `mounted()` Lifecycle Hook Kab Chalta Hai

**Important concept yaad rakho:**

> **`mounted()` hook tab call hota hai jab component ALREADY DOM mein insert ho chuka hota hai** — matlab jab tak `mounted()` chalna start hota hai, tab tak initial template (jisme `loading: true` wali state thi) **already screen par render ho chuka hota hai**.

Isliye sequence hamesha ye hoti hai:
```
1. data() se initial values milti hain (loading: true)
2. Component DOM mein render hota hai → "Loading..." screen par dikhta hai
3. TABHI mounted() hook call hota hai (component ke render hone ke turant baad)
```

## Concept 3: `async mounted()` Kaise Kaam Karta Hai

```javascript
async mounted() {
    this.data = await Promise.resolve('Hello MAD2');
    this.loading = false;
}
```

Yahan **important cheez** hai ki `mounted()` ke aage `async` keyword laga hua hai — iska matlab hai **Vue is method ko async treat karega**, aur andar `await` use kar sakte hain.

**Ek important myth clear karo:** Kai log sochte hain ki `mounted()` mein `async` use karna **invalid** hai ya error dega (jaisa Option D keh raha hai) — **ye galat hai**. Vue **lifecycle hooks mein `async` functions poori tarah supported hain**. Vue internally is method ko normal function ki tarah hi call karta hai; bas ye function ek **Promise return karta hai** jo Vue **await nahi karta** (matlab Vue is Promise ke resolve hone ka wait nahi karega aage badhne ke liye), lekin method **andar se bilkul normally execute hoga**, koi error nahi aayega.

Ab step-by-step dekho kya hota hai andar:

```javascript
this.data = await Promise.resolve('Hello MAD2');
```

- `Promise.resolve('Hello MAD2')` — Ye ek **already-resolved Promise** create karta hai, jiski value `'Hello MAD2'` hai
- `await` iss Promise ke resolve hone ka wait karta hai (jo turant ho jata hai, kyunki ye already resolved hai) aur uski **value nikal leta hai**
- Isliye `this.data = 'Hello MAD2'` ho jata hai

```javascript
this.loading = false;
```

- Iske baad `loading` ko `false` set kar diya jata hai

## Concept 4: Vue Ki Reactivity — Automatic Re-render

Jaise hi `this.data` aur `this.loading` **change** hoti hain, Vue ki **reactivity system** turant detect kar leti hai ki template ke andar in properties par depend karne wale parts **re-render** karne hain:

```html
<p v-if="loading">Loading...</p>     ← loading ab false hai, isliye ye HIDE ho jayega
<p v-else>{{ data }}</p>              ← ye ab SHOW hoga, aur {{ data }} = 'Hello MAD2'
```

Isliye `v-if/v-else` switch ho jata hai, aur screen par ab dikhega:
```
Hello MAD2
```

## Poori Timeline Ek Sath Dekho

| Step | Kya hota hai | Screen par kya dikhta hai |
|---|---|---|
| 1 | `data()` se initial state milti hai (`loading: true`) | — |
| 2 | Component DOM mein mount hota hai | **"Loading..."** |
| 3 | `mounted()` hook call hota hai (async) | "Loading..." (abhi bhi) |
| 4 | `await Promise.resolve(...)` resolve hota hai → `this.data` set hoti hai | "Loading..." (abhi bhi, reactivity turant nahi hui async ke andar wale step tak) |
| 5 | `this.loading = false` set hota hai | Vue reactivity trigger hoti hai → re-render |
| **6 (Final)** | Template update ho jata hai | **"Hello MAD2"** ✅ |

**Note:** Step 2 aur step 6 ke beech mein technically "Loading..." bohot **thodi der (milliseconds)** ke liye dikhta hai (kyunki `Promise.resolve()` turant resolve hota hai, lekin phir bhi ek microtask cycle lagta hai) — lekin question puch raha hai **"finally kya appears"** (matlab final/settled state), isliye answer wahi hai jo **last mein stably dikhta hai**.

## Ab Options Ko Check Karo

### ❌ Option A — "Nothing renders"
Galat — component turant render hota hai, pehle "Loading...", phir "Hello MAD2". "Nothing" kabhi nahi hota.

### ❌ Option B — "Loading... remains"
Galat — ye tab hota agar `loading` **kabhi false na ho** (jaise agar `this.loading = false` line missing hoti, ya Promise kabhi resolve hi na hota — jaise real API call fail ho jaye bina catch ke). Lekin yahan `Promise.resolve()` **turant aur guaranteed resolve** hota hai, aur `loading = false` explicitly set kiya gaya hai — isliye "Loading..." **permanently nahi reh sakta**.

### ✅ Option C — "Hello MAD2"
Sahi hai — jaisa humne trace kiya, final state mein `loading = false` aur `data = 'Hello MAD2'` ho jaate hain, jisse `v-else` wala paragraph render hota hai final content ke sath.

### ❌ Option D — "Error (await in mounted)"
Galat — ye ek **common misconception** hai. `async` lifecycle hooks (`mounted`, `created`, etc.) Vue mein **completely valid aur commonly used pattern** hai. Koi error nahi aata `mounted()` ke andar `await` use karne se.

## Final Answer: **Option C — "Hello MAD2"** ✅

## Exam Ke Liye Key Takeaways 🎯

1. **`mounted()` hamesha initial render ke BAAD chalta hai** — isliye jo bhi initial `data()` state ho, wo **pehle screen par dikhti hai**, phir `mounted()` ke andar ke changes reflect hote hain
2. **`async mounted()` bilkul valid hai** — Vue lifecycle hooks mein `async/await` freely use kar sakte ho, koi restriction nahi hai
3. **Vue, async lifecycle hook ke Promise ka wait nahi karta** — matlab agar `mounted()` async hai, to Vue us hook ke poora "resolve" hone ka intezaar **nahi** karta aage badhne ke liye; hook apni jagah **independently** execute hota rehta hai, aur jab bhi state change ho, reactivity apna kaam karti hai
4. **Question mein "finally/final state" puchne ka matlab hai** — tumhe **saari async operations complete hone ke baad ki stable state** batani hai, na ki intermediate/temporary states

**Simple mental model:**
> Pehle **synchronous render** hota hai (jo bhi `data()` mein initial values hain) → phir **`mounted()` async chalna start hota hai** in the background → jab andar ki `await` wali cheez resolve hoti hai, tab data update hota hai → Vue reactivity turant DOM ko **sync** kar deti hai final values ke sath.

# **Question 16:**


**Consider the following JavaScript code.**

```javascript
function fetchData() {
    return Promise.reject('Network Down');
}

fetchData()
    .then(res => console.log(res))
    .catch(err => console.log('Error:', err))
    .then(() => console.log('Done'));
````

**What will be logged on the browser’s console?**

**Options:**

* **A.** ✓

  ```text
  Error: Network Down
  Done
  ```

* **B.** ✗

  ```text
  Done
  Error: Network Down
  ```

* **C.** ✗

  ```text
  Error: Network Down
  ```

* **D.** ✗ `Error: Unhandled Promise rejection`

#### **Answer: A** ✅


Ye question **Promise Chain + `.catch()` ke baad chain continue hone ka concept** test karta hai — chalo step-by-step trace karte hain.

## Step 1: `fetchData()` Function

```javascript
function fetchData() {
    return Promise.reject('Network Down');
}
```

- `Promise.reject('Network Down')` — Ye ek **already-rejected Promise** create karta hai, jiska rejection reason hai string `'Network Down'`
- Matlab jaise hi `fetchData()` call hota hai, ye ek **rejected Promise** turant return kar deta hai — koi network call nahi ho raha yahan, sirf simulate kiya ja raha hai ki request fail ho gayi

## Step 2: Promise Chain Ko Samjho

```javascript
fetchData()
    .then(res => console.log(res))
    .catch(err => console.log('Error:', err))
    .then(() => console.log('Done'));
```

Is chain mein **teen handlers** hain. Chalo ek-ek karke dekhte hain kaun chalega aur kaun skip hoga.

### Concept: Rejection State Aage "Skip" Karti Hai Jab Tak `.catch()` Na Mile

> **Jab koi Promise reject hoti hai, to us rejection ke baad wale saare `.then()` handlers (jinme sirf success/first argument diya ho) SKIP ho jate hain, jab tak ki koi `.catch()` (ya `.then()` ka doosra error-argument) na mil jaye.**

Ye bilkul waisa hai jaise ek train **seedhe rejection ke sath aage badhti rehti hai**, har normal station (`.then()`) ko **bina ruke paar karti jaati hai**, jab tak use koi **"error station" (`.catch()`)** na mile.

### Handler 1: `.then(res => console.log(res))`

```javascript
.then(res => console.log(res))
```

- Chunki `fetchData()` ne **rejected Promise** return kiya (na ki resolved), is `.then()` ka **success handler nahi chalega**
- Ye handler **skip** ho jata hai, aur rejection **seedhe aage** pass ho jaati hai next handler ko
- **Kuch bhi print nahi hota yahan**

### Handler 2: `.catch(err => console.log('Error:', err))`

```javascript
.catch(err => console.log('Error:', err))
```

- `.catch()` **specifically rejection ko handle karne** ke liye banaya gaya hai — ye pichle chain mein jo bhi rejection "propagate" hoke aayi thi, usko **yahan catch kar leta hai**
- `err = 'Network Down'`
- Isliye ye print hoga:
```
Error: Network Down
```

**Important concept:** Jab `.catch()` ka callback **normally complete ho jata hai** (matlab andar koi naya error throw nahi hota, aur na hi koi rejected promise return hota hai), to Vue... *(oops, JS)* — to **Promise chain wapas "resolved" state mein chali jaati hai**. `console.log()` khud ek **`undefined` return karta hai**, jo ek normal (non-error) value hai.

Isliye is `.catch()` ke **turant baad wali chain, resolved state se continue hogi**.

### Handler 3: `.then(() => console.log('Done'))`

```javascript
.then(() => console.log('Done'))
```

Chunki pichla `.catch()` **successfully complete** ho chuka (koi naya error throw nahi hua), is `.then()` ka **success handler chalega** (kyunki chain ab "resolved" state mein hai):

```
Done
```

## Poora Flow Ek Sath Dekho

| Step | Kya hota hai | Console Output |
|---|---|---|
| 1 | `fetchData()` rejected Promise return karta hai (`'Network Down'`) | — |
| 2 | Pehla `.then(res => ...)` **skip** hota hai (kyunki rejection hai, success nahi) | — |
| 3 | `.catch(err => ...)` rejection ko **catch** karta hai | `Error: Network Down` |
| 4 | `.catch()` normally complete hota hai → chain **resolved** ho jaati hai | — |
| 5 | Aakhri `.then(() => ...)` chalta hai (kyunki ab chain resolved hai) | `Done` |

## Final Console Output

```
Error: Network Down
Done
```

Yahi hai **Option A** ✅

## Baaki Options Kyun Galat Hain

- **B ❌** — Order **ulta** hai. `.catch()` hamesha **pehle** chalega (kyunki wo chain mein pehle aata hai), phir `.then()` "Done" print karega. Sequence kabhi reverse nahi ho sakta kyunki Promise chain **strictly top-se-bottom order mein hi execute** hoti hai.
- **C ❌** — Sirf "Error: Network Down" print karke ruk jana galat hai. Ye tab hota agar `.catch()` ke baad koi `.then()` hi na hota, ya `.catch()` ke andar **error throw** hota (jisse chain phir se reject ho jaati aur agla `.then()` bhi skip ho jata). Lekin yahan `.catch()` normally complete hota hai, isliye chain aage **zaroor badhegi**, aur "Done" bhi print hoga.
- **D ❌** — "Unhandled Promise rejection" tab aata jab **koi `.catch()` hi nahi hota** poore chain mein — matlab rejection ko **kahin bhi handle nahi kiya gaya hota**. Yahan explicitly `.catch()` maujood hai jo rejection ko properly handle kar raha hai, isliye koi "unhandled" error nahi aayega.

## Exam Ke Liye Key Takeaways 🎯

1. **Rejection "skip" karti hai saare normal `.then()` handlers ko**, jab tak use koi `.catch()` (ya `.then()` ka 2nd error-argument) na mil jaye — bilkul jaise ek exception JS mein bubble up hoti hai jab tak `catch` block na mile
2. **`.catch()` ke baad chain wapas "resolved" ho jaati hai** — **agar** `.catch()` ka callback normally complete ho (naya error throw na kare, rejected promise return na kare). Isliye `.catch()` ke turant baad wale `.then()` **normal success handlers ki tarah hi chalte hain**
3. **`console.log()` hamesha `undefined` return karta hai** — jo ek valid, non-error value hai, isliye ye kabhi bhi Promise ko reject nahi karta

**Simple mental model yaad rakho:**
> `.catch()` ek **"safety net"** ki tarah kaam karta hai — ek baar error "catch" ho jaye, to chain **default rup se wapas normal (resolved) track par aa jaati hai**, jab tak `.catch()` ke andar dobara koi naya error na phenka jaye.

