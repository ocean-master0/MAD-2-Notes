# 📘 App Dev 2 (MAD 2) — Quiz 1 Jan 2026 PYQ Solution

# Question 2

## Original Question
> Which of the following statements about JavaScript scope is/are correct in the browser?
>
> Options:
> 1. `var` is function-scoped
> 2. `let` and `const` are block-scoped
> 3. Variables declared with `let` are hoisted and initialized with `undefined`
> 4. `const` variables can be declared without initialization
> 5. Global variables are always attached to window (as a property)

(Multiple Select Question, Correct Marks: 2)

---

## Correct Answer
**Correct Options:** 1 and 2
(`var` is function-scoped ✅, `let`/`const` are block-scoped ✅)

---

## Concept Used
**Scope** ka matlab hai — variable kaha tak "visible" ya "accessible" hai code me.

- 📘 **Function Scope (`var`):** `var` se declare kiya gaya variable pure function ke andar accessible hota hai, chahe wo kisi bhi `{}` block ke andar ho.
- 📘 **Block Scope (`let`, `const`):** `let` aur `const` sirf us `{ }` block ke andar accessible hote hain jisme wo declare hue hain.
- 📘 **Hoisting:** JavaScript engine code run karne se pehle saari variable/function declarations ko "upar utha" leta hai (memory allocate kar deta hai), lekin initialization wahi rehti hai jaha likhi hai.
  - `var` hoisting → variable `undefined` se initialize hota hai.
  - `let`/`const` hoisting → variable hoist to hota hai lekin **Temporal Dead Zone (TDZ)** me rehta hai — use karne par `ReferenceError` aata hai, `undefined` nahi milta.
- 📘 Exam me is concept se **true/false type ya multi-select statements** milte hain jisme scope, hoisting aur `var/let/const` ke differences test kiye jaate hain.

**Example:**
```js
function test() {
  if (true) {
    var a = 1;   // function scoped
    let b = 2;   // block scoped
  }
  console.log(a); // 1 (accessible)
  console.log(b); // ReferenceError (not accessible)
}
```

---

## Step-by-Step Solution
1. **Statement 1:** "`var` is function-scoped" → Ye **sahi** hai. `var` kisi bhi block (`if`, `for`) ke andar declare ho, wo pure function level pe accessible rehta hai.
2. **Statement 2:** "`let` and `const` are block-scoped" → Ye bhi **sahi** hai. Ye sirf us `{}` ke andar hi accessible hote hain jisme declare hue.
3. **Statement 3:** "Variables declared with `let` are hoisted and initialized with `undefined`" → **Galat**. `let` hoist to hota hai but `undefined` se initialize nahi hota — wo TDZ (Temporal Dead Zone) me rehta hai. Access karne par error aayega.
4. **Statement 4:** "`const` variables can be declared without initialization" → **Galat**. `const` hamesha declare karte time value deni hi padti hai, warna SyntaxError aayega. E.g. `const x;` invalid hai.
5. **Statement 5:** "Global variables are always attached to window" → **Galat**. Sirf `var` se bane global variables `window` object pe attach hote hain. `let`/`const` se bane global variables `window` pe attach **nahi** hote.

---

## Final Answer
**Correct Statements: 1 and 2** — `var` function-scoped hai aur `let`/`const` block-scoped hain.

---

## Why Other Options are Wrong?
### Statement 3 (let hoisted with undefined)
Wrong hai kyunki `let`/`const` Temporal Dead Zone me hote hain, `undefined` nahi milta — error milta hai agar declaration se pehle access karo.

### Statement 4 (const without initialization)
Wrong hai kyunki `const` ek immutable binding banata hai jiski value declare karte waqt hi deni padti hai.

### Statement 5 (global vars attached to window)
Wrong hai kyunki sirf `var` window object ka property banta hai, `let`/`const` nahi.

---

## Important Exam Notes
- ✅ `var` → function scope, hoisted with `undefined`, attaches to `window`.
- ✅ `let`/`const` → block scope, hoisted but in TDZ, NOT attached to `window`.
- ⚠️ Common Mistake: Log karna ki `let` bhi `undefined` deta hai — galat, ReferenceError aata hai.
- 💡 Memory Trick: "**V**ar = **V**isible everywhere in function, **L**et/**C**onst = **L**ocked in block"

---

## Similar Question Pattern
Aise questions me `var` vs `let` vs `const` ke differences (scope, hoisting, redeclaration, window attachment) puche jaate hain — multi-select format common hai.

---

## Revision Box
`var` = function-scoped + hoisted as `undefined` + attaches to window.
`let`/`const` = block-scoped + hoisted in TDZ (no `undefined`) + not attached to window.
`const` needs mandatory initialization.

---
---

# Question 3

## Original Question
> Which of the following statements about JavaScript functions is/are true?
>
> Options:
> 1. Every function in JavaScript is an object.
> 2. Arrow functions have their own "this" binding
> 3. Function declarations are hoisted.
> 4. Function expressions assigned to variables are hoisted with their function value.

(Multiple Select Question, Correct Marks: 2)

---

## Correct Answer
**Correct Options:** 1 and 3

---

## Concept Used
- 📘 **Functions as Objects:** JavaScript me functions bhi ek type ke **object** hote hain (`typeof function(){} === "function"` but internally ye Object type se derive hota hai). Isi liye functions ke pass properties/methods (`.call`, `.apply`, `.bind`) hote hain.
- 📘 **Arrow Functions & `this`:** Arrow functions ka **apna `this` nahi hota** — ye lexical scoping follow karte hain, matlab jis scope me arrow function likha gaya hai, wahi ka `this` use karte hain (outer/enclosing scope se `this` "inherit" karte hain).
- 📘 **Hoisting of Functions:**
  - **Function Declarations** (`function foo(){}`) puri tarah hoist hoti hain — matlab unko call karne se pehle bhi use kar sakte ho.
  - **Function Expressions** (`const foo = function(){}`) sirf variable declaration hoist hoti hai (jaise `var`/`let` rule), lekin function **value** hoist nahi hoti. Agar `var` use hua ho to variable `undefined` hoga call time pe, agar `let/const` to TDZ error aayega.

**Example:**
```js
sayHi(); // works! (hoisted)
function sayHi() { console.log("hi"); }

sayBye(); // TypeError: sayBye is not a function
var sayBye = function() { console.log("bye"); };
```

---

## Step-by-Step Solution
1. **Statement 1:** "Every function in JavaScript is an object" → **True**. Functions JS me first-class objects hain, unme properties add ki ja sakti hain aur methods call kiye ja sakte hain.
2. **Statement 2:** "Arrow functions have their own 'this' binding" → **False**. Ye ulta hai — arrow functions ka apna `this` hota hi nahi, wo enclosing (lexical) scope se `this` uthate hain.
3. **Statement 3:** "Function declarations are hoisted" → **True**. Poora function definition (body samet) hoist hota hai, isliye call-before-define kaam karta hai.
4. **Statement 4:** "Function expressions assigned to variables are hoisted with their function value" → **False**. Sirf variable name hoist hota hai, function value nahi — assignment run-time pe hoti hai jab code us line tak pahuchta hai.

---

## Final Answer
**Correct Statements: 1 and 3** — Functions objects hote hain, aur function declarations fully hoisted hoti hain.

---

## Why Other Options are Wrong?
### Statement 2
Galat hai kyunki arrow functions specifically JS me isliye banaye gaye the taaki wo apna `this` na banaye, balki outer scope ka `this` use karein — ye ek common interview/exam trap hai.

### Statement 4
Galat hai kyunki hoisting sirf declaration ko upar le jaati hai, definition/assignment nahi. Function expression ka value tabhi milta hai jab us line ka execution ho.

---

## Important Exam Notes
- ✅ Function Declaration = Fully hoisted (name + body).
- ✅ Function Expression = Sirf variable naam hoist, value nahi.
- ⚠️ Common Mistake: Arrow function ko "own this" wala samajhna — ye galat hai, ye "no own this" wala concept hai.
- 💡 Trick: "**Arrow = No Own This**" yaad rakho.

---

## Similar Question Pattern
Function hoisting aur arrow function ka `this` behaviour dono exam me bahut common topics hain — expect karo similar MSQ ya code-output questions.

---

## Revision Box
Function declarations fully hoist hoti hain, function expressions sirf naam. Functions objects hote hain. Arrow functions ka apna `this` nahi hota — wo lexical/enclosing scope ka `this` use karte hain.

---
---

# Question 4

## Original Question
```js
var a = 3;
const obj1 = {
  a: 20,
  show: function () {
    console.log(this.a);
  }
};
const obj2 = { a: 10 };

obj1.show.apply(obj1);
```
> What will be printed on the console?
> Options: (A) 20 (B) 10 (C) undefined (D) 3

---

## Correct Answer
**Correct Option:** A (20)

---

## Concept Used
- 📘 **`this` keyword:** JavaScript me `this` ki value **kaise function call hua** us pe depend karti hai, function kaha define hua us pe nahi.
- 📘 **`.apply()` method:** `apply()` function ko call karta hai aur explicitly specify karta hai ki `this` kya hoga. `functionName.apply(thisArg, [argsArray])`.
- 📘 Jab `obj1.show.apply(obj1)` likha jaata hai, hum explicitly bol rahe hain ki is function call ke andar `this` = `obj1` hoga.

**Example:**
```js
function greet() { console.log(this.name); }
const person = { name: "Rakesh" };
greet.apply(person); // "Rakesh"
```

---

## Step-by-Step Solution
1. **Step 1:** `obj1` object banaya gaya hai jisme `a: 20` property hai aur `show` method hai jo `this.a` print karta hai.
   - *Reason:* Ye samajhna zaroori hai ki `show` method `this` use karta hai, koi fixed variable nahi.
2. **Step 2:** `obj1.show.apply(obj1)` call hota hai — yaha `apply(obj1)` explicitly `this` ko `obj1` set kar raha hai.
   - *Reason:* `.apply()` ka pehla argument hamesha `this` ki value decide karta hai.
3. **Step 3:** Function ke andar `this.a` execute hota hai → `this` = `obj1` → `obj1.a` = `20`.
   - *Reason:* Kyunki humne explicitly `this` ko `obj1` bana diya, isliye `obj1` ki property `a` (jo 20 hai) use hogi, na ki global `var a = 3`.
4. **Step 4:** Console output = `20`.

**Shortcut:** Jab bhi `.apply()`, `.call()`, ya `.bind()` dikhe, seedha dekho pehla argument — wahi `this` banega.

---

## Final Answer
**20** — kyunki `apply(obj1)` ne `this` ko `obj1` set kar diya, aur `obj1.a = 20`.

---

## Why Other Options are Wrong?
### Option B (10)
`10` `obj2.a` ki value hai, lekin `obj2` ka is call se koi lena dena nahi hai — humne `apply(obj1)` likha, `apply(obj2)` nahi.

### Option C (undefined)
Galat hai — agar `this` undefined hota (jaise plain function call me) to shayad ye answer hota, lekin yaha humne explicitly `this` bind kiya hai `apply()` se.

### Option D (3)
Ye global `var a = 3` ki value hai. Ye tab print hota jab `this` global object (window) point karta — lekin `apply(obj1)` ki wajah se `this` = `obj1` hai, global nahi.

---

## Important Exam Notes
- ✅ `apply(thisArg, [args])` — `this` explicitly set karta hai.
- ✅ `call(thisArg, arg1, arg2...)` — same as apply but args array ki jagah comma-separated.
- ⚠️ Common Mistake: Global `var a` ko use kar lena — but jab `this` explicitly set ho, global variable ignore ho jaata hai.
- 💡 Trick: "**A**pply = **A**rray of args, **C**all = **C**omma args"

---

## Similar Question Pattern
`this` binding ke saath `call`, `apply`, `bind` wale code-output questions bahut common hain — object methods, nested `this`, arrow function ke saath `this` ka mix bhi expect karo.

---

## Revision Box
`apply(obj)` explicitly `this` ko `obj` bana deta hai andar wale function call ke liye. Isliye `this.a` = `obj1.a` = 20.

---
---

# Question 5

## Original Question
```js
function addItemToCart(item) {
  let cart = JSON.parse(localStorage.getItem('cart')) || [];
  cart.push(item);
  localStorage.setItem('cart', JSON.stringify(cart));
}

function getCartItems() {
  return localStorage.getItem('cart') || [];
}

addItemToCart({ id: 1, name: 'Laptop' }.name);

console.log(getCartItems());
```
> The above code is initially loaded in the browser, and then the browser is refreshed two times. What will be the final output printed in the console?
>
> Options:
> A. `[]`
> B. `['Laptop', 'Laptop', 'Laptop']`
> C. `[{ id: 1, name: 'Laptop' }]`
> D. `[undefined]`

---

## Correct Answer
**Correct Option:** B (`['Laptop', 'Laptop', 'Laptop']`)

---

## Concept Used
- 📘 **`localStorage`:** Browser ka ek storage mechanism jo data ko **persist** karta hai — matlab browser band karne, refresh karne ya tab close karne ke baad bhi data delete nahi hota (jab tak manually clear na karo).
- 📘 **Contrast with `sessionStorage`:** `sessionStorage` sirf tab open rehne tak data rakhta hai, refresh pe survive karta hai lekin tab close hone pe delete ho jaata hai. `localStorage` refresh **aur** tab close dono ke baad bhi rehta hai.
- 📘 Jab bhi page **reload/refresh** hota hai, poora JS script phir se top-se-bottom run hota hai — matlab jo bhi code globally likha hai (jaise `addItemToCart(...)` ka call), wo phir se execute hoga.

**Example:** Agar ek counter `localStorage` me store ho aur har refresh pe +1 ho, to refresh karte rehne se number badhta jayega — data reset nahi hota.

---

## Step-by-Step Solution
1. **Step 1:** Sabse pehle dhyan do — `addItemToCart({ id: 1, name: 'Laptop' }.name)` call ho raha hai. `{ id: 1, name: 'Laptop' }.name` ka matlab hai object banao aur turant uski `.name` property nikaalo — jo hai `'Laptop'` (sirf string, poora object nahi).
   - *Reason:* Ye ek chhota trap hai — bahut students socheinge poora object cart me jaa raha hai, lekin actually sirf `'Laptop'` string ja rahi hai.
2. **Step 2: Initial Page Load** — `cart` khali hai (`[]`), `'Laptop'` push hota hai → `cart = ['Laptop']` → `localStorage` me save.
   - *Reason:* Pehli baar page load hone pe `localStorage` empty tha, isliye `|| []` fallback use hua.
3. **Step 3: First Refresh** — Page reload hota hai, script phir chalti hai. Ab `localStorage` me pehle se `['Laptop']` hai (kyunki localStorage persist karta hai). `cart = ['Laptop']`, phir `'Laptop'` push → `cart = ['Laptop', 'Laptop']` → save.
   - *Reason:* localStorage ka data refresh ke baad bhi safe rehta hai, isliye purana data mila.
4. **Step 4: Second Refresh** — Same process phir se. `cart = ['Laptop', 'Laptop']` → push → `cart = ['Laptop', 'Laptop', 'Laptop']` → save.
5. **Step 5:** Ab `console.log(getCartItems())` chalta hai. **Yaha ek chhota bug hai code me:** `getCartItems()` `localStorage.getItem('cart')` return karta hai jo actually ek **JSON string** hota hai (kyunki humne `JSON.stringify` kar ke store kiya tha), na ki actual array. Lekin conceptually/expected answer ke hisaab se ye stringified array ka content dikhayega jo visually `['Laptop', 'Laptop', 'Laptop']` jaisa dikhta hai.
   - *Reason:* Total 3 baar `addItemToCart` chala (1 initial load + 2 refresh), har baar `'Laptop'` add hua.

**Shortcut:** Jab bhi "refreshed N times" dikhe with `localStorage`, socho: **Total executions = 1 (initial) + N (refreshes)**.

---

## Final Answer
**`['Laptop', 'Laptop', 'Laptop']`** — kyunki total 3 baar script chali (1 load + 2 refresh) aur `localStorage` persist karta hai.

---

## Why Other Options are Wrong?
### Option A (`[]`)
Ye tab hota agar storage `sessionStorage`/normal variable hota jo refresh pe reset ho jaata — lekin `localStorage` reset nahi hota.

### Option C (`[{ id: 1, name: 'Laptop' }]`)
Ye galat hai kyunki code me `.name` already extract kiya gaya hai object se pehle push karne se pehle — poora object kabhi push hi nahi hua.

### Option D (`[undefined]`)
Ye tab hota agar `.name` property exist na karti object me — lekin yaha `name: 'Laptop'` property properly defined hai object me.

---

## Important Exam Notes
- ✅ `localStorage` = persists across refresh AND browser close.
- ✅ `sessionStorage` = persists across refresh, but clears on tab close.
- ⚠️ Common Mistake: Object destructure/chaining jaise `{...}.name` ko dhyan se padhna — ye trap questions me bahut aata hai.
- 💡 Trick: "Refresh count + 1 = total executions" jab tak initial load bhi count ho.

---

## Similar Question Pattern
LocalStorage/sessionStorage persistence ke saath "refresh N times" wale trace-the-output questions bahut common hain MAD2 me.

---

## Revision Box
localStorage data refresh/reload ke baad bhi persist karta hai. 1 initial load + 2 refresh = 3 executions, har execution me `'Laptop'` push hota hai.

---
---

# Question 6

## Original Question
> Match the following storage mechanisms used in the browser with their correct characteristics.

| Storage Type | Characteristic |
|---|---|
| 1. Session Storage | A. Data automatically encrypts itself without developer intervention. |
| 2. Token Storage | B. Data persists only until the browser/tab is closed. |
| 3. Cookie Storage | C. Often used to store authentication tokens securely on the client side. |
| | D. Data persists across sessions, typically stored on the client and sent with every HTTP request. |

> Options:
> A. 1→B, 2→C, 3→A
> B. 1→B, 2→A, 3→C
> C. 1→A, 2→C, 3→B
> D. 1→B, 2→C, 3→D

---

## Correct Answer
**Correct Option:** D (1→B, 2→C, 3→D)

---

## Concept Used
- 📘 **Session Storage:** Data sirf tab tak rehta hai jab tak wo tab/browser open ho. Tab close karte hi data delete ho jaata hai.
- 📘 **Token Storage (jaise `localStorage`/`sessionStorage` used for tokens):** Authentication tokens (jaise JWT) client side pe securely store karne ke liye use hota hai — session/local storage isme use ho sakte hain.
- 📘 **Cookie Storage:** Cookies data ko client pe store karte hain aur ye data **automatically har HTTP request ke saath server ko bhej diya jaata hai**. Cookies sessions ke across bhi persist kar sakte hain (agar expiry set ho).
- 📘 Exam me is type ke matching questions me concept clarity chahiye — kaunsa storage kis use-case ke liye best hai.

---

## Step-by-Step Solution
1. **1. Session Storage → B:** "Data persists only until the browser/tab is closed" — Ye definition exactly Session Storage ki hai.
   - *Reason:* Session Storage tab-specific hota hai, close hote hi gayab.
2. **2. Token Storage → C:** "Often used to store authentication tokens securely on the client side" — Token Storage ka primary use hi authentication tokens rakhna hota hai.
   - *Reason:* Naam se hi clear hai — "Token Storage" ka matlab tokens ko store karna.
3. **3. Cookie Storage → D:** "Data persists across sessions, typically stored on client and sent with every HTTP request" — Cookies ki ye sabse unique property hai ki wo automatically har request ke sath server ko bheji jaati hain.
   - *Reason:* Ye cookies ko `localStorage`/`sessionStorage` se differentiate karta hai — sirf cookies hi automatically request headers me jaati hain.
4. **Leftover option A** ("Data automatically encrypts itself") kisi bhi storage type se match nahi hota — koi bhi client-side storage automatically encrypt nahi hoti, ye ek **distractor** hai.

---

## Final Answer
**1→B, 2→C, 3→D**

---

## Why Other Options are Wrong?
### Option A (1→B, 2→C, 3→A)
Cookie storage ko "auto-encrypts" bolna galat hai — cookies by default plain text hote hain, encryption manually implement karni padti hai.

### Option B (1→B, 2→A, 3→C)
Token Storage ko "auto-encrypts" bolna galat — aur Cookie ko "auth token storage" bolna bhi incomplete hai, kyunki cookie ki asli defining property "sent with every HTTP request" hai.

### Option C (1→A, 2→C, 3→B)
Session Storage ko "auto-encrypts" bolna galat hai — Session storage ki defining property tab-close pe delete hona hai, encryption nahi.

---

## Important Exam Notes
- ✅ Session Storage → tab close = data gone.
- ✅ Local/Token Storage → persists, often used for auth tokens.
- ✅ Cookies → sent automatically with every HTTP request, persist across sessions (based on expiry).
- ⚠️ Common Mistake: "Auto-encryption" wala option kabhi bhi kisi storage type se match mat karna — ye hमेशा एक trap hota hai.
- 💡 Trick: "**C**ookie = **C**arried with every request"

---

## Similar Question Pattern
Storage mechanisms (localStorage vs sessionStorage vs cookies) ki matching/comparison questions bahut common hain MAD2 me.

---

## Revision Box
Session Storage = tab-close pe gone. Token Storage = auth tokens ke liye. Cookies = client pe store + automatically har HTTP request ke sath bheji jaati hain.

---
---

# Question 7

## Original Question
**index.html:**
```html
<div id="app">
  <h4>total payable amount is {{totalPayableAmount}}</h4>
</div>
<script src="app.js"></script>
```

**app.js:**
```js
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
    },
  },
})

appData = [
  [2000, 10, 2],
  [3000, 20, 3],
  [5000, 30, 4],
]
let handler = setInterval(() => {
  data = appData.pop()
  app.principal = data[0]
  app.annualInterestRate = data[1]
  app.duration = data[2]
  app.totalPayableAmount += app.simpleInterest
}, 1000)
```
> What will be rendered by the browser after 4 seconds?
>
> Options: A. 6000  B. 8200  C. 1800  D. 7800

---

## Correct Answer
**Correct Option:** B (8200)

---

## Concept Used
- 📘 **`setInterval`:** Ek function ko har **N milliseconds** baad baar-baar call karta hai. Yaha `1000` ka matlab hai har **1 second** baad function chalega.
- 📘 **Array `.pop()`:** Array ke **last element** ko nikaal kar return karta hai aur array se delete kar deta hai (LIFO — Last In First Out order).
- 📘 **Vue Computed Property:** `computed` properties automatically recalculate hoti hain jab bhi unke andar use hone wale reactive data (`principal`, `annualInterestRate`, `duration`) change hote hain.
- 📘 Simple Interest Formula: `SI = (Principal × Rate × Time) / 100`

---

## Step-by-Step Solution
1. **Step 1: Samjho array ka order.** `appData = [[2000,10,2], [3000,20,3], [5000,30,4]]`. `.pop()` **last se** element nikalta hai, isliye order hoga: pehle `[5000,30,4]`, phir `[3000,20,3]`, phir `[2000,10,2]`.
   - *Reason:* `.pop()` hamesha array ke end se element hataata hai.
2. **Step 2: t = 1 second pe (1st interval call):** `data = [5000, 30, 4]` → `principal=5000, rate=30, duration=4`.
   - SI = (5000 × 30 × 4) / 100 = 600000/100 = **6000**
   - `totalPayableAmount = 0 + 6000 = 6000`
3. **Step 3: t = 2 seconds pe (2nd interval call):** `data = [3000, 20, 3]` → `principal=3000, rate=20, duration=3`.
   - SI = (3000 × 20 × 3) / 100 = 180000/100 = **1800**
   - `totalPayableAmount = 6000 + 1800 = 7800`
4. **Step 4: t = 3 seconds pe (3rd interval call):** `data = [2000, 10, 2]` → `principal=2000, rate=10, duration=2`.
   - SI = (2000 × 10 × 2) / 100 = 40000/100 = **400**
   - `totalPayableAmount = 7800 + 400 = 8200`
5. **Step 5: t = 4 seconds pe (4th interval call):** Ab `appData` khali ho chuka hai (sab pop ho gaye). `appData.pop()` empty array pe `undefined` return karega, aur agli line `data[0]` execute karte hi **error** aayega (kyunki `undefined[0]` invalid hai) — is wajah se `totalPayableAmount` update **nahi** hoga, wo apni last successful value (`8200`) pe hi ruka rahega.
   - *Reason:* Jab array khaali ho jaata hai, `.pop()` `undefined` deta hai, aur code crash ho jaata hai — screen pe last successful render hi dikhega.

**Shortcut:** Total 3 data-points hain, isliye sirf pehle 3 seconds tak hi valid updates honge. 4th second pe answer same rahega jo 3rd second ke baad tha.

---

## Final Answer
**"total payable amount is 8200"**

---

## Why Other Options are Wrong?
### Option A (6000)
Ye sirf 1st interval (t=1s) ke baad ki value hai, final (t=4s) ki nahi.

### Option C (1800)
Ye galat hai — 1800 sirf 2nd data point ka individual SI hai, cumulative total nahi.

### Option D (7800)
Ye 2nd interval (t=2s) ke baad ki cumulative value hai, final answer nahi (3rd interval baaki hai).

---

## Important Exam Notes
- ✅ `.pop()` array ke end se element nikalta hai — order reverse ho jaata hai.
- ✅ `setInterval(fn, 1000)` har 1000ms (1 sec) pe `fn` chalata hai.
- ⚠️ Common Mistake: Array ke start se elements process karna assume karna — actually `.pop()` end se karta hai.
- 💡 Trick: Jab bhi array khatam ho jaaye aur loop/interval chalta rahe, dhyan do ki age errors aa sakte hain — last valid state hi dikhta hai.

---

## Similar Question Pattern
Vue computed properties ke saath `setInterval`/timer-based reactive updates ke trace-output questions common hain — dhyan se timing aur array methods dono track karne hote hain.

---

## Revision Box
`.pop()` array ke last se elements nikalta hai. Har second ek naya data process hota hai aur `totalPayableAmount` me SI add hoti hai. 3 seconds me total = 6000+1800+400 = **8200**. 4th second pe array khali hone se update ruk jaata hai.

---
---

# Question 8

## Original Question
```js
const course = {
  courseName: 'Modern Application Development 2',
  courseCode: 'mad2',
}
const student = {
  __proto__: course,
  studentName: 'Rakesh',
  studentCity: 'Delhi',
}
const { courseName } = student
console.log(courseName)
```
> Options: A. Undefined  B. Modern Application Development 2  C. Will throw syntax error  D. None of these

---

## Correct Answer
**Correct Option:** B (Modern Application Development 2)

---

## Concept Used
- 📘 **Prototype Chain:** JavaScript me har object ka ek "hidden link" hota hai apne **prototype** object se (`__proto__` property se access hota hai). Agar kisi property ko object khud nahi rakhta, to JS engine us property ko **prototype chain** me upar jaake dhoondta hai.
- 📘 **`__proto__`:** Ye ek object ka reference hota hai uske prototype (parent) object ki taraf. Yaha `student.__proto__ = course`, matlab `student` ka prototype `course` hai.
- 📘 **Destructuring (`{ courseName } = student`):** Destructuring bhi normal property access ki tarah kaam karta hai — matlab agar property object me directly na ho, to ye bhi prototype chain follow karega.

**Example:**
```js
const parent = { greet: "hello" };
const child = { __proto__: parent };
console.log(child.greet); // "hello" (found via prototype chain)
const { greet } = child;
console.log(greet); // "hello" (destructuring also checks prototype chain)
```

---

## Step-by-Step Solution
1. **Step 1:** `course` object banaya gaya hai jisme `courseName` property hai (`'Modern Application Development 2'`).
2. **Step 2:** `student` object banaya gaya hai, jiska `__proto__` explicitly `course` set kiya gaya hai. Matlab `student` object khud `courseName` property nahi rakhta, lekin uska **prototype** (`course`) rakhta hai.
   - *Reason:* `__proto__: course` likh kar humne prototype chain manually link kar di.
3. **Step 3:** `const { courseName } = student` — ye destructuring `student` object se `courseName` nikalne ki koshish karta hai.
   - *Reason:* Destructuring engine pehle `student` ki apni properties check karta hai (`studentName`, `studentCity`) — `courseName` wahan nahi milta.
4. **Step 4:** Kyunki `courseName` `student` me directly nahi hai, JS engine prototype chain follow karta hai aur `student.__proto__` (yaani `course`) me check karta hai — wahan `courseName` mil jaata hai.
   - *Reason:* Property lookup (chahe dot notation ho ya destructuring) hamesha prototype chain traverse karta hai jab tak property mil na jaaye ya chain khatam na ho jaaye.
5. **Step 5:** `courseName` = `'Modern Application Development 2'` print hota hai.

**Shortcut:** Jab bhi `__proto__` explicitly set dikhe object literal me, turant socho "prototype chain lookup" — property directly na mile to parent me dhoondo.

---

## Final Answer
**"Modern Application Development 2"**

---

## Why Other Options are Wrong?
### Option A (Undefined)
Galat hai — ye tab hota agar `student` object ka prototype set na hota, ya `course` me `courseName` na hoti. Yaha dono conditions satisfy hain, isliye value mil jaati hai.

### Option C (Will throw syntax error)
Galat hai — `__proto__` ko object literal me directly set karna valid JavaScript syntax hai, koi error nahi aata.

### Option D (None of these)
Galat hai kyunki Option B (correct answer) already present hai list me.

---

## Important Exam Notes
- ✅ Destructuring bhi prototype chain follow karta hai, jaise normal property access karta hai.
- ✅ `__proto__` object literal ke andar likh kar directly prototype link set kar sakte hain.
- ⚠️ Common Mistake: Ye samajhna ki destructuring sirf object ki "own properties" tak limited hai — actually ye prototype chain bhi dekhta hai.
- 💡 Trick: "Property missing? Check the Parent (prototype)!"

---

## Similar Question Pattern
Prototype chain, `__proto__`, `Object.create()`, aur inheritance-based property lookup questions MAD2 me common hain, especially destructuring ke saath combine kiye hue.

---

## Revision Box
`student` object ka prototype `course` hai. `courseName` `student` me nahi, `course` me hai — prototype chain se mil jaata hai. Destructuring bhi normal property access jaisa hi behave karta hai, chain follow karta hai.

---
---

# Question 9

## Original Question
```js
let x = 2;
function op(x) {
  x *= 3;
  x += 4;
  x -= 1;
}
op();
op();
console.log(x);
```
> What is the final value of `x`, logged on the console? (Numeric Answer)

---

## Correct Answer
**Correct Answer:** `2`

---

## Concept Used
- 📘 **Variable Shadowing:** Jab function ka **parameter** ka naam outer/global variable ke naam se same hota hai, to function ke andar wahi naam **local parameter** ko refer karta hai, outer variable ko nahi. Ye "shadowing" kehlaata hai.
- 📘 **Function Parameters:** Function call karte time agar argument pass nahi kiya jaaye (jaise `op()` bina kisi value ke), to parameter ki value `undefined` hoti hai (ya default value agar di ho).
- 📘 Har function call apna **naya local scope** banata hai — is scope ke andar ka `x` bilkul alag hai outer `x` se.

**Example:**
```js
let y = 10;
function change(y) {
  y = 100; // ye local y hai
}
change();
console.log(y); // 10 (outer y untouched)
```

---

## Step-by-Step Solution
1. **Step 1:** `let x = 2;` — Ye **outer/global** scope ka `x` hai, value `2`.
2. **Step 2:** `function op(x) { ... }` — Is function ka parameter bhi `x` naam ka hai. Jab function ke andar `x` use hota hai, wo is **local parameter `x`** ko refer karega, outer wale `x` ko nahi (shadowing).
   - *Reason:* JavaScript ka scope rule hai — sabse "nearest" (closest) `x` use hota hai, jo yaha function parameter hai.
3. **Step 3:** `op();` — Pehla call, koi argument pass nahi kiya gaya, isliye local `x = undefined`.
   - `x *= 3` → `undefined * 3 = NaN`
   - `x += 4` → `NaN + 4 = NaN`
   - `x -= 1` → `NaN - 1 = NaN`
   - Ye saari calculations sirf **local `x`** pe ho rahi hain, outer `x` (jo 2 hai) ko koi touch nahi kar raha!
   - *Reason:* Function ke andar `x` local parameter hai, outer `x` se koi link nahi (function `return` bhi nahi kar raha aur na hi outer `x` ko modify kar raha hai).
4. **Step 4:** `op();` — Doosra call bhi same tarah chalta hai, phir se local `x = undefined → NaN` sequence, outer `x` still untouched.
   - *Reason:* Har function call independent hota hai, purani calls ka effect naye call pe nahi padta (koi shared state nahi hai yaha).
5. **Step 5:** `console.log(x);` — Ye **outer/global** `x` ko print kar raha hai (function ke bahar hai ye statement), jo shuru se **2** hi hai, kyunki function ke andar ki koi bhi calculation isse touch nahi karti.
   - *Reason:* Function ke andar ka `x` (parameter) sirf function ke andar hi exist karta hai — function khatam hote hi wo destroy ho jaata hai, outer `x` par koi asar nahi padta.

**Shortcut:** Jab bhi function parameter aur outer variable ka naam same ho, turant check karo — "kya function outer variable ko explicitly modify kar raha hai (jaise `window.x` ya bina naam-clash wale parameter se)?" Agar nahi, to outer variable **safe** hai.

---

## Final Answer
**2**

---

## Why Other Options are Wrong?
Ye Short Answer Question hai (numeric type), isliye "options" nahi hain — lekin common **galat answers** jo students de sakte hain:

### "NaN"
Students confuse ho sakte hain aur function ke andar wali `NaN` calculation ko final answer maan sakte hain — lekin `console.log(x)` **function ke bahar** hai, outer `x` print karega, jo 2 hi hai.

### "8" ya koi calculated value
Agar koi galti se sochta hai ki `op()` outer `x` ko modify kar raha hai (jaise `2*3+4-1=9` type calculation), to wo galat hoga, kyunki parameter shadowing ki wajah se outer `x` touch hi nahi hota.

---

## Important Exam Notes
- ✅ Function parameter automatically ek **naya local variable** banata hai, chahe naam outer variable se match kare.
- ✅ Shadowing hone par local variable priority leta hai function ke andar.
- ⚠️ Common Mistake: Function ke andar ki calculations ko outer variable pe apply maan lena — galat hai jab tak explicit modification (return + reassignment, ya closure) na ho.
- 💡 Trick: "Same naam ho to bhi, parameter hamesha apna alag box banata hai."

---

## Similar Question Pattern
Variable shadowing + function scope wale trace-output numeric-answer questions common hain — dhyan rakhna hai ki kaunsa `x` (local ya global) print ho raha hai.

---

## Revision Box
Function parameter `x` outer `x` ko shadow karta hai — function ke andar ki calculations sirf local `x` pe hoti hain. Outer `x` untouched rehta hai, isliye final `console.log(x)` = **2**.

---
---

# Question 10

## Original Question
> Which of the following best describes UI State (Ephemeral State) in frontend applications?
>
> Options:
> A. It includes all data stored permanently in the database and is shared across users
> B. It represents the complete system data such as users, products, and transactions
> C. It refers to short-lived interface elements like loading indicators or selected tabs
> D. It handles complex application logic and long-term session management

---

## Correct Answer
**Correct Option:** C

---

## Concept Used
- 📘 **UI State (Ephemeral State):** Ye wo state hoti hai jo sirf **temporary/short-term** hoti hai aur sirf frontend (browser) tak limited hoti hai. Ye database me store nahi hoti, aur page refresh/navigate hone pe generally lost ho jaati hai.
  - Examples: kaunsa tab selected hai, loading spinner show ho raha hai ya nahi, dropdown open hai ya closed, form field ka temporary input.
- 📘 **Contrast — Application/Server State:** Ye wo data hota hai jo **permanently** database me store hota hai aur multiple users ke beech shared ho sakta hai (jaise user profiles, products, orders).
- 📘 Frontend applications me in dono states ko differentiate karna important hai kyunki inko manage karne ke tools/approach alag hote hain (jaise React me local `useState` UI state ke liye, aur Redux/API calls server state ke liye).

---

## Step-by-Step Solution
1. **Step 1:** Definition dhyan se padho — "UI State (Ephemeral State)" ka naam khud clue de raha hai: **"Ephemeral"** ka matlab hota hai **short-lived / temporary**.
   - *Reason:* Word meaning se hi concept samajh aata hai.
2. **Step 2:** Option C — "short-lived interface elements like loading indicators or selected tabs" — ye directly "ephemeral" (temporary) nature ko describe karta hai.
   - *Reason:* Loading indicator sirf tab tak dikhta hai jab tak data load ho raha ho — uske baad gayab ho jaata hai. Selected tab bhi sirf current session/interaction tak relevant hoti hai.
3. **Step 3:** Baaki saare options (A, B, D) **permanent/complex/database-level** data ke baare me baat kar rahe hain, jo UI State ka opposite concept hai.

---

## Final Answer
**"It refers to short-lived interface elements like loading indicators or selected tabs"**

---

## Why Other Options are Wrong?
### Option A
Ye **Server/Database State** ki definition hai, na ki UI State ki. Database data permanent hota hai, UI state temporary.

### Option B
Ye bhi **Application/Business Data** (users, products, transactions) ki baat kar raha hai, jo backend/server pe manage hoti hai, frontend ki ephemeral UI state nahi.

### Option D
Ye "long-term session management" ki baat karta hai, jo UI State ke bilkul opposite hai — UI state short-term hoti hai, long-term nahi.

---

## Important Exam Notes
- ✅ UI State = temporary, frontend-only, resets easily (loading spinners, selected tab, modal open/close).
- ✅ Server/App State = permanent, database-backed, shared across users.
- ⚠️ Common Mistake: UI state ko "application data" samajh lena.
- 💡 Trick: Word **"Ephemeral"** yaad rakho = **temporary/short-lived**.

---

## Similar Question Pattern
Conceptual definition-based MCQs (UI state vs server state, client-side vs server-side rendering, etc.) frequently aate hain MAD2 me.

---

## Revision Box
UI State (Ephemeral State) = temporary, short-lived frontend elements jaise loading spinners, selected tabs. Ye database me store nahi hoti aur easily reset ho jaati hai.

---
---

# Question 11

## Original Question
> Given that HTTP is stateless, which approach is commonly used to manage application state between the client and the server?
>
> Options:
> A. Storing all state permanently in frontend memory across sessions
> B. Allowing the frontend to handle complex business logic and data storage
> C. Client or server maintaining state and explicitly exchanging it through requests
> D. Eliminating state entirely from web applications

---

## Correct Answer
**Correct Option:** C

---

## Concept Used
- 📘 **HTTP Stateless Protocol:** HTTP protocol by design **stateless** hai — matlab har request **independent** hoti hai, server ko pichhli request ka koi memory/context nahi hota. Har request apne aap me complete honi chahiye.
- 📘 **State Management Solution:** Kyunki HTTP khud state yaad nahi rakhta, developers ko **manually** state manage karni padti hai. Common approaches:
  - **Cookies** (server client ko cookie bhejta hai, client har request me wapas bhejta hai)
  - **Sessions** (server side state, session ID cookie/token se track hoti hai)
  - **Tokens (JWT)** (client token store karta hai aur har request ke sath explicitly bhejta hai — jaise `Authorization` header)
- 📘 Ye concept web development ka **foundation** hai — samajhna zaroori hai ki client aur server dono milkar explicitly state ko "exchange" karte hain requests ke through, taaki application "stateful-jaisi" feel de, jabki underlying protocol stateless hai.

---

## Step-by-Step Solution
1. **Step 1:** Samjho HTTP stateless kyu hai — har naya request server ke liye "fresh/naya" hota hai, koi built-in memory nahi.
   - *Reason:* Ye protocol simplicity aur scalability ke liye design kiya gaya tha.
2. **Step 2:** Isliye application-level pe state ko maintain karne ka koi tarika chahiye — ya to client apni state store kare aur har request me bheje (jaise token), ya server apni taraf session store kare aur ek identifier (session ID) client ko de jo wo wapas bheje.
   - *Reason:* Dono taraf explicit "exchange" hona zaroori hai taaki continuity bani rahe.
3. **Step 3:** Option C exactly ye describe karta hai — "Client or server maintaining state and explicitly exchanging it through requests" — ye industry-standard approach hai (cookies, tokens, sessions).

---

## Final Answer
**"Client or server maintaining state and explicitly exchanging it through requests"**

---

## Why Other Options are Wrong?
### Option A
"Storing all state permanently in frontend memory" galat hai kyunki frontend memory (jaise JS variables) page refresh pe reset ho jaati hai — ye reliable state management nahi hai.

### Option B
Frontend ko "complex business logic aur data storage" handle karne dena bad practice hai — security aur data integrity ke liye business logic server-side honi chahiye.

### Option D
"Eliminating state entirely" impossible hai — real-world applications (login, cart, session) ko state chahiye hi hoti hai kaam karne ke liye.

---

## Important Exam Notes
- ✅ HTTP = stateless by design.
- ✅ State management tools: Cookies, Sessions, Tokens (JWT).
- ⚠️ Common Mistake: Sochna ki modern web apps "stateless" hi rehti hain — actually application level pe state explicitly manage ki jaati hai.
- 💡 Trick: "**S**tateless HTTP + **E**xplicit exchange = Working Web Apps"

---

## Similar Question Pattern
HTTP fundamentals (stateless nature, cookies vs sessions vs tokens, REST principles) conceptual MCQs common hain.

---

## Revision Box
HTTP stateless hota hai, isliye state manage karne ke liye client aur server explicitly data exchange karte hain requests ke through (cookies, sessions, tokens use karke).

---
---

# Question 12

## Original Question
```js
function outer() {
    let count = 0;
    return function inner() {
        return count++;
    };
}

const c1 = outer();
const c2 = outer();

console.log(c1())
console.log(c1())
console.log(c2())
```
> What will be the output of the above program?
>
> Options:
> A. 1, 2, 1
> B. 0, 0, 0
> C. 1, 2, 3
> D. 0, 1, 0

---

## Correct Answer
**Correct Option:** D (0, 1, 0)

---

## Concept Used
- 📘 **Closures:** Ek **closure** tab banti hai jab ek inner function apne outer function ke variables ko "yaad" rakhta hai, chahe outer function ka execution khatam ho gaya ho. Har baar jab outer function call hota hai, ek **naya, independent** closure environment banta hai.
- 📘 **Post-increment (`count++`):** Ye pehle current value **return** karta hai, phir value ko increment karta hai. Isliye `count++` jab `count=0` hota hai, to ye **0 return karega** aur uske baad `count` ho jayega `1`.
- 📘 Har `outer()` call apna **alag `count` variable** banata hai — ye ek dusre se completely independent hote hain (isolated memory).

**Example:**
```js
function counter() {
  let n = 0;
  return () => n++;
}
const a = counter();
const b = counter();
console.log(a()); // 0
console.log(a()); // 1 (a's own n incremented)
console.log(b()); // 0 (b has its OWN separate n)
```

---

## Step-by-Step Solution
1. **Step 1:** `const c1 = outer();` — `outer()` call hota hai, `count = 0` banta hai is closure ke andar, aur `inner` function return hota hai jo is `count` ko "yaad" rakhta hai. `c1` ab is specific `inner` function ko refer karta hai jiska apna `count = 0` hai.
   - *Reason:* Har `outer()` call apna naya scope/environment banata hai.
2. **Step 2:** `const c2 = outer();` — Ek **naya, alag** `outer()` call hota hai. Isme bhi `count = 0` banta hai, lekin ye **c1 wale count se bilkul different variable hai** (alag memory location).
   - *Reason:* Har function call independent execution context banata hai.
3. **Step 3:** `console.log(c1())` — Pehli baar `c1` call ho raha hai. Iske andar `count++` chalta hai jab `count = 0`. Post-increment hone ki wajah se **pehle 0 return hota hai**, phir `count` ho jaata hai `1`.
   - Output: **0**
4. **Step 4:** `console.log(c1())` — Doosri baar `c1` call ho raha hai. Ab is closure ka `count` already `1` hai (pichhli call se update hua). `count++` → **1 return hota hai**, phir `count` ho jaata hai `2`.
   - Output: **1**
5. **Step 5:** `console.log(c2())` — Ye `c2` hai, jiska **apna alag `count`** hai jo abhi tak `0` hi hai (kyunki `c2` pehli baar call ho raha hai, `c1` ki calls ka isse koi lena dena nahi).
   - `count++` → **0 return hota hai**, phir `count` ho jaata hai `1`.
   - Output: **0**

**Shortcut:** Jab bhi `outer()` do baar call ho (do alag const jaise `c1`, `c2`), turant socho — "dono ka apna **separate** closure hai, ek dusre ko affect nahi karte."

---

## Final Answer
**0, 1, 0**

---

## Why Other Options are Wrong?
### Option A (1, 2, 1)
Ye galat hai kyunki `count++` **post-increment** hai — pehli call pe `0` return hona chahiye, `1` nahi (agar `++count` hota to `1` sahi hota).

### Option B (0, 0, 0)
Ye galat hai kyunki closure ke andar `count` **persist/remember** karta hai apni value calls ke beech — `c1`'s second call pe `count` already `1` ho chuka hoga (0 nahi).

### Option C (1, 2, 3)
Ye galat hai — ye tab hota agar `c1` aur `c2` **same** `count` share kar rahe hote (jaise agar `count` global hota) — lekin har `outer()` call apna independent `count` banata hai.

---

## Important Exam Notes
- ✅ Har function call = naya closure = naya independent variable environment.
- ✅ `x++` (post-increment) = pehle current value return, phir increment.
- ✅ `++x` (pre-increment) = pehle increment, phir new value return.
- ⚠️ Common Mistake: Post vs Pre increment confuse karna.
- 💡 Trick: "Naya `outer()` call = Naya `count`, purane se koi relation nahi!"

---

## Similar Question Pattern
Closures + counters + multiple instances ke trace-output questions bahut common hain — memoization, private variables jaise concepts bhi isi tarah test hote hain.

---

## Revision Box
Har `outer()` call apna independent closure/`count` banata hai. `count++` pehle purani value return karta hai phir increment karta hai. `c1` ka apna counter (0→1→2), `c2` ka alag counter (0→1) hai — dono independent.

---
---

# Question 13

## Original Question
**File: index.html**
```html
<div id="app">
  <my-comp>
    <h3>Exploring Vue Js</h3>

    <slot v-slot:last>
      <h3>Learning App Dev 2</h3>
    </slot>
  </my-comp>
</div>
```

**File: script.js**
```js
const MyComp = {
  name: 'my-comp',
  props: ['tech'],
  template: `
    <div class="container">
      <slot name="complete">
        <h3>Learned DBMS</h3>
      </slot>

      <slot>
        Exploring Frontend
      </slot>

      <slot name="last"></slot>
    </div>
  `
};

const app = new Vue({
  el: '#app',
  components: {
    MyComp
  }
});
```
> What will be rendered on the browser for the code setup given above?

---

## Correct Answer
**Correct Option:** The rendering shows **"Learned DBMS"**, **"Exploring Vue Js"**, and **"Learning App Dev 2"** (parent-content Vue Js line replaces the unnamed slot's fallback, "complete" slot keeps its fallback, and "last" named slot gets its content).

---

## Concept Used
- 📘 **Vue Slots:** Slots ek tarah ka "placeholder" hote hain child component ke template me, jaha **parent component** apna content inject kar sakta hai.
- 📘 **Default (Unnamed) Slot:** `<slot>...</slot>` (bina naam ke) — parent jo bhi content bina `v-slot` label ke deta hai, wo yahan aata hai. Agar parent kuch nahi deta, to slot ka apna **fallback content** dikhta hai.
- 📘 **Named Slots:** `<slot name="xyz">` — parent ko specifically `v-slot:xyz` use karke content dena hota hai is slot ke liye. Agar parent us naam ka content nahi deta, to slot ka **fallback content** dikhta hai.
- 📘 **Fallback Content:** Slot tag ke andar jo content likha hota hai (jaise `<slot name="complete"><h3>Learned DBMS</h3></slot>`), wo tab dikhta hai jab parent us slot ko **kuch content nahi deta**. Agar parent deta hai, to parent ka content fallback ko **replace** kar deta hai.

**Example:**
```html
<!-- Child template -->
<slot name="title">Default Title</slot>

<!-- Parent usage 1: no content passed -->
<my-comp></my-comp>  <!-- shows "Default Title" -->

<!-- Parent usage 2: content passed -->
<my-comp><template v-slot:title>Custom Title</template></my-comp> <!-- shows "Custom Title" -->
```

---

## Step-by-Step Solution
1. **Step 1: Component ke 3 slots identify karo:**
   - `slot name="complete"` → fallback: "Learned DBMS"
   - `slot` (unnamed/default) → fallback: "Exploring Frontend"
   - `slot name="last"` → koi fallback nahi (empty)
2. **Step 2: Parent (index.html) me diya gaya content dekho:**
   - `<h3>Exploring Vue Js</h3>` — koi `v-slot` naam nahi diya, isliye ye **default/unnamed slot** ke liye content hai.
   - `<slot v-slot:last><h3>Learning App Dev 2</h3></slot>` — ye `v-slot:last` label ke sath diya gaya hai, isliye ye **"last" named slot** ke liye content hai.
3. **Step 3: "complete" slot ke liye check karo** — Parent ne is naam ka koi content diya nahi, isliye is slot ka **fallback** dikhega: "**Learned DBMS**".
   - *Reason:* Jab parent kisi named slot ke liye content nahi deta, fallback use hota hai.
4. **Step 4: unnamed/default slot ke liye check karo** — Parent ne `<h3>Exploring Vue Js</h3>` diya hai bina kisi naam ke, isliye ye **default slot ka fallback ("Exploring Frontend") replace** kar dega.
   - Result: "**Exploring Vue Js**" dikhega, "Exploring Frontend" nahi.
   - *Reason:* Parent ka content hamesha slot ke fallback ko override karta hai.
5. **Step 5: "last" named slot ke liye check karo** — Parent ne `v-slot:last` se `<h3>Learning App Dev 2</h3>` diya hai, isliye ye is slot me render hoga.
   - Result: "**Learning App Dev 2**"
   - *Reason:* Named slot ka fallback (agar hota) parent ke content se replace ho jaata, yaha to fallback tha hi nahi (empty slot), so parent ka content directly aa jaata hai.
6. **Final Rendered Content (top to bottom):** "Learned DBMS" → "Exploring Vue Js" → "Learning App Dev 2"

---

## Final Answer
**"Learned DBMS", "Exploring Vue Js", "Learning App Dev 2"** (in this order) render hote hain.

---

## Why Other Options are Wrong?
### Option "Exploring Backend" wala option
Galat hai kyunki na to component me "Exploring Backend" text hai, aur na hi parent ne aisa content diya hai — ye ek completely fabricated distractor hai.

### "None of these" wala option
Galat hai kyunki correct combination already ek option me maujood hai (Learned DBMS + Exploring Vue Js + Learning App Dev 2).

### Options jo "Exploring Frontend" dikhate hain
Galat hain kyunki default slot ka fallback ("Exploring Frontend") parent ke diye hue content ("Exploring Vue Js") se **replace** ho jaata hai — dono ek sath nahi dikhte.

---

## Important Exam Notes
- ✅ Named slot: `<slot name="x">` — parent `v-slot:x` se content deta hai.
- ✅ Default slot: `<slot>` — parent bina naam ke seedha content de sakta hai.
- ✅ Fallback content sirf tab dikhta hai jab parent koi content na de us slot ke liye.
- ⚠️ Common Mistake: Sochna ki fallback aur parent ka content **dono ek sath** render honge — Actually parent ka content fallback ko **completely replace** karta hai.
- 💡 Trick: "Parent gives content? Fallback goes away!"

---

## Similar Question Pattern
Vue slots (named, default, fallback content, scoped slots) ke rendering-output questions frequently aate hain — dhyan se dekhna hota hai kaunsa content kis slot me map ho raha hai.

---

## Revision Box
3 slots: "complete" (fallback used - "Learned DBMS"), default (parent content used - "Exploring Vue Js"), "last" (parent content used via `v-slot:last` - "Learning App Dev 2"). Parent content hamesha fallback ko replace karta hai jab diya gaya ho.

---
---

# Question 14

## Original Question
```js
function createCounter(start) {
  let count = start;

  return function (step) {
    if (typeof step === "number") {
      count += step;
      return count;
    }

    return (function () {
      let temp = count;

      return function (reset = false) {
        if (reset) {
          count = start;
        } else {
          count++;
        }
        return temp + count;
      };
    })();
  };
}

const counter = createCounter(3);

let a = counter(2);
let d = counter()(true);

a + d;
```
> What would be the end result at the end of execution of the code?
>
> Options: A. 5  B. 13  C. 8  D. 11

---

## Correct Answer
**Correct Option:** B (13)

---

## Concept Used
- 📘 **Nested Closures:** Ye code multiple levels ki **closures** use karta hai — ek function dusre function ko return karta hai, jo aage teesra function return karta hai. Har level apne outer scope ke variables (`count`, `start`, `temp`) ko "yaad" rakhta hai.
- 📘 **IIFE (Immediately Invoked Function Expression):** `(function(){ ... })()` — ye ek function hai jo define hote hi turant call ho jaata hai. Yaha ye `temp = count` ko **capture** karne ke liye use hua hai (current `count` ki value snapshot lene ke liye).
- 📘 **Conditional Branching based on `typeof`:** Function decide karta hai ki agar argument ek **number** hai to ek path lo, warna doosra path (nested closure return karo).
- 📘 **Default Parameters:** `function(reset = false)` — agar argument na diya jaaye to `reset` ki default value `false` hogi.

---

## Step-by-Step Solution
1. **Step 1:** `const counter = createCounter(3);` — `createCounter` call hota hai `start = 3`. Andar `let count = start` → `count = 3`. Ye `count` is closure ke andar reh jaata hai. `counter` ab us inner function ko refer karta hai jo `step` parameter leta hai.
   - *Reason:* `createCounter` apna "private" `count` variable banata hai jo bahar se directly access nahi ho sakta, sirf returned function ke through.

2. **Step 2:** `let a = counter(2);` — `counter(2)` call hota hai, matlab `step = 2`.
   - `typeof step === "number"` → `typeof 2 === "number"` → **true**
   - `count += step` → `count = 3 + 2 = 5`
   - `return count` → returns **5**
   - `a = 5`
   - *Reason:* Kyunki `2` ek number hai, `if` block execute hota hai, jo directly `count` ko update aur return karta hai.

3. **Step 3:** `let d = counter()(true);` — Ye do parts me hota hai:
   - **Part A: `counter()`** — koi argument nahi diya, isliye `step = undefined`.
     - `typeof step === "number"` → `typeof undefined === "number"` → **false**
     - Isliye `if` block skip hota hai, aur neeche wala IIFE chalta hai:
       - `let temp = count;` — is waqt `count = 5` hai (Step 2 se update hua tha), isliye `temp = 5`.
       - IIFE ek naya function return karta hai jo `reset` parameter leta hai (default `false`).
     - `counter()` ka result = ye returned inner function (jisme `temp = 5` "captured" hai closure se).
     - *Reason:* `temp` us waqt ke `count` ki value ko "freeze/snapshot" kar leta hai jab ye IIFE chala.
   - **Part B: `(...)(true)`** — Ab humne upar wale returned function ko `true` argument ke sath call kiya (`reset = true`).
     - `if (reset)` → `true` hai, isliye: `count = start` → `count = 3` (reset ho gaya wapas starting value pe).
     - `return temp + count` → `temp` (5) + naya `count` (3) = **8**
     - `d = 8`
     - *Reason:* `reset = true` hone ki wajah se `count` wapas `start` (3) pe reset ho gaya, phir `temp` (jo 5 tha, pehle ka snapshot) + naya `count` (3) add kiya gaya.

4. **Step 4:** `a + d;` — `5 + 8 = 13`
   - *Reason:* Dono independent calculations ka final sum yehi expression evaluate karta hai (halaki ye result kahi print nahi ho raha, lekin ye "end result" hai jo evaluate hota hai).

**Shortcut:** Aise nested-closure questions me **hamesha ek table banao** — track karo `count` ki value har step ke baad, aur `temp` kab capture hua.

| Step | Action | `count` value | Result |
|---|---|---|---|
| Initial | `createCounter(3)` | 3 | - |
| `counter(2)` | number path, `count += 2` | 5 | `a = 5` |
| `counter()` | non-number, `temp = count(5)` snapshot | 5 | (inner fn returned) |
| `(...)(true)` | `reset=true` → `count = start = 3` | 3 | `d = temp(5) + count(3) = 8` |
| `a + d` | `5 + 8` | - | **13** |

---

## Final Answer
**13**

---

## Why Other Options are Wrong?
### Option A (5)
Ye sirf `a` ki value hai, `d` ko include nahi kiya gaya — poora sum nahi hai.

### Option C (8)
Ye sirf `d` ki value hai, `a` ko add nahi kiya — poora sum nahi hai.

### Option D (11)
Ye galat calculation ka result hai — shayad koi `reset` logic ko galat samajh kar (jaise `count++` wala path le liya bina `temp` capture properly kiye) is value pe pahuchega — lekin sahi trace 13 deta hai.

---

## Important Exam Notes
- ✅ Nested closures me **track karo variable ki value step-by-step**, table bana kar confusion se bacho.
- ✅ IIFE turant execute hota hai, `temp` ki value us waqt "freeze" ho jaati hai.
- ✅ Default parameters (`reset = false`) sirf tab apply hote hain jab argument diya hi na jaaye.
- ⚠️ Common Mistake: `temp` ko galat point pe capture samajhna, ya `count` reset hone ke baad bhi purani value use kar lena.
- 💡 Trick: Har function call ke baad ek chhota "state snapshot" likh lo (count = ?, temp = ?) — confusion nahi hoga.

---

## Similar Question Pattern
Multi-level nested closures + conditional branches + IIFEs ke complex trace-output questions high-mark (4-5 marks) questions me common hain — inme patience aur step-by-step tracing zaroori hai.

---

## Revision Box
`counter(2)` → number path → `count` 3→5, `a=5`. `counter()` → non-number path → `temp` captures `count(5)`. `(true)` call → `reset=true` → `count` resets to `start(3)` → returns `temp(5)+count(3)=8`, `d=8`. Final: `a+d = 5+8 = 13`.

---
---

# Question 15

## Original Question
```js
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
    super(type);
    this.brand = brand;
  }

  info(brand = "type") {
    return `${this.brand} is a ${this.type} device`;
  }
}

let d = new Device("Electronic");
let m = new Mobile("Electronic", "Samsung");

let x = m.info;
```
> Which of the following statements are TRUE?
>
> Options:
> A. `d.info()` returns "Electronic device"
> B. `m.info()` returns "Samsung is a Electronic device"
> C. Calling `x()` throws a runtime error
> D. `d.constructor()` returns "Electronic device"

(Multiple Select Question, Correct Marks: 4)

---

## Correct Answer
**Correct Options:** A, B, and C

---

## Concept Used
- 📘 **Class Inheritance (`extends`, `super`):** `Mobile` class `Device` ko extend karti hai — matlab `Mobile` ki instances ke pass `Device` ke saare properties/methods bhi available hote hain (jab tak `Mobile` unhe override na kare). `super(type)` parent class ka constructor call karta hai.
- 📘 **Method Overriding:** `Mobile` class ne apna `info()` method define kiya hai jo `Device` ke `info()` ko **override** karta hai — matlab `Mobile` instance pe `info()` call hone se `Mobile` wala version chalega, `Device` wala nahi.
- 📘 **Default Parameters vs Instance Properties — IMPORTANT TRAP:** `Mobile` ke `info(brand = "type")` me `brand` ek **local parameter** hai (default value `"type"` string ke saath), lekin function ke body me `this.brand` use ho raha hai — jo **instance property** hai, parameter `brand` se bilkul alag cheez! Ye do alag variables hain jinka naam same hai.
- 📘 **Class Methods & `this` binding:** Class ke andar defined methods (jaise `info`) **strict mode** me hote hain by default. Agar unhe **plain function ki tarah call** kiya jaaye (bina object ke, jaise `x()`), to unke andar `this` = `undefined` hoga (strict mode rule), jo `this.brand`/`this.type` access karne pe **TypeError** throw karega.
- 📘 **`constructor` property:** Har object ka ek `constructor` property hota hai jo us class/function ko point karta hai jisne use bana ya. Class constructors ko `new` ke bina call karna JavaScript me **invalid** hai — TypeError throw hota hai: "Class constructor cannot be invoked without 'new'".

---

## Step-by-Step Solution
1. **Statement A: `d.info()` returns "Electronic device"**
   - `d` `Device` class ka instance hai, `d.type = "Electronic"`.
   - `d.info()` → `Device` class ka `info()` method chalega (kyunki `d` `Mobile` ka instance nahi hai) → `` `${this.type} device` `` → `"Electronic device"`.
   - **TRUE** ✅
   - *Reason:* `d` sirf `Device` class ka object hai, isme koi override nahi hai.

2. **Statement B: `m.info()` returns "Samsung is a Electronic device"**
   - `m` `Mobile` class ka instance hai — constructor me `this.type = "Electronic"` aur `this.brand = "Samsung"` set hue (via `super(type)` aur `this.brand = brand`).
   - `m.info()` call hota hai **bina argument ke** — isliye local parameter `brand` apni **default value** `"type"` legi (ye string literal "type" hai, koi variable nahi!).
   - **IMPORTANT:** Function body me `this.brand` use ho raha hai, local parameter `brand` **nahi** use ho raha (dhyan do — dono alag hain, `this.brand` object property hai, `brand` sirf parameter hai jo function ke andar defined hai but istemal nahi ho raha body me)!
   - `this.brand` = `"Samsung"` (instance property se), `this.type` = `"Electronic"` (instance property se).
   - Result: `` `${this.brand} is a ${this.type} device` `` → `"Samsung is a Electronic device"`.
   - **TRUE** ✅
   - *Reason:* Ye ek classic trap hai — parameter `brand` ka naam instance property `this.brand` se match karta hai, lekin body me `this.brand` explicitly use hone ki wajah se parameter ka koi role nahi hai final output me.

3. **Statement C: Calling `x()` throws a runtime error**
   - `let x = m.info;` — Ye `info` method ka **reference** `x` me store kar raha hai, bina call kiye.
   - Jab `x()` **plain function ki tarah** call hota hai (na ki `m.info()` ki tarah, jaha `m` ke through call hota), to `this` **binding lost** ho jaati hai.
   - Class methods strict mode me hote hain, isliye plain call pe `this = undefined`.
   - Function body `this.brand` access karne ki koshish karega → `undefined.brand` → **TypeError: Cannot read properties of undefined**.
   - **TRUE** ✅
   - *Reason:* Method ko object se "detach" karke call karne se `this` binding toot jaati hai — ye JavaScript ka bahut common gotcha hai.

4. **Statement D: `d.constructor()` returns "Electronic device"**
   - `d.constructor` `Device` class (constructor function) ko refer karta hai.
   - Class constructors ko **`new` keyword ke bina call karna invalid** hai — JavaScript engine turant **TypeError** throw karega: "Class constructor Device cannot be invoked without 'new'".
   - Isliye ye statement kabhi "Electronic device" return **nahi** karega — ye error throw karega.
   - **FALSE** ❌
   - *Reason:* Classes explicitly design ki gayi hain taaki unhe `new` ke bina call na kiya ja sake (functions ke opposite, jo bina `new` ke bhi call ho sakte hain).

---

## Final Answer
**A, B, and C are TRUE; D is FALSE.**

---

## Why Other Options are Wrong?
### Option D
`d.constructor()` galat hai kyunki class constructor ko `new` ke bina call karna JavaScript specification ke against hai — ye "Electronic device" return karne ki jagah seedha **TypeError** throw karega.

---

## Important Exam Notes
- ✅ Parameter aur instance property same naam ke ho sakte hain, lekin wo **alag variables** hain — body me jo explicitly use ho raha hai wahi count hota hai.
- ✅ Class methods ko object se "detach" karke (plain function ki tarah) call karne se `this` binding toot jaati hai → error.
- ✅ Class constructor `new` ke bina call nahi ho sakta — TypeError aata hai.
- ⚠️ Common Mistake: Parameter default value (`brand = "type"`) ko hi answer me use kar lena, jabki body `this.brand` use kar raha hai.
- 💡 Trick: Jab bhi method ka reference nikaal kar (`let x = obj.method`) alag se call ho, turant socho — "this binding lost ho gayi hogi!"

---

## Similar Question Pattern
Class inheritance, method overriding, `this` binding issues (detached methods), aur constructor invocation rules — ye sab topics high-mark MSQ questions me combine karke puche jaate hain.

---

## Revision Box
`d.info()` = "Electronic device" (base class). `m.info()` = "Samsung is a Electronic device" (body me `this.brand`/`this.type` use hota hai, parameter default value irrelevant hai). `x()` (detached call) = TypeError (this binding lost). `d.constructor()` = TypeError (class constructor needs `new`).

---
---

# Question 16

## Original Question
**File: index.html**
```html
<div id="app">
  <p id="text">{{ message }}</p>
</div>
```

**File: script.js**
```js
new Vue({
  el: "#app",
  data: {
    message: "Start"
  },
  beforeMount() {
    this.message = this.message + " A";
    console.log("beforeMount message:", this.message);
  },
  mounted() {
    const el = document.getElementById("text");
    if (el) {
      el.innerText = el.innerText + " DOM";
      console.log("DOM text after manipulation:", el.innerText);
    }
  }
});
```
> When the `beforeMount()` lifecycle hook is invoked, which of the following statements becomes TRUE?
>
> Options:
> A. The `<p id="text">` element exists in the DOM but still contains `{{ message }}`
> B. The DOM content already reflects the updated value "Start A"
> C. The `mounted()` hook has already executed
> D. The DOM does not exist at all

---

## Correct Answer
**Correct Option:** A

---

## Concept Used
- 📘 **Vue Lifecycle Hooks:** Vue component ki life me alag-alag stages hoti hain, aur har stage pe ek specific "hook" (function) call hota hai:
  1. `beforeCreate` → data/events initialize hone se pehle
  2. `created` → data reactive ho chuka, lekin DOM nahi bana
  3. `beforeMount` → template **compile** ho chuka hai (virtual DOM ready), lekin **real DOM me insert nahi hua**
  4. `mounted` → component real DOM me insert ho chuka hai, ab DOM manipulation possible hai
- 📘 **`beforeMount` ka matlab hai:** "Mount hone se **pehle**". Iska matlab hai ki Vue ne abhi tak apna **compiled template ko actual visible DOM me render nahi kiya** hai. HTML page ka original markup (jisme `{{ message }}` mustache syntax likha hai) abhi bhi as-it-is hai DOM me — Vue ne use interpolate/replace nahi kiya.
- 📘 **Important Distinction:** `el: "#app"` wala element (jo humne HTML me likha) **already DOM me exist karta hai** page load hote hi (browser HTML parse karke DOM banata hai) — lekin uske andar ka content (`{{ message }}`) tab tak "raw/uncompiled" rehta hai jab tak Vue `mount()` process complete na kare.

---

## Step-by-Step Solution
1. **Step 1:** Samjho HTML page load hote hi browser `<div id="app"><p id="text">{{ message }}</p></div>` ko parse kar ke DOM tree bana deta hai — **ye Vue se independent hai**, plain HTML parsing hai.
   - *Reason:* Browser HTML ko turant parse karta hai, Vue baad me is DOM pe "control" leta hai.
2. **Step 2:** Jab Vue instance banti hai aur `beforeMount()` hook call hota hai, iska matlab hai Vue ne apna internal template **compile** kar liya hai (JS memory me), lekin abhi tak is compiled template ko actual visible DOM me **replace/render** nahi kiya.
   - *Reason:* "beforeMount" naam se hi clear hai — ye mounting (DOM me insert karne) se **pehle** ka stage hai.
3. **Step 3:** Isliye is waqt `<p id="text">` element **DOM me already maujood hai** (browser ne parse kar diya tha), lekin uske andar text abhi bhi raw `{{ message }}` hi hai — kyunki Vue ne abhi tak interpolation (mustache ko actual value se replace karna) nahi kiya.
   - *Reason:* Interpolation (mustache replace) sirf **mounting process ke dauraan** hoti hai, jo `beforeMount` ke **baad** hota hai.
4. **Step 4:** Isliye Option A sahi hai: "The `<p id="text">` element exists in the DOM but still contains `{{ message }}`".

---

## Final Answer
**"The `<p id="text">` element exists in the DOM but still contains `{{ message }}`"**

---

## Why Other Options are Wrong?
### Option B
Galat hai — `beforeMount` stage pe DOM abhi bhi **updated value nahi dikhata**, kyunki interpolation abhi hui hi nahi hai. "Start A" (jo `beforeMount` ke andar hi set hua tha `this.message` pe) sirf JS **data model** me update hua hai, DOM me abhi tak show nahi ho raha.

### Option C
Galat hai — `mounted()` hook **hamesha `beforeMount()` ke baad** chalta hai, kabhi pehle nahi. Vue lifecycle ka fixed order hai.

### Option D
Galat hai — DOM element already exist karta hai (browser ne HTML parse karke bana diya hai), sirf uska content Vue se update nahi hua hai abhi.

---

## Important Exam Notes
- ✅ Lifecycle Order: `beforeCreate → created → beforeMount → mounted → beforeUpdate → updated → beforeDestroy → destroyed`
- ✅ `beforeMount`: template compiled but not yet rendered to real DOM.
- ✅ `mounted`: DOM fully updated, safe to do direct DOM manipulation.
- ⚠️ Common Mistake: Sochna ki `beforeMount` ka matlab hai DOM element exist hi nahi karta — actually raw HTML element already DOM me hota hai, sirf uska **content interpolate nahi hua** hota.
- 💡 Trick: "**before**Mount = element exists, content still raw"

---

## Similar Question Pattern
Vue/React lifecycle hooks ke order aur unme DOM ki state (kya update hui, kya nahi) puchne wale conceptual + code-based questions common hain.

---

## Revision Box
`beforeMount` ka matlab hai template compile ho chuka lekin DOM me interpolate nahi hua. DOM element already exist karta hai (raw HTML se), lekin uske andar abhi bhi `{{ message }}` hi dikhega, updated value nahi.

---
---

# Question 17

## Original Question
(Same code as Question 16 — comprehension based)

> What will be the final text content displayed in the browser inside the `<p>` element after the component is fully mounted and all statements in the `mounted()` hook have executed?
>
> Options:
> A. "Start A"
> B. "Start A B"
> C. "Start A B DOM"
> D. "Start A DOM"

---

## Correct Answer
**Correct Option:** D ("Start A DOM")

---

## Concept Used
- 📘 **Order of Lifecycle Hook Execution:** `beforeMount()` hamesha `mounted()` se **pehle** chalta hai. Jo bhi data changes `beforeMount()` me hote hain, wo **mounting process ke dauraan** hi DOM me reflect ho jaate hain (kyunki interpolation `mounted` se pehle ho chuki hoti hai).
- 📘 **Reactive Data Update vs Direct DOM Manipulation:** Do alag tarike se content change ho raha hai is code me:
  1. `this.message = this.message + " A"` — Ye Vue ke **reactive data model** ko update kar raha hai (`beforeMount` ke andar). Jab template render/mount hoga, ye updated value (`"Start A"`) hi DOM me dikhegi.
  2. `el.innerText = el.innerText + " DOM"` — Ye **direct DOM manipulation** hai (`mounted` hook ke andar), jo already-rendered DOM text ke upar seedha string append kar raha hai, Vue ke reactive system se bahar ja kar.
- 📘 Ye dono updates **sequentially** apply hote hain, isliye final result dono ka combination hoga.

---

## Step-by-Step Solution
1. **Step 1:** Initial `data.message = "Start"`.
2. **Step 2:** `beforeMount()` chalta hai (mounting se pehle) → `this.message = "Start" + " A" = "Start A"`. Ab Vue ke data model me `message = "Start A"` hai.
   - *Reason:* Ye reactive data ko update karta hai, DOM ko abhi tak nahi (jaisa Question 16 me discuss kiya).
3. **Step 3:** Ab **mounting process** hota hai — Vue apna template `<p id="text">{{ message }}</p>` ko actual DOM me render karta hai, is waqt jo bhi `message` ki latest value hai (`"Start A"`), wahi DOM me interpolate hoti hai.
   - *Reason:* Mounting ke time Vue current data state use karta hai render karne ke liye — `beforeMount` me hui update (`"Start A"`) already reflect ho chuki hoti hai.
   - Ab DOM me: `<p id="text">Start A</p>`
4. **Step 4:** Component fully mount ho jaata hai, ab `mounted()` hook chalta hai.
   - `const el = document.getElementById("text");` — element ko DOM se fetch karta hai (jisme abhi text "Start A" hai).
   - `el.innerText = el.innerText + " DOM";` → `el.innerText = "Start A" + " DOM" = "Start A DOM"`.
   - *Reason:* Ye **direct DOM manipulation** hai — jo bhi text is waqt DOM me tha (`"Start A"`), uske aage `" DOM"` string append ho gayi.
5. **Step 5:** Final displayed text = **"Start A DOM"**.

**Shortcut:** Yaad rakho — `beforeMount` ka change **automatically render ho jaata hai** mount ke time (kyunki wo Vue data model use karta hai), aur `mounted` ka change **manually/directly** DOM pe apply hota hai (extra step).

---

## Final Answer
**"Start A DOM"**

---

## Why Other Options are Wrong?
### Option A ("Start A")
Galat hai — ye sirf `beforeMount` ke baad ki state hai, `mounted()` hook ka DOM manipulation (`+ " DOM"`) yaha include nahi kiya gaya.

### Option B ("Start A B")
Galat hai — code me kahi bhi `" B"` add nahi ho raha, ye ek fabricated/distractor value hai jo confusion create karne ke liye di gayi hai.

### Option C ("Start A B DOM")
Galat hai — same reason, `" B"` wala part kahi bhi code me exist nahi karta.

---

## Important Exam Notes
- ✅ `beforeMount` ke reactive data changes automatically final render me shamil ho jaate hain.
- ✅ `mounted` ke andar direct DOM manipulation (`innerText`, `innerHTML`) us waqt ke already-rendered content ke upar apply hota hai.
- ⚠️ Common Mistake: Sochna ki `beforeMount` ka change DOM me turant dikh jaata hai (galat — wo sirf data model update karta hai, jo baad me mount ke time render hota hai).
- 💡 Trick: Track karo — "Reactive update" (data model) vs "Direct DOM update" (manual) — dono alag mechanism hain but sequentially combine hote hain final output me.

---

## Similar Question Pattern
Vue lifecycle hooks ke combination (data update in one hook + DOM manipulation in another hook) ke sequential trace-output questions comprehension format me common hain.

---

## Revision Box
`beforeMount`: `message` = "Start" → "Start A" (data model update, DOM me tab tak nahi dikhta). Mount hone par DOM me "Start A" render hota hai. `mounted`: DOM directly manipulate hoke "Start A" + " DOM" = **"Start A DOM"** final result banta hai.

---
---

# 🎯 Overall Quick Revision Summary

| Q.No | Topic | Key Concept |
|---|---|---|
| 1 | Exam Instructions | Subject confirmation (0 marks) |
| 2 | Scope (var/let/const) | Function vs Block scope, TDZ |
| 3 | Functions | Functions are objects, hoisting rules |
| 4 | `this` binding | `.apply()` explicitly sets `this` |
| 5 | localStorage | Persists across refresh, unlike sessionStorage |
| 6 | Storage Types | Session vs Token vs Cookie storage |
| 7 | Vue Computed + setInterval | `.pop()` order + cumulative reactive updates |
| 8 | Prototype Chain | Destructuring also checks `__proto__` |
| 9 | Variable Shadowing | Function parameter shadows outer variable |
| 10 | UI State | Ephemeral = short-lived frontend elements |
| 11 | HTTP Statelessness | State managed via explicit client-server exchange |
| 12 | Closures | Each function call = independent closure |
| 13 | Vue Slots | Named/default slots, fallback content override |
| 14 | Nested Closures + IIFE | Multi-level state tracking with `temp` snapshot |
| 15 | Class Inheritance | Method overriding, `this` binding loss, constructor rules |
| 16 | Vue Lifecycle (`beforeMount`) | Template compiled but not rendered to DOM yet |
| 17 | Vue Lifecycle (`mounted`) | Reactive update + direct DOM manipulation combine |

---
**📌 Note:** Ye notes exam revision ke liye complete hain — har question ek mini-chapter ki tarah explain kiya gaya hai taaki original book/solution dekhne ki zaroorat na pade. 🎓
