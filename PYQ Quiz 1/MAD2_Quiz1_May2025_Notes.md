# MAD 2 — Quiz 1 (May 2025) PYQ Detailed Solutions

---

# Question 112

## Original Question
> In Vue 2, what happens when you update a data property?
>
> Options:
> A. DOM is immediately updated without virtual DOM
> B. The page reloads
> C. The virtual DOM detects changes and updates efficiently
> D. A full component re-renders from scratch

---

## Correct Answer
**Correct Option:** C (The virtual DOM detects changes and updates efficiently)

---

## Concept Used
- 📘 **Virtual DOM:** Ye ek **lightweight JavaScript representation** hoti hai actual DOM ki — matlab ek "copy" jo memory me rehti hai, real browser DOM se bahut **fast** hoti hai manipulate karne ke liye.
- 📘 **Reactivity System (Vue 2):** Jab bhi koi `data` property update hoti hai, Vue us change ko **detect** karta hai (Vue 2 internally `Object.defineProperty()` ke through getters/setters use karta hai reactivity ke liye).
- 📘 **Virtual DOM Diffing Process:**
  1. Data change hoti hai.
  2. Vue **naya virtual DOM tree** banata hai (updated state ke sath).
  3. Vue **purane aur naye virtual DOM trees ko compare** karta hai — is process ko "**diffing**" kehte hain.
  4. Sirf wahi **minimal changes** jo actually zaroori hain, wahi **real DOM** me apply hoti hain (patch karta hai).
- 📘 **Intuition:** Real DOM ko baar-baar directly manipulate karna **slow/expensive** hota hai. Virtual DOM approach se Vue sirf **necessary updates** hi real DOM pe karta hai, poora page reload ya poora component re-render nahi karta — isse performance **efficient** rehti hai.

**Example:** Agar ek list me sirf 1 item ka text change ho, to Virtual DOM diffing se Vue **sirf us ek item ka DOM node** update karega, poori list ko dobara render nahi karega.

---

## Step-by-Step Solution
1. **Step 1:** Vue 2 me `data` object ki properties reactive hoti hain — jab unki value change hoti hai, Vue turant is change ko **track** kar leta hai (internal getter/setter mechanism se).
   - *Reason:* Reactivity system ka poora base hi ye hai ki data changes automatically detect ho jaayein.
2. **Step 2:** Change detect hone ke baad, Vue **poora page reload nahi karta**, na hi seedha real DOM ko directly manipulate karta hai — balki pehle ek **naya Virtual DOM tree** banata hai.
   - *Reason:* Virtual DOM ek "**staging area**" ki tarah kaam karta hai, jaha changes safely calculate kiye ja sakein bina real DOM ko baar-baar touch kiye.
3. **Step 3:** Vue purane aur naye Virtual DOM trees ko **compare (diff)** karta hai, taaki pata chale **exactly kya-kya change hua hai**.
4. **Step 4:** Sirf wahi minimal, necessary changes **real DOM** me **patch** kiye jaate hain — isse rendering **efficient** hoti hai.

**Shortcut:** Yaad rakho — "**Virtual DOM = Smart Middleman**" jo real DOM ko unnecessary updates se bachata hai.

---

## Final Answer
**"The virtual DOM detects changes and updates efficiently"**

---

## Why Other Options are Wrong?
### Option A (DOM immediately updated without virtual DOM)
Galat hai — Vue **specifically Virtual DOM use karta hai** performance ke liye. Direct/immediate DOM manipulation (bina virtual DOM ke) slow hoti agar bar-bar ki jaaye, isliye Vue is approach ko avoid karta hai.

### Option B (page reloads)
Galat hai — Vue jaise frameworks ka poora purpose hi ye hai ki **SPA (Single Page Application)** banaye jaha page reload ki zaroorat na pade. Data update hone se page kabhi reload nahi hota.

### Option D (full component re-renders from scratch)
Galat hai — Ye ek common misconception hai. Vue **poora component dobara se banata nahi hai** — Virtual DOM diffing ki wajah se sirf wahi **specific parts** update hote hain jo actually change hue hain, poora "from scratch" re-render nahi hota.

---

## Important Exam Notes
- ✅ Virtual DOM = lightweight JS representation of real DOM.
- ✅ Diffing = purane aur naye Virtual DOM trees ka comparison.
- ✅ Sirf necessary/minimal changes hi real DOM me apply hoti hain (patching).
- ⚠️ Common Mistake: "Full re-render" aur "efficient patching" ko confuse karna.
- 💡 Trick: "Virtual DOM = Change Detector + Efficient Updater"

---

## Similar Question Pattern
Vue/React ke internal working (Virtual DOM, reactivity, diffing algorithm) ke conceptual MCQs common hain — inme "kaise" aur "kyu" dono samajhna zaroori hai.

---

## Revision Box
Vue 2 data property update hone par: naya Virtual DOM tree banta hai → purane se compare (diff) hota hai → sirf necessary changes real DOM me patch hote hain. Ye poora process **efficient** hota hai — na page reload hota hai, na poora component re-render.

---
---

# Question 113

## Original Question
**HTML:**
```html
<div id="app">
    <h3>Shopping Cart</h3>
    <p>Items: {{itemCount}}</p>
    <p>Total Cost: ${{totalCost}}</p>
    <button @click="addItem">Add Item ($15)</button>
    <button @click="removeItem">Remove Item</button>
    <p>Status: {{status}}</p>
</div>
```
**Script:**
```js
const app = new Vue({
    el: '#app',
    data() {
        return {
            itemCount: 0,
            itemPrice: 15,
            totalCost: 0
        }
    },
    computed: {
        status() {
            if (this.itemCount === 0) {
                return 'Empty Cart';
            } else if (this.itemCount < 3) {
                return 'Few Items';
            } else {
                return 'Full Cart';
            }
        }
    },
    methods: {
        addItem() {
            this.itemCount++;
            this.totalCost += this.itemPrice;
        },
        removeItem() {
            if (this.itemCount > 0) {
                this.itemCount--;
                this.totalCost -= this.itemPrice;
            }
        }
    }
})
```
> If the user clicks the "Add Item" button 3 times, then clicks the "Remove Item" button 1 time, what will be displayed for Items, Total Cost, and Status?
>
> Options:
> A. Items: 2, Total Cost: $30, Status: Few Items
> B. Items: 2, Total Cost: $45, Status: Few Items
> C. Items: 3, Total Cost: $30, Status: Full Cart
> D. Items: 2, Total Cost: $30, Status: Full Cart
> E. Items: 1, Total Cost: $15, Status: Few Items

---

## Correct Answer
**Correct Option:** A (Items: 2, Total Cost: $30, Status: Few Items)

---

## Concept Used
- 📘 **Vue Methods:** `methods` object me define kiye gaye functions **user actions** (jaise button clicks) handle karte hain — inhe explicitly call karna padta hai (jaise `@click="addItem"`).
- 📘 **Vue Computed Properties:** `computed` properties **automatically recalculate** hoti hain jab bhi unke andar use hone wale reactive data properties (`itemCount`) change hote hain. Inhe function ki tarah call nahi karna padta — Vue khud detect karta hai kab recalculate karna hai.
- 📘 **Conditional Logic (if-else if-else):** `status` computed property `itemCount` ki value ke hisaab se teen alag states return karta hai — "Empty Cart" (0 items), "Few Items" (1 ya 2 items), "Full Cart" (3 ya zyada items).

---

## Step-by-Step Solution
1. **Step 1:** Initial state: `itemCount = 0`, `totalCost = 0`.
2. **Step 2: "Add Item" 1st click** — `addItem()` chalta hai:
   - `itemCount++` → `0 → 1`
   - `totalCost += itemPrice(15)` → `0 + 15 = 15`
3. **Step 3: "Add Item" 2nd click:**
   - `itemCount++` → `1 → 2`
   - `totalCost += 15` → `15 + 15 = 30`
4. **Step 4: "Add Item" 3rd click:**
   - `itemCount++` → `2 → 3`
   - `totalCost += 15` → `30 + 15 = 45`
   - *Reason:* Teen baar "Add Item" click hua hai, isliye teen baar `addItem()` chala.
5. **Step 5: "Remove Item" 1 click** — `removeItem()` chalta hai:
   - Condition check: `this.itemCount > 0` → `3 > 0` → **true**, isliye andar ka code chalega.
   - `itemCount--` → `3 → 2`
   - `totalCost -= 15` → `45 - 15 = 30`
   - *Reason:* Ek baar hi "Remove Item" click hua hai.
6. **Step 6: Final state check:** `itemCount = 2`, `totalCost = 30`.
7. **Step 7: `status` computed property calculate karo (current `itemCount = 2` ke sath):**
   - `this.itemCount === 0` → `2 === 0` → **false**
   - `this.itemCount < 3` → `2 < 3` → **true** → return `'Few Items'`
   - *Reason:* `itemCount = 2` hai, jo `0` nahi hai (Empty Cart nahi) lekin `3` se kam hai (Full Cart bhi nahi) — isliye "Few Items" category me aata hai.

**Shortcut:** Aise sequential button-click questions me **ek table bana lo** — har click ke baad values track karo, confusion nahi hoga.

| Action | itemCount | totalCost |
|---|---|---|
| Initial | 0 | 0 |
| Add (1st) | 1 | 15 |
| Add (2nd) | 2 | 30 |
| Add (3rd) | 3 | 45 |
| Remove (1st) | 2 | 30 |

---

## Final Answer
**"Items: 2, Total Cost: $30, Status: Few Items"**

---

## Why Other Options are Wrong?
### Option B (Items: 2, Total Cost: $45)
Galat hai — `totalCost` galat calculate hui hai. "Remove Item" click ke baad `totalCost` bhi **kam** hona chahiye tha (`45 - 15 = 30`), lekin is option me remove hone ke baad bhi `totalCost` 45 hi reh gayi — jaise remove operation cost update na kar raha ho.

### Option C (Items: 3, Total Cost: $30, Status: Full Cart)
Galat hai — `itemCount = 3` galat hai. "Remove Item" click hone ke baad `itemCount` **3 se 2** hona chahiye tha, is option me `itemCount` update hi nahi hua.

### Option D (Items: 2, Total Cost: $30, Status: Full Cart)
Items aur Total Cost sahi hain, lekin **Status galat** hai. `itemCount = 2` hai, jo `< 3` condition satisfy karta hai, isliye status **"Few Items"** hona chahiye, "Full Cart" nahi (Full Cart tab hota jab `itemCount >= 3` ho).

### Option E (Items: 1, Total Cost: $15)
Galat hai — Ye values sirf **1 baar Add** karne ke baad ki hain, poore sequence (3 Add + 1 Remove) ko consider nahi kiya gaya.

---

## Important Exam Notes
- ✅ `methods` = explicit function calls (button clicks se trigger hote hain).
- ✅ `computed` = automatically recalculate hoti hain based on dependencies, explicitly call nahi karna padta.
- ⚠️ Common Mistake: Sequential operations (multiple clicks) ko track karte waqt koi ek step miss kar dena.
- 💡 Trick: Har click ke baad state ek table me likho — values track karna aasan ho jaata hai.

---

## Similar Question Pattern
Vue methods + computed properties ke combination wale multi-step state-tracking questions bahut common hain — dhyan rakhna hai sequence of actions ko sahi order me follow karna.

---

## Revision Box
3 "Add Item" clicks: itemCount 0→1→2→3, totalCost 0→15→30→45. 1 "Remove Item" click: itemCount 3→2, totalCost 45→30. Final: itemCount=2 (< 3, not 0) → Status = **"Few Items"**. Answer: Items: 2, Total Cost: $30, Status: Few Items.

---
---

# Question 114

## Original Question
```js
const arr = [1, 2, 3];
const result = arr.map(function(item, index) {
  return this[index] * item;
}, [10, 20, 30]);
console.log(result);
```
> What will be the output?
>
> Options: A. `[1, 2, 3]`  B. `[10, 40, 90]`  C. `[10, 20, 30]`  D. `[NaN, NaN, NaN]`

---

## Correct Answer
**Correct Option:** B (`[10, 40, 90]`)

---

## Concept Used
- 📘 **`.map()` ka Second Argument (thisArg):** `.map()` method ka ek **lesser-known feature** hai — iska **second parameter** (callback function ke baad) `thisArg` hota hai, jo callback function ke andar `this` ki value **explicitly set** kar deta hai.
  - Syntax: `array.map(callbackFunction, thisArg)`
- 📘 **`this[index]`:** Kyunki `thisArg` array `[10, 20, 30]` pass kiya gaya hai, is callback function ke andar `this` = `[10, 20, 30]`. Isliye `this[index]` matlab hai `[10, 20, 30][index]`.
- 📘 **Important — Regular Function Required:** Ye `thisArg` feature **sirf regular functions** (`function(){}`) ke sath kaam karta hai, kyunki unka `this` **dynamically bindable** hota hai. **Arrow functions** ke sath ye kaam **nahi** karega, kyunki arrow functions ka apna `this` hota hi nahi — wo lexical scope se `this` lete hain, `thisArg` parameter ko **ignore** kar dete hain.

**Example:**
```js
[1, 2].map(function(item) {
  return this.multiplier * item;
}, { multiplier: 10 }); // [10, 20]
```

---

## Step-by-Step Solution
1. **Step 1:** `arr = [1, 2, 3]` — Original array jispe `.map()` chalega.
2. **Step 2:** `.map(function(item, index) {...}, [10, 20, 30])` — Callback function ke sath **second argument** `[10, 20, 30]` bhi diya gaya hai — ye `thisArg` hai, matlab callback ke andar `this` = `[10, 20, 30]` set ho jaayega.
   - *Reason:* `.map()` ka second parameter specifically `this` binding ke liye reserved hai.
3. **Step 3: Index 0 (item = 1):**
   - `this[index]` → `this[0]` → `[10,20,30][0]` = `10`
   - `this[index] * item` → `10 * 1 = 10`
4. **Step 4: Index 1 (item = 2):**
   - `this[1]` = `20`
   - `20 * 2 = 40`
5. **Step 5: Index 2 (item = 3):**
   - `this[2]` = `30`
   - `30 * 3 = 90`
6. **Step 6:** Final result array: `[10, 40, 90]`.

**Shortcut:** Jab bhi `.map()`, `.forEach()`, `.filter()` jaise methods me **do arguments** dikhe (callback ke baad ek aur value), turant socho — "**ye `thisArg` hai, callback ke andar `this` isi value ko refer karega**" (bashart callback regular function ho, arrow na ho).

---

## Final Answer
**`[10, 40, 90]`**

---

## Why Other Options are Wrong?
### Option A (`[1, 2, 3]`)
Ye galat hai — ye tab hota agar `thisArg` completely ignore kar diya jaata aur sirf `item` return hota (`this[index]` ka koi role na hota).

### Option C (`[10, 20, 30]`)
Ye galat hai — ye sirf `thisArg` array (`[10,20,30]`) ko as-is return kar raha hai, lekin actual calculation `this[index] * item` hai, sirf `this[index]` nahi.

### Option D (`[NaN, NaN, NaN]`)
Ye galat hai — ye tab hota agar `this` `undefined` ho jaata (jaise `thisArg` diya hi na jaata, aur non-strict mode me bhi kabhi `this[index]` `undefined` deta) — lekin yaha explicitly `thisArg` diya gaya hai, isliye `this` properly `[10,20,30]` array ko refer karta hai.

---

## Important Exam Notes
- ✅ `.map(callback, thisArg)` — second argument `this` binding ke liye hai.
- ✅ Ye feature sirf **regular functions** ke sath kaam karta hai, arrow functions ke sath nahi.
- ⚠️ Common Mistake: `.map()` ka second argument bhool jaana ya ignore kar dena — ye ek kam-jaana-jaata but valid JavaScript feature hai.
- 💡 Trick: "**map(fn, thisArg)** — dusra argument = andar ka `this`"

---

## Similar Question Pattern
Array methods (`.map()`, `.forEach()`, `.filter()`, `.some()`, `.every()`) ke second `thisArg` parameter wale trace-output questions occasionally aate hain — ye ek **lesser-known JS feature** test karta hai.

---

## Revision Box
`.map(callback, thisArg)` — second argument callback ke andar `this` set karta hai. Yaha `this = [10,20,30]`. `this[index] * item`: `10*1=10, 20*2=40, 30*3=90`. Result: **`[10, 40, 90]`**.

---
---

# Question 115

## Original Question
```js
data() {
  return {
    items: [1, 2, 3],
    obj: { a: 1 }
  }
}
```
> In Vue 2, which of the following will NOT trigger reactivity?
>
> Options:
> A. `this.items.push(4)`
> B. `this.items[0] = 10`
> C. `this.items = [10, 2, 3]`
> D. `this.obj.a = 2`

---

## Correct Answer
**Correct Option:** B (`this.items[0] = 10`)

---

## Concept Used
- 📘 **Vue 2 Reactivity Caveats (Array Limitations):** Vue 2 apna reactivity system `Object.defineProperty()` use karke banata hai, jo har property pe **getter/setter** attach karta hai. Lekin ye approach **arrays ke direct index assignment** ko detect **nahi** kar pati — ye ek well-known **Vue 2 limitation** hai.
- 📘 **Kaunse Array Operations Reactive HAIN (Vue 2 me):** Vue 2 ne specifically **7 array methods** ko "**patch**" kiya hai taaki wo reactivity trigger karein: `push()`, `pop()`, `shift()`, `unshift()`, `splice()`, `sort()`, `reverse()`.
- 📘 **Kya Reactive NAHI hai:**
  - `this.items[index] = newValue` — **direct index se element set karna** reactivity trigger **nahi** karta (Vue isse detect nahi kar pata).
  - `this.items.length = newLength` — array ki length directly badalna bhi reactive nahi hai.
- 📘 **Poori array reassign karna** (`this.items = [...]`) **reactive hai** kyunki ye poori property (`items`) ko replace kar raha hai, jo already ek reactive property hai (getter/setter attached hai `items` pe khud).
- 📘 **Object properties:** Agar property already `data()` me define ki gayi hai (jaise `obj.a`), to uski value change karna (`this.obj.a = 2`) reactive hai, kyunki Vue ne already us property pe getter/setter laga rakha hai.

---

## Step-by-Step Solution
1. **Option A: `this.items.push(4)`** — Ye Vue ke **patched array methods** me se ek hai. Vue ne `push()` ko specially override kiya hai taaki ye reactivity trigger kare.
   - **Reactive** ✅ (isliye ye answer nahi hai "NOT trigger" question ke liye)
2. **Option B: `this.items[0] = 10`** — Ye **direct index assignment** hai. Vue 2 ka `Object.defineProperty()`-based system array ke **individual indices** pe getters/setters lagane me **capable nahi hai** (performance reasons ki wajah se JavaScript specification-level limitation hai) — isliye ye change **detect nahi hota**.
   - **NOT Reactive** ❌ — Ye hi humara answer hai.
   - *Reason:* Vue 2 arrays ke length aur indices ko directly track nahi kar sakta, sirf specific "patched" methods ke through hi changes detect hote hain.
3. **Option C: `this.items = [10, 2, 3]`** — Ye **poori `items` property ko reassign** kar raha hai. Kyunki `items` khud ek reactive property hai (`data()` me define hui thi, jispe Vue ne getter/setter laga rakha hai), poori property ko naye array se replace karna **reactive hai**.
   - **Reactive** ✅
4. **Option D: `this.obj.a = 2`** — `obj.a` already `data()` me define ki gayi property hai. Vue ne is property pe bhi getter/setter laga rakha hai (nested reactivity), isliye is value ko update karna **reactive hai**.
   - **Reactive** ✅

**Shortcut:** Yaad rakho — "**Array INDEX se change = NOT reactive, Array METHOD se change = reactive**" (jaise push/pop/splice etc.)

---

## Final Answer
**`this.items[0] = 10`** — Ye reactivity trigger **nahi** karega.

---

## Why Other Options are Wrong?
### Option A
`push()` Vue ke **7 patched array methods** me se ek hai — Vue specifically inhe intercept karta hai taaki reactivity trigger ho.

### Option C
Poori property (`items`) ko reassign karna reactive hai kyunki `items` khud ek tracked/reactive data property hai.

### Option D
`obj.a` ek existing reactive property hai (nested object property), Vue 2 nested objects ke properties ko bhi (jab tak wo `data()` me initially defined hon) reactive banata hai.

---

## Important Exam Notes
- ✅ Vue 2 patched array methods (reactive): `push`, `pop`, `shift`, `unshift`, `splice`, `sort`, `reverse`.
- ❌ Vue 2 me reactive NAHI: direct index assignment (`arr[i] = x`), direct `length` change.
- ✅ Poori array/property reassign karna hamesha reactive hai.
- ⚠️ Common Mistake: Sochna ki array ka koi bhi modification automatically reactive hoga — Vue 2 me ye sach nahi hai, specific limitations hain.
- 💡 Trick: Agar Vue 2 me array ke kisi specific index ko update karna ho, to `Vue.set(array, index, value)` ya `splice()` use karo — direct index assignment avoid karo.

---

## Similar Question Pattern
Vue 2 ki reactivity system limitations (especially arrays ke sath) ke conceptual MCQs bahut common hain — Vue 3 (jo Proxy use karta hai) me ye limitations nahi hoti, isliye Vue 2 vs Vue 3 comparison bhi puch sakte hain.

---

## Revision Box
Vue 2 reactivity `Object.defineProperty()` pe based hai — ye array ke **direct index assignment** (`items[0]=10`) ko detect nahi kar pata. Reactive operations: patched methods (`push`, `splice`, etc.), poori array/property reassignment, aur existing object properties ka update. **NOT reactive:** direct index assignment.

---
---

# Question 116

## Original Question
```js
function createFunctions() {
  var funcs = [];
  for (var i = 0; i < 3; i++) {
    funcs.push(function() {
      return i;
    });
  }
  return funcs;
}

const functions = createFunctions();
console.log(functions[0](), functions[1](), functions[2]());
```
> What will be the output of this code?
>
> Options: A. `0, 1, 2`  B. `1, 2, 3`  C. `undefined, undefined, undefined`  D. `3, 3, 3`

---

## Correct Answer
**Correct Option:** D (`3, 3, 3`)

---

## Concept Used
- 📘 **`var` aur Function-Scope Closures (Classic JS Interview Trap):** `var` **function-scoped** hota hai, **block-scoped nahi**. Isliye `for (var i = 0; ...)` loop ke andar `i` ek **hi shared variable** hai — har loop iteration ka apna **alag `i` nahi banta**.
- 📘 **Closures aur Shared Reference:** Jab loop ke andar functions `funcs.push(function(){ return i; })` create hote hain, ye teeno functions **same `i` variable ko reference** karte hain (kyunki `var` ki wajah se sirf **ek hi `i`** exist karta hai poore `createFunctions` function ke scope me).
- 📘 **Loop Khatam Hone Ke Baad `i` ki Value:** Loop `i < 3` condition false hone tak chalta hai — matlab loop **`i = 3`** hone par khatam hota hai (kyunki `i++` last valid iteration `i=2` ke baad `i` ko `3` bana deta hai, phir condition check hoti hai aur false ho jaati hai, loop ruk jaata hai).
- 📘 **Jab bhi functions baad me call hote hain**, wo us **shared `i`** ki **latest/final value** dekhte hain, jo loop khatam hone ke baad `3` hai.

**Contrast with `let`:** Agar `var` ki jagah `let i` use hota, to `let` **block-scoped** hota, aur har loop iteration apna **naya, alag `i`** banata — is case me output `0, 1, 2` hota.

---

## Step-by-Step Solution
1. **Step 1:** `var funcs = [];` — Empty array jisme functions store honge.
2. **Step 2:** `for (var i = 0; i < 3; i++)` — Ye loop `var i` use kar raha hai, jo **function-scoped** hai (poore `createFunctions` function ke andar sirf **ek hi `i`** exist karega, har iteration alag `i` nahi banayega).
   - *Reason:* Ye is puzzle ka **core concept** hai — `var` block-scoping follow nahi karta.
3. **Step 3: Iteration 1 (`i=0`):** `funcs.push(function(){ return i; })` — Ek function `funcs` array me add hota hai jo `i` ko refer karta hai (**reference**, koi "snapshot/copy" nahi).
4. **Step 4: Iteration 2 (`i=1`):** Same tarike se ek aur function add hota hai — ye bhi **wahi same `i`** ko refer karta hai jo pehle function ne kiya tha (kyunki sirf ek hi `i` hai poore scope me).
5. **Step 5: Iteration 3 (`i=2`):** Teesra function bhi add hota hai, wahi `i` ko refer karta huA.
6. **Step 6:** Loop check karta hai `i < 3` — `i` ab `3` ho chuka hai (`i++` ne `2` se `3` bana diya), condition `3 < 3` **false**, loop **ruk jaata hai**.
   - *Reason:* Loop exit hone ke baad bhi `i` variable **exist karta hai** (memory me — kyunki functions closure ke through use ise "yaad" rakhte hain), aur uski final value `3` hai.
7. **Step 7:** `functions = createFunctions();` — `funcs` array return hota hai jisme 3 functions hain, sab **same `i`** (jo ab `3` hai) ko refer karte hain.
8. **Step 8:** `functions[0]()`, `functions[1]()`, `functions[2]()` — teeno functions call hote hain. Har ek `return i;` execute karta hai — aur `i` ki **current/latest value** dekhta hai, jo **`3`** hai (sabke liye same, kyunki sab **same variable** share karte hain).
9. **Step 9:** Output: `3, 3, 3`.

**Shortcut:** Jab bhi `for (var i...)` loop ke andar functions create karke baad me call kiye jaayein, turant yaad karo — "**sab functions ek hi `i` share karte hain, aur loop khatam hone ke baad `i` ki final value sabko milegi**". Agar `let` hota, to har ek apna alag `i` capture karta.

---

## Final Answer
**`3, 3, 3`**

---

## Why Other Options are Wrong?
### Option A (`0, 1, 2`)
Ye tab sahi hota agar loop me `let i` use hota (bina `var` ke) — `let` block-scoped hota hai aur har iteration apna **naya `i`** banata, isliye har function apni "capture ki hui" specific value dekhta.

### Option B (`1, 2, 3`)
Ye galat hai — koi bhi calculation aisi nahi hai code me jo `i` ko **+1** kare return karte waqt. Ye ek arbitrary/distractor value hai.

### Option C (`undefined, undefined, undefined`)
Ye tab hota agar `i` kisi tarah **out of scope** ho jaata ya delete ho jaata function call hone tak — lekin closures ki wajah se `i` **hamesha accessible** rehta hai jab tak function reference exist karta hai, `undefined` nahi hota.

---

## Important Exam Notes
- ✅ `var` = function-scoped, **shared** variable across loop iterations.
- ✅ `let` = block-scoped, **naya variable** har iteration me.
- ✅ Closures **live reference** rakhte hain, "snapshot" nahi — loop khatam hone ke baad ki **final value** hi milegi `var` ke case me.
- ⚠️ Common Mistake: Sochna ki har function "apni waqt ki" `i` value yaad rakhega — ye sirf `let` ke case me sahi hai, `var` ke case me nahi.
- 💡 Trick: "**var in loop = All functions share ONE final value**" — ye ek classic JS interview question hai!

---

## Similar Question Pattern
`var` vs `let` in loops + closures ke trace-output questions **bahut hi common** hain JS interviews aur exams me — ye ek fundamental JS gotcha hai jo baar-baar test hota hai.

---

## Revision Box
`var i` poore function ke liye **ek hi shared variable** banata hai (block-scoped nahi hai `var`). Loop ke andar bane teeno functions **same `i`** ko refer karte hain. Loop khatam hone ke baad `i = 3` (kyunki `i++` ne `2` ko `3` bana diya before condition failed). Sab functions call hone par `i` ki final value **3** dekhte hain. Output: **3, 3, 3**.

---
---

# Question 117

## Original Question
> Which computed property implementation is **INCORRECT**?
>
> Options:
>
> **A.**
> ```js
> computed: {
>   fullName() {
>     return this.firstName + ' ' + this.lastName;
>   }
> }
> ```
>
> **B.**
> ```js
> computed: {
>   fullName: {
>     get() { return this.firstName + ' ' + this.lastName; },
>     set(value) {
>       const names = value.split(' ');
>       this.firstName = names[0];
>       this.lastName = names[1];
>     }
>   }
> }
> ```
>
> **C.**
> ```js
> computed: {
>   async fullName() {
>     return await this.fetchFullName();
>   }
> }
> ```
>
> **D.**
> ```js
> computed: {
>   fullName: function() {
>     return this.firstName + ' ' + this.lastName;
>   }
> }
> ```

---

## Correct Answer
**Correct Option:** C (`async fullName() {...}`) — Ye INCORRECT implementation hai.

---

## Concept Used
- 📘 **Computed Properties:** Vue ke `computed` properties **synchronously** calculate hote hain — matlab jab bhi unhe access kiya jaata hai (jaise `{{ fullName }}`), Vue **turant** unki value **return** expect karta hai, koi "wait" nahi kar sakta.
- 📘 **Computed Properties Ka Purpose:** Ye reactive dependencies (jaise `firstName`, `lastName`) ke basis pe **derived value** synchronously calculate karte hain aur **cache** karte hain — jab tak dependencies change na hon, cached value hi return hoti hai.
- 📘 **Async Functions Kyu Kaam Nahi Karte Computed Me:** Ek `async function` **hamesha ek `Promise` return karti hai** (chahe uske andar `return` statement kuch bhi ho) — kabhi bhi **direct/synchronous value** nahi return karti. Vue ka computed property system `Promise` ko handle karne ke liye **design nahi** kiya gaya — Vue simply is `Promise` object ko **as-is treat karega** (jaise `[object Promise]` template me dikhega), actual resolved value nahi.
- 📘 **Correct Alternative:** Agar async data chahiye ho, to `data` property (jaise `fullName: ''`) use karo aur ek `method` ya `watcher`/`created()` hook me async call karke us data property ko **manually update** karo.

**Example (galat vs sahi tarika async data ke liye):**
```js
// ❌ GALAT (computed me async)
computed: {
  async userData() { return await fetchUser(); } // Promise return hoga, actual data nahi
}

// ✅ SAHI (data + method/created)
data() { return { userData: null }; },
async created() {
  this.userData = await fetchUser(); // yaha reactive data manually update ho raha hai
}
```

---

## Step-by-Step Solution
1. **Option A Check:** `fullName() { return this.firstName + ' ' + this.lastName; }` — Ye standard **method shorthand syntax** hai computed property likhne ka. Ye **synchronously** ek string return karta hai.
   - **CORRECT** ✅
2. **Option B Check:** `fullName: { get() {...}, set(value) {...} }` — Ye **computed property with getter/setter** hai. Iska use tab hota hai jab computed property ko **assign** bhi karna ho (jaise `this.fullName = "John Doe"`), tab `set()` chalta hai jo `firstName`/`lastName` ko update kar deta hai. Ye ek **valid aur advanced** pattern hai.
   - **CORRECT** ✅
3. **Option C Check:** `async fullName() { return await this.fetchFullName(); }` — Ye computed property ko **`async` function** ki tarah define kar raha hai.
   - **Problem:** `async` function **hamesha `Promise` return karti hai**, kabhi bhi resolved/final value directly nahi. Vue computed properties **synchronous evaluation** expect karte hain — is wajah se template me `fullName` access karne par aapko ek **unresolved Promise object** milega, actual computed string nahi.
   - **INCORRECT** ❌ — Ye hi humara answer hai.
4. **Option D Check:** `fullName: function() { return this.firstName + ' ' + this.lastName; }` — Ye bhi valid syntax hai, sirf **traditional function expression** style me likha gaya hai (Option A ka hi equivalent, bas syntax style different hai — ES6 shorthand vs traditional `function` keyword).
   - **CORRECT** ✅

**Shortcut:** Jab bhi `computed` property ke andar `async`/`await` dikhe, turant flag karo — "**Ye INVALID hai, computed properties sirf synchronous value return kar sakte hain!**"

---

## Final Answer
**Option C (`async fullName()`) is INCORRECT** — computed properties async nahi ho sakte.

---

## Why Other Options are Wrong?
Ye "**find the incorrect one**" question hai, isliye baaki options **sahi (valid)** hain:

### Option A
Sahi hai — standard ES6 method shorthand syntax computed property ke liye, synchronous return.

### Option B
Sahi hai — getter/setter pattern computed properties ke liye valid hai, dono directions (read aur write) handle karta hai.

### Option D
Sahi hai — traditional `function` keyword syntax bhi valid hai, functionally Option A jaisa hi hai.

---

## Important Exam Notes
- ✅ Computed properties = **synchronous** hone chahiye, `Promise` return nahi kar sakte.
- ✅ Async data chahiye ho to `data` property + `methods`/`created()`/`watch` use karo, `computed` nahi.
- ✅ Computed properties getter/setter dono support karte hain (advanced pattern).
- ⚠️ Common Mistake: Async operations ko seedha `computed` me daal dena — ye ek bahut common galti hai jo Vue beginners karte hain.
- 💡 Trick: "**Computed = Sync only, Async data = data + method/created**"

---

## Similar Question Pattern
Vue computed properties ke valid/invalid implementations, especially async-related traps, common conceptual MCQs hain — inme dhyan rakhna hai computed properties ka **synchronous nature**.

---

## Revision Box
Computed properties **hamesha synchronous** hone chahiye — `async` function computed me use karna **INCORRECT** hai kyunki wo `Promise` return karti hai, actual value nahi. Getter/setter pattern aur normal function syntax dono valid hain computed properties ke liye.

---
---

# Question 118

## Original Question
```js
const products = [
    { id: 101, name: "laptop", price: 800 },
    { id: 102, name: "mouse", price: 25 },
    { id: 103, name: "keyboard", price: 60 },
    { id: 104, name: "monitor", price: 300 }
];

const priorities = { "102": 1, "104": 2, "101": 3, "103": 4 };

products
    .filter((product) => product.price > 50)
    .map((product) => {
        return { ...product, priority: priorities[product.id] }
    })
    .sort((a, b) => a.priority - b.priority)
    .forEach((product) => {
        console.log(product.name);
    });
```
> What will be the output of the code?
>
> Options:
> A. laptop / keyboard / monitor
> B. monitor / laptop / keyboard
> C. mouse / monitor / laptop / keyboard
> D. laptop / mouse / keyboard / monitor
> E. The code will throw an error

---

## Correct Answer
**Correct Option:** B (monitor / laptop / keyboard)

---

## Concept Used
- 📘 **Method Chaining (`.filter().map().sort().forEach()`):** Ye ek **4-step chain** hai — har method ek naya array return karta hai (except `forEach`, jo kuch return nahi karta, sirf side-effects karta hai jaise `console.log`).
- 📘 **Spread Operator with new property (`{...product, priority: ...}`):** Ye original product object ke saare properties copy karke ek **naya property `priority`** add karta hai.
- 📘 **Object Key Type — String vs Number Trap:** `priorities` object ke keys **strings** hain (`"102"`, `"104"` — quotes me), lekin `product.id` **numbers** hain (`101`, `102`, etc., bina quotes ke). JavaScript me jab hum object ke andar `priorities[product.id]` access karte hain, JS **automatically number ko string me convert** kar deta hai (kyunki object keys hamesha strings ya symbols hoti hain internally) — isliye `priorities[101]` aur `priorities["101"]` **same cheez** access karte hain, koi problem nahi hoti.
- 📘 **`.sort((a,b) => a.priority - b.priority)`:** Ye `priority` field ke basis pe **ascending order** (chhota priority number pehle) sort karta hai.

---

## Step-by-Step Solution
1. **Step 1:** Original `products` array (4 items): laptop(800), mouse(25), keyboard(60), monitor(300).
2. **Step 2: `.filter((product) => product.price > 50)`** — sirf wahi products rakho jinki price **50 se zyada** ho:
   - laptop(800) → `800 > 50` → **true** → included
   - mouse(25) → `25 > 50` → **false** → **excluded**
   - keyboard(60) → `60 > 50` → **true** → included
   - monitor(300) → `300 > 50` → **true** → included
   - Result: `[laptop(101), keyboard(103), monitor(104)]`
   - *Reason:* Mouse ki price sirf 25 hai, jo 50 se kam hai, isliye exclude ho gaya.
3. **Step 3: `.map((product) => ({...product, priority: priorities[product.id]}))`** — har product me `priority` field add karo, `priorities` object se lookup karke:
   - laptop (id=101) → `priorities[101]` = `priorities["101"]` = **3**
   - keyboard (id=103) → `priorities[103]` = `priorities["103"]` = **4**
   - monitor (id=104) → `priorities[104]` = `priorities["104"]` = **2**
   - *Reason:* Number `id` automatically string me convert hota hai jab object property access ki jaati hai — koi error nahi aata.
   - Result: `[{laptop,priority:3}, {keyboard,priority:4}, {monitor,priority:2}]`
4. **Step 4: `.sort((a,b) => a.priority - b.priority)`** — priority ke hisaab se **ascending order** me sort karo (chhota number pehle):
   - Priorities: laptop(3), keyboard(4), monitor(2)
   - Ascending order: monitor(2) → laptop(3) → keyboard(4)
   - Result: `[monitor, laptop, keyboard]`
5. **Step 5: `.forEach((product) => console.log(product.name))`** — har product ka naam print karo, sorted order me:
   - `"monitor"`
   - `"laptop"`
   - `"keyboard"`

**Shortcut:** Priority-based sorting questions me **ek table banao** jisme har item ka **priority number** likho, phir sirf priority ke hisaab se **ascending order** arrange kar do.

| Product | Price | Priority |
|---|---|---|
| laptop | 800 | 3 |
| keyboard | 60 | 4 |
| monitor | 300 | 2 |

Sorted by priority (ascending): **monitor(2) → laptop(3) → keyboard(4)**

---

## Final Answer
**monitor, laptop, keyboard**

---

## Why Other Options are Wrong?
### Option A (laptop / keyboard / monitor)
Ye galat hai — ye **original filtered order** hai (bina sort kiye), sorting apply nahi hui hai is option me.

### Option C (mouse / monitor / laptop / keyboard)
Galat hai — `mouse` ko **filter step me hi exclude** ho jaana chahiye tha (`price=25`, jo `>50` condition fail karta hai), isliye mouse final output me kabhi aa hi nahi sakta.

### Option D (laptop / mouse / keyboard / monitor)
Galat hai — same reason, `mouse` exclude hona chahiye tha, aur order bhi priority ke hisaab se sorted nahi hai.

### Option E (Code will throw an error)
Galat hai — code me koi syntax ya runtime error nahi hai. `priorities[product.id]` access **valid** hai (number automatically string me convert hota hai object key lookup ke liye) — koi error throw nahi hoga.

---

## Important Exam Notes
- ✅ Object keys hamesha **strings** (ya symbols) hoti hain internally — number se access karne pe automatic conversion ho jaata hai, error nahi aata.
- ✅ `.filter()` → `.map()` → `.sort()` → `.forEach()` chaining — har step ka result agle step ka input banta hai.
- ⚠️ Common Mistake: Sochna ki `priorities[101]` (number) aur `priorities["101"]` (string) alag-alag values denge — actually dono **same** hain.
- 💡 Trick: Priority/rank-based sorting questions me hamesha ek **quick table** banao, calculation confusion avoid karne ke liye.

---

## Similar Question Pattern
Multi-step method chaining with lookup objects (jaise priority maps, ID-to-value mappings) ke trace-output questions common hain real-world data-processing scenarios me.

---

## Revision Box
Filter (price>50) → mouse exclude ho jaata hai. Map → priorities lookup se naya field add hota hai (number automatically string-key access karta hai, no error). Sort (ascending priority) → monitor(2) < laptop(3) < keyboard(4). Final output: **monitor, laptop, keyboard**.

---
---

# Question 119

## Original Question
```js
const grandParent = {
  username: 'GrandParent',
  getName: function () {
    return this.username;
  },
  parent: {
    username: 'Parent',
    getName: function () {
      return this.username;
    },
    child: {
      username: 'Child',
      getNameArrow: () => {
        return this.username;
      },
      getNameFunc: function () {
        return this.username;
      }
    }
  }
};

console.log("A: " + grandParent.getName());
console.log("B: " + grandParent.parent.getName());
console.log("C: " + grandParent.parent.child.getNameFunc());
console.log("D: " + grandParent.parent.child.getNameArrow());
```
> What will be the result of all the console.log after execution of the above code?

---

## Correct Answer
**Correct Output:**
```
A: GrandParent
B: Parent
C: Child
D: undefined
```

---

## Concept Used
- 📘 **Regular Function `this` Binding — "Caller decides `this`":** Regular functions (`function(){}`) ka `this` us object pe depend karta hai jispe method **call** kiya gaya (call-site pe decide hota hai). `obj.method()` call hone se `this = obj`.
- 📘 **Nested Object Method Calls:** Har level pe method call **us specific object** ke context me hoti hai — `grandParent.getName()` → `this=grandParent`; `grandParent.parent.getName()` → `this=grandParent.parent` (na ki `grandParent`, kyunki `getName` yaha `parent.` ke through call hui).
- 📘 **Arrow Function `this` — "Definition-site decides `this`":** Arrow functions ka apna `this` nahi hota — wo apne **enclosing/lexical scope** ka `this` "borrow" karte hain, jo unke **defined hone ke waqt** already fix ho chuka hota hai.
- 📘 **IMPORTANT TRAP — `getNameArrow` object literal ke andar directly likha gaya hai:** Chunki `getNameArrow` **kisi function ke andar nahi**, balki directly ek **object literal property** ki tarah define hua hai (top-level object literal ke andar), iska lexical scope **top-level/module scope** hai, **`child` object ka nahi**! Isliye iska `this` = top-level `this` (jo browser script me generally `window`, ya strict/module context me `undefined` hota hai).

---

## Step-by-Step Solution
1. **Line A: `grandParent.getName()`**
   - `getName` **regular function** hai, `grandParent.` ke through call hui, isliye `this = grandParent`.
   - `this.username` → `grandParent.username` = `"GrandParent"`.
   - **Output: `"A: GrandParent"`**

2. **Line B: `grandParent.parent.getName()`**
   - `getName` (parent ka wala, alag function hai grandParent ke `getName` se) **regular function** hai, `grandParent.parent.` ke through call hui, isliye `this = grandParent.parent`.
   - `this.username` → `grandParent.parent.username` = `"Parent"`.
   - **Output: `"B: Parent"`**
   - *Reason:* `this` hamesha **immediate caller object** ko refer karta hai, dot notation ki chain me sabse **nearest/last** object.

3. **Line C: `grandParent.parent.child.getNameFunc()`**
   - `getNameFunc` **regular function** hai, `grandParent.parent.child.` ke through call hui, isliye `this = grandParent.parent.child`.
   - `this.username` → `grandParent.parent.child.username` = `"Child"`.
   - **Output: `"C: Child"`**

4. **Line D: `grandParent.parent.child.getNameArrow()`**
   - `getNameArrow` ek **arrow function** hai. Arrow function ka `this` uske **definition location ke lexical scope** se aata hai, na ki call-site se.
   - `getNameArrow` object literal (`child` ke andar) me **directly** likha gaya hai — ye kisi outer **regular function** ke andar nested nahi hai. Object literals `{}` **naya scope create nahi karte** JavaScript me (sirf functions/blocks scope create karte hain) — isliye `getNameArrow` ka lexical scope actually **top-level/module scope** hai (jaha poora `grandParent` object khud define ho raha hai).
   - Isliye `this` (top-level scope me) = `window` (browser, non-strict) ya `undefined` (strict/module mode).
   - `this.username` → `window.username` (ya `undefined.username` agar strict) — chunki `username` kabhi bhi top-level pe `window`/global object pe **set nahi** hua (koi bhi `var username` ya `window.username = ...` nahi likha gaya code me), isliye ye **`undefined`** hoga.
   - **Output: `"D: undefined"`**
   - *Reason:* Ye ek **classic exam trap** hai — students sochte hain ki arrow function bhi "nearest object" ka `this` lega (jaise regular function karta hai), lekin arrow function **kabhi bhi call-site se `this` nahi leta**, sirf **definition ki jagah** se leta hai.

**Shortcut:** Jab bhi arrow function **object literal ke andar directly** (kisi regular function ke andar nested hue bina) dikhe, turant flag karo — "**Iska `this` top-level/enclosing scope ka hoga, us object ka bilkul nahi jisme ye likha gaya hai!**"

---

## Final Answer
```
A: GrandParent
B: Parent
C: Child
D: undefined
```

---

## Why Other Options are Wrong?
### Option (A: GrandParent, B: Parent, C: Child, D: Reference Error)
Galat hai — `D` "Reference Error" nahi dega. `this.username` access karna **error throw nahi karta** (chahe `this` `undefined` ho strict mode me — `this.username` tab error dega, lekin non-strict/browser script context me `this = window`, aur `window.username` bas `undefined` return karega, error nahi).

### Option (A: GrandParent, B: Parent, C: undefined, D: Child)
Galat hai — `C` aur `D` **swap** ho gaye hain is option me. `getNameFunc` (regular function) sahi tarike se `child.username = "Child"` return karega, aur `getNameArrow` (arrow) `undefined` dega — is option me ulta likha hai.

### Option (A: GrandParent, B: GrandParent, C: Parent, D: GrandParent)
Poori tarah galat hai — `B` ke liye `this` = `grandParent.parent` hona chahiye (immediate caller), na ki `grandParent`. Har method call apne **immediate calling object** ka context leta hai, sabse "top-level" wale ka nahi.

---

## Important Exam Notes
- ✅ Regular function `this` = **call-site** pe decide hota hai (jo bhi object se `.method()` call hua, wahi `this`).
- ✅ Arrow function `this` = **definition-site (lexical scope)** pe decide hota hai, kabhi call-site se nahi.
- ✅ Object literals `{}` apna **naya scope create nahi karte** — sirf functions aur blocks (with `let`/`const`) scope banate hain.
- ⚠️ Common Mistake: Arrow function ko bhi "nearest enclosing object" ka `this` maan lena — sirf tabhi sahi hai jab arrow function **kisi regular function ke andar** nested ho.
- 💡 Trick: "Arrow function ke around **`{ }` ginti mat karo**, dekho **kya ye kisi `function(){}` ke andar hai ya nahi**" — agar nahi, to iska `this` top-level jaayega.

---

## Similar Question Pattern
Deeply nested objects + mix of regular/arrow functions ke `this` binding trace-output questions high-mark questions me common hain — inme patience se **har method ka context (kaha se call hua, kaha define hua)** track karna padta hai.

---

## Revision Box
Regular functions ka `this` = **caller object** (jaise `grandParent.getName()` → `this=grandParent`; `parent.getName()` → `this=parent`; `child.getNameFunc()` → `this=child`). Arrow function (`getNameArrow`), object literal ke andar directly likhi hone ki wajah se, **top-level `this`** leti hai (child ka nahi) — isliye `this.username = undefined`. Final Output: **A: GrandParent, B: Parent, C: Child, D: undefined**.

---
---

# Question 120

## Original Question
```html
<div id="app">
  <my-counter :start="5"></my-counter>
</div>

<script>
  Vue.component('my-counter', {
    props: ['start'],
    data: function () {
      return {
        count: this.start
      };
    },
    template: `
              <div>
                <p>{{ count }}</p>
                <button @click="increment">Increment</button>
              </div>
              `,
    methods: {
      increment() {
        this.count++;
      }
    }
  });

  new Vue({
    el: '#app',
    data: { start: 10 }
  });
</script>
```
> What will be displayed when the page loads, and what happens when you click the button?
>
> Options:
> A. The counter starts at 10, and increments by 1 on each button click.
> B. The counter starts at 5, and increments by 1 on each button click.
> C. Nothing is rendered because the Vue component template is invalid when using Vue via CDN.
> D. Vue throws a warning about compiling templates in the browser and fails to bind the component.

---

## Correct Answer
**Correct Option:** B (The counter starts at 5, and increments by 1 on each button click.)

---

## Concept Used
- 📘 **Props (`props: ['start']`):** Child component (`my-counter`) `start` naam ka ek **prop accept** karta hai — jo bhi value parent isse pass karega, wahi `this.start` ke through child ke andar accessible hogi.
- 📘 **Prop Passing (`:start="5"`):** Parent template me `<my-counter :start="5">` likha gaya hai. `:` (yaani `v-bind:`) ka matlab hai ki `"5"` ko **JavaScript expression** ki tarah **evaluate** kiya jaayega, matlab ye **number `5`** hai, string nahi (agar bina `:` ke `start="5"` likha hota, tab ye string "5" hota).
- 📘 **IMPORTANT — Root Instance ka `data.start` Isse Bilkul Unrelated Hai:** Root Vue instance ka `data: { start: 10 }` — ye `start` sirf **root instance ka apna local data** hai. Child component `<my-counter :start="5">` ko **literal value `5`** de raha hai, root instance ke `start` (jo 10 hai) se **koi automatic connection nahi** hai (agar `:start="start"` likha hota, tab connection hota — root ke `start` data property se bind hota).
- 📘 **`data()` Function me Props Access:** Vue lifecycle me **props pehle se available** hote hain jab tak `data()` function chalta hai, isliye `data() { return { count: this.start } }` me `this.start` already `5` hoga (prop se), aur `count` initial value **5** legi.
- 📘 **`methods.increment()`:** Button click hone par `this.count++` chalta hai — ye simple increment hai, koi twist nahi.

---

## Step-by-Step Solution
1. **Step 1:** Parent template me likha hai `<my-counter :start="5">` — `:start="5"` ka matlab hai `5` (number) **directly hardcoded value** hai jo `my-counter` component ke `start` prop me pass ho rahi hai.
   - *Reason:* `:` (v-bind shorthand) `"5"` ko JS expression ki tarah treat karta hai — ye literal number `5` hai, root instance ke `start` data se koi lena dena nahi.
2. **Step 2:** `my-counter` component ke andar `props: ['start']` define hai — isliye `this.start` (component ke andar) = **5** (jo pass hua hai).
3. **Step 3:** `data: function() { return { count: this.start }; }` chalta hai jab component initialize hota hai. `this.start` us waqt `5` hai (prop already available hai `data()` chalne se pehle), isliye `count = 5`.
   - *Reason:* Vue lifecycle order — props pehle initialize hote hain, phir `data()` chalta hai, isliye `data()` ke andar props access karna **safe** hai.
4. **Step 4:** Page load hote hi template render hota hai — `<p>{{ count }}</p>` → **"5"** dikhega.
5. **Step 5:** User "Increment" button click karta hai — `increment()` method chalta hai:
   - `this.count++` → `5 → 6`.
   - *Reason:` Simple increment logic, koi special behavior nahi.
6. **Step 6:** Har click pe `count` **1-1 badhta jaayega** (5→6→7→...).

**Shortcut:** Jab bhi parent-child components me props pass ho rahe hon, dhyan do — **agar `:propName="literalValue"` (jaise `"5"` sीधे number) diya gaya hai**, to ye **root instance ke kisi bhi data property se automatically link nahi hoga**, chahe naam same ho ya na ho. Sirf `:propName="dataPropertyName"` (bina quotes ke variable reference) likhne se hi connection banta hai.

---

## Final Answer
**"The counter starts at 5, and increments by 1 on each button click."**

---

## Why Other Options are Wrong?
### Option A (starts at 10)
Ye ek **common misconception-based trap** hai — students sochte hain ki root instance ka `data: { start: 10 }` kisi tarah child component ke `start` prop ko override kar dega ya connect ho jayega. Lekin actually parent template me **explicitly `:start="5"` (hardcoded literal)** diya gaya hai, jo root ke `start` data se **bilkul independent** hai. Root ka `start:10` is code me **kahi bhi use hi nahi ho raha** actually (dead/unused data property hai yaha).

### Option C (Nothing renders, template invalid via CDN)
Galat hai — Vue 2 CDN version **string templates** (jaise backtick-based `template: \`...\`` syntax) ko **support karta hai** — ye completely valid syntax hai. Full build (CDN version) me runtime compiler included hota hai jo templates ko compile kar sakta hai.

### Option D (Vue throws warning, fails to bind)
Galat hai — koi warning ya binding failure nahi hoga. Ye template syntax Vue 2 CDN build ke sath **perfectly compatible** hai — ye galat statement hai jo Vue build variants (full vs runtime-only) ki confusion se aata hai, lekin standard CDN link **full build** hoti hai jisme compiler included hota hai.

---

## Important Exam Notes
- ✅ `:propName="literal"` = hardcoded value pass ho rahi hai, parent data se automatic link nahi hota.
- ✅ `:propName="dataProperty"` = ye **actual reactive binding** hoti hai parent ke data property se.
- ✅ Props `data()` function chalne se **pehle** available hote hain — `this.propName` safely `data()` ke andar use kar sakte hain.
- ⚠️ Common Mistake: Same naam ki root data property aur child prop ko automatically "linked" samajh lena — ye sirf explicit binding se hota hai.
- 💡 Trick: Hamesha check karo — parent template me prop **quotes ke andar literal value hai ya variable name**.

---

## Similar Question Pattern
Props passing (literal vs data-bound), aur `data()` function me props access karne wale conceptual + trace-output questions common hain Vue component-based questions me.

---

## Revision Box
`:start="5"` parent template me **hardcoded literal 5** pass kar raha hai `my-counter` component ko — root instance ke `data:{start:10}` se **koi connection nahi**. Component ke andar `count = this.start = 5` initial value leta hai. Button click pe `count++` hota hai — **starts at 5, increments by 1**.

---
---

# Question 121

## Original Question
```js
function createMultiplier(base) {
    let multiplier = base;

    function updateMultiplier(newValue) {
        multiplier = newValue;
        return multiplier;
    }

    function multiply(num) {
        return num * multiplier;
    }

    return {
        update: updateMultiplier,
        calculate: multiply,
        getMultiplier: function() { return multiplier; }
    };
}

const math1 = createMultiplier(3);
const math2 = createMultiplier(5);

console.log(math1.calculate(4));
console.log(math2.calculate(2));
console.log(math1.update(7));
console.log(math1.calculate(4));
console.log(math2.getMultiplier());
console.log(math1.getMultiplier());
```
> What will be the output of the above program?
>
> Options:
> A. `12, 10, 7, 12, 5, 7`
> B. `12, 10, 7, 28, 5, 7`
> C. `12, 10, 3, 28, 5, 3`
> D. `15, 10, 7, 28, 7, 7`
> E. `12, 10, 7, 21, 5, 7`

---

## Correct Answer
**Correct Option:** B (`12, 10, 7, 28, 5, 7`)

---

## Concept Used
- 📘 **Closures with Multiple Returned Functions (Module Pattern):** `createMultiplier` function **teen internal functions** (`updateMultiplier`, `multiply`, aur ek inline `getMultiplier`) ko ek **object** ke andar wrap karke return kar raha hai. Ye teeno functions **same `multiplier` variable** ko closure ke through share karte hain.
- 📘 **Independent Closures per Function Call:** Har baar `createMultiplier()` call hota hai (`math1` aur `math2` ke liye alag-alag), ek **naya, independent** `multiplier` variable banta hai. `math1` aur `math2` ke `multiplier` **completely separate** hote hain — ek dusre ko affect nahi karte.
- 📘 **Shared State Within One Instance:** `math1` ke teeno methods (`update`, `calculate`, `getMultiplier`) **same `multiplier` variable share karte hain** — agar ek method usse update kare, to baaki methods bhi **updated value** dekhenge (kyunki wo sab same closure ke andar hain).

---

## Step-by-Step Solution
1. **Step 1:** `const math1 = createMultiplier(3);` — Ek naya closure banta hai jisme `multiplier = 3`. `math1` in teen functions ka object hai jo is `multiplier` (3) ko share karte hain.
2. **Step 2:** `const math2 = createMultiplier(5);` — Ek **alag, independent** closure banta hai jisme `multiplier = 5`. `math2` ka `multiplier` `math1` ke `multiplier` se **bilkul separate** hai.
3. **Step 3:** `console.log(math1.calculate(4));`
   - `multiply(4)` → `4 * multiplier(3) = 12`
   - **Output: `12`**
4. **Step 4:** `console.log(math2.calculate(2));`
   - `math2` ka `multiply(2)` → `2 * multiplier(5) = 10`
   - **Output: `10`**
   - *Reason:* `math2` ka apna alag `multiplier(5)` hai, `math1` se koi relation nahi.
5. **Step 5:** `console.log(math1.update(7));`
   - `updateMultiplier(7)` chalta hai `math1` ke closure me — `multiplier = 7` (3 se 7 update hua), phir `return multiplier` → `7`.
   - **Output: `7`**
   - *Reason:* Ye `math1`'s `multiplier` ko permanently update kar deta hai — future calls me ye naya value use hogi.
6. **Step 6:** `console.log(math1.calculate(4));`
   - Ab `math1`'s `multiplier` **updated hoke 7** ho chuka hai (Step 5 ki wajah se).
   - `multiply(4)` → `4 * multiplier(7) = 28`
   - **Output: `28`**
   - *Reason:* Closures **live reference** rakhte hain, isliye `multiply` function turant naye `multiplier` (7) ko dekhega, purana (3) nahi.
7. **Step 7:** `console.log(math2.getMultiplier());`
   - `math2` ka `multiplier` **abhi bhi 5** hai (kabhi update nahi hua, `math1` ke updates se **unaffected** hai kyunki alag closure hai).
   - **Output: `5`**
8. **Step 8:** `console.log(math1.getMultiplier());`
   - `math1` ka `multiplier` **7** hai (Step 5 me update hua tha).
   - **Output: `7`**

**Shortcut:** Jab bhi ek hi factory function (`createMultiplier`) se **multiple independent instances** (`math1`, `math2`) banayi jaayein, turant socho — "**har instance ka apna alag closure/state hai, ek dusre ko touch nahi karte**" — bas ek instance ke andar ke saare methods **shared state** rakhte hain.

---

## Final Answer
**`12, 10, 7, 28, 5, 7`**

---

## Why Other Options are Wrong?
### Option A (last two values `12, 5, 7`)
Galat hai — 4th value `12` hai is option me, jo galat hai kyunki `math1`'s `multiplier` **already 7 ho chuka hai** update ke baad (Step 5), isliye `calculate(4)` = `4*7=28` hona chahiye, `12` (jo purani `multiplier=3` ke sath calculate hua hota) nahi.

### Option C (`3, 28, 5, 3` pattern)
Galat hai — 3rd value `3` galat hai; `update(7)` khud `7` return karta hai (naya multiplier value), `3` (purani value) nahi. Aur last value bhi `3` galat hai — `math1`'s multiplier update ho chuka hai `7` pe.

### Option D (`15, ..., 7, 7` pattern)
Galat hai — 1st value `15` galat hai; `math1.calculate(4)` shuru me `4*3=12` hona chahiye (`multiplier` abhi update nahi hua tha), `15` nahi. 5th value bhi galat hai — `math2` ka multiplier `5` hi rehna chahiye (kabhi update nahi hua), `7` nahi.

### Option E (`21` wala option)
Galat hai — 4th value `21` galat hai; sahi calculation `4 * 7 = 28` hai (update ke baad multiplier 7 hai), `21` (jo `3*7` ho sakta hai galti se) nahi.

---

## Important Exam Notes
- ✅ Har `createMultiplier()` call apna **naya, independent closure** banata hai.
- ✅ Ek instance (`math1`) ke andar ke **saare methods** (`update`, `calculate`, `getMultiplier`) **same variable share** karte hain — ek method ka change dusre methods ko bhi dikhega.
- ✅ Do alag instances (`math1`, `math2`) ke closures **completely independent** hote hain.
- ⚠️ Common Mistake: `update()` call karne ke baad purani `multiplier` value use kar lena — closures **hamesha latest value** dekhte hain.
- 💡 Trick: Ek table banao jisme har instance ka `multiplier` value track karo, har operation ke baad update karte jao.

| Step | Action | math1's multiplier | math2's multiplier | Output |
|---|---|---|---|---|
| 1 | Create | 3 | - | - |
| 2 | Create | 3 | 5 | - |
| 3 | math1.calculate(4) | 3 | 5 | 12 |
| 4 | math2.calculate(2) | 3 | 5 | 10 |
| 5 | math1.update(7) | 7 | 5 | 7 |
| 6 | math1.calculate(4) | 7 | 5 | 28 |
| 7 | math2.getMultiplier() | 7 | 5 | 5 |
| 8 | math1.getMultiplier() | 7 | 5 | 7 |

---

## Similar Question Pattern
Module pattern / factory functions with multiple returned methods sharing closure state — ye high-mark (4.5 marks) trace-output questions me common hai, patience se track karna padta hai.

---

## Revision Box
`createMultiplier(base)` har baar naya independent closure banata hai. `math1` aur `math2` alag-alag `multiplier` maintain karte hain. Ek instance ke saare methods (`update`, `calculate`, `getMultiplier`) same `multiplier` share karte hain — update hone se sabko naya value dikhta hai. Output: **12, 10, 7, 28, 5, 7**.

---
---

# Question 122

## Original Question
```js
var globalScore = 25;

const player1 = {
    score: 150,
    getScore: function() {
        return this.score;
    },
    displayScore: function(bonus) {
        return this.score + (bonus || 0);
    }
};

const player2 = { score: 200 };
const player3 = { score: 75 };

const method1 = player1.getScore;
const method2 = player1.getScore.bind(player2);
const method3 = player1.displayScore.bind(player3);

console.log(method1());
console.log(method2());
console.log(method3(50));
console.log(method3.call(player1, 25));
```
> What will be the output of the above JavaScript code?
>
> Options:
> A. `undefined, 200, 125, 175`
> B. `25, 200, 125, 175`
> C. `150, 200, 125, 100`
> D. `25, 200, 125, 100`
> E. `undefined, 200, 125, 100`

---

## Correct Answer
**Correct Option:** E (`undefined, 200, 125, 100`)

---

## Concept Used
- 📘 **Detached Method (`method1 = player1.getScore`):** Jab function reference ko object se **detach** karke ek plain variable me store kiya jaata hai, aur baad me **plain call** (`method1()`) kiya jaata hai, to `this` binding **lost** ho jaati hai — `this` object ki jagah **global object (`window`)** (non-strict mode) ya **`undefined`** (strict mode) ban jaata hai.
- 📘 **`.bind()` — Permanent `this` Locking:** `.bind(obj)` ek **naya function** return karta hai jiska `this` **permanently** `obj` pe **lock** ho jaata hai. Ye lock **kabhi bhi override nahi ho sakta** — chahe aap us bound function ko `.call()` ya `.apply()` se dobara call karne ki koshish karo aur naya `this` pass karo, wo **ignore** ho jaayega, original bound `this` hi use hoga.
- 📘 **`var` at Top-Level — Window Attachment:** `var globalScore = 25;` top-level pe likha gaya hai — ye `window.globalScore` bhi ban jaata hai browser me. **Lekin dhyan do** — property ka naam `globalScore` hai, **`score` nahi**! Isliye `window.score` **exist nahi karta**, chahe `globalScore` window pe attached ho.

---

## Step-by-Step Solution
1. **Step 1:** `const method1 = player1.getScore;` — Ye `getScore` function ko **detach** kar raha hai `player1` se — bina call kiye, sirf reference store ho raha hai.
2. **Step 2:** `console.log(method1());` — `method1()` **plain function call** hai (bina kisi object ke through, jaise `player1.method1()` nahi, seedha `method1()`).
   - Plain call me regular function ka `this` = **global object (`window`)** (browser, non-strict mode assuming — normal `<script>` tag).
   - `this.score` → `window.score` → **`undefined`** (kyunki `window` pe koi `score` naam ki property set nahi hui hai — sirf `globalScore` set hui hai, jo alag naam hai).
   - **Output: `undefined`**
   - *Reason:* Ye ek classic "detached method" trap hai — `this` binding lost ho jaati hai plain call me.
3. **Step 3:** `const method2 = player1.getScore.bind(player2);` — `.bind(player2)` ek **naya function** banata hai jiska `this` **permanently `player2`** pe lock ho gaya hai.
   - `console.log(method2());` → `this = player2` (bound), `this.score` → `player2.score` = **`200`**.
   - **Output: `200`**
4. **Step 4:** `const method3 = player1.displayScore.bind(player3);` — `displayScore` ka `this` **permanently `player3`** pe lock ho gaya.
   - `console.log(method3(50));` → `this = player3` (bound), `bonus = 50`.
   - `this.score + (bonus || 0)` → `player3.score(75) + 50` → `75 + 50 = 125`.
   - **Output: `125`**
5. **Step 5:** `console.log(method3.call(player1, 25));` — Ye `method3` (jo **already bound hai `player3` se**) ko **`.call(player1, 25)`** se call karne ki koshish kar raha hai — matlab `this` ko `player1` banane ki koshish, aur `25` ko `bonus` banane ki koshish.
   - **IMPORTANT:** Ek baar `.bind()` ho jaane ke baad, `this` **permanently lock** ho jaata hai — `.call()` se dobara `this` change karne ki koshish **completely ignore** ho jaati hai. `this` **`player3` hi rahega**, `player1` nahi banega.
   - Lekin **arguments** (`25`) normally pass ho jaate hain (bind sirf `this` ko lock karta hai, arguments ko tab tak lock nahi karta jab tak explicitly `bind()` call ke waqt hi arguments bhi diye gaye hon — yaha nahi diye the).
   - Isliye: `this = player3` (locked, `player1` ignore hua), `bonus = 25` (ye pass ho gaya).
   - `this.score + (bonus || 0)` → `player3.score(75) + 25` → `75 + 25 = 100`.
   - **Output: `100`**
   - *Reason:* `.bind()` se bana hua function "**permanently sealed**" hota hai `this` ke liye — koi bhi baad ka `.call()`/`.apply()` attempt `this` ko change **nahi** kar sakta.

**Shortcut:** Yaad rakho — "**Bind is Forever**" — ek baar `.bind()` ho gaya, to `this` **hamesha wahi** rahega, chahe kitni baar `.call()`/`.apply()` try karo naya `this` dene ke liye.

---

## Final Answer
**`undefined, 200, 125, 100`**

---

## Why Other Options are Wrong?
### Option A (`undefined, ..., 175`)
Galat hai — last value `175` galat hai. Ye tab hota agar `.call(player1, 25)` `this` ko successfully `player1` bana deta (`150+25=175`), lekin `.bind()` ke baad `this` **change nahi ho sakta** — `player3` hi rahega.

### Option B (`25, ...`)
Galat hai — 1st value `25` galat hai. Ye tab hota agar `this` = global object hota aur `globalScore`(25) hi `score` property maan liya jaata — lekin property naam **`score`** hai, `globalScore` nahi, ye alag naam hai. `window.score` `undefined` hi hai.

### Option C (`150, ..., 100`)
Galat hai — 1st value `150` galat hai. Ye tab hota agar `method1()` call hone par `this` kisi tarah `player1` reh jaata — lekin plain call me `this` binding **lost** ho jaati hai, `player1.score(150)` nahi milega.

### Option D (`25, ..., 100`)
Last value (`100`) sahi hai, lekin 1st value (`25`) galat hai — same reason jo Option B me bataya (property naam mismatch — `score` vs `globalScore`).

---

## Important Exam Notes
- ✅ Detached method (`obj.method` bina call kiye variable me store karna) + plain call = `this` binding **lost**.
- ✅ `.bind()` = **permanent** `this` lock — kabhi bhi baad me `.call()`/`.apply()` se override **nahi** ho sakta.
- ✅ Property **naam match** karna zaroori hai — `globalScore` aur `score` **alag** properties hain, even though dono numbers hain.
- ⚠️ Common Mistake: Sochna ki `.call()` `.bind()` ke baad bhi `this` change kar sakta hai — ye bahut common galti hai.
- 💡 Trick: "**Bind = Superglue for `this`**" — ek baar chipak gaya, phir kabhi nahi hatega!

---

## Similar Question Pattern
`this` binding ke saath `bind()`, `call()`, `apply()` ka combination, especially "bind ke baad call try karna" wale trap questions high-mark me bahut common hain.

---

## Revision Box
`method1()` (detached, plain call) → `this=window`, `window.score=undefined`. `method2()` (bound to player2) → `this=player2`, `200`. `method3(50)` (bound to player3) → `75+50=125`. `method3.call(player1,25)` → **bind permanent hai**, `this` still `player3` (player1 ignored), but argument `25` pass hota hai → `75+25=100`. Output: **undefined, 200, 125, 100**.

---
---

# Question 123

## Original Question
```js
function normalFunc() {
  console.log(this.constructor.name);
}

const arrowFunc = () => {
  console.log(this.constructor.name);
};

const objj = {
  name: 'Test',
  normalMethod: normalFunc,
  arrowMethod: arrowFunc,
  outer: function () {
    return () => {
      console.log(this.name);
    };
  }
};

const inner = objj.outer();

objj.normalMethod();
objj.arrowMethod();
inner();
new arrowFunc();
```
> Which of the following statements is/are TRUE?
>
> Options:
> A. `obj.normalMethod()` prints "Object"
> B. `obj.arrowMethod()` prints "Window" or throws an error in strict mode
> C. `inner()` prints "Test"
> D. `new arrowFunc()` throws an error because arrow functions cannot be used as constructors

(Multiple Select Question, Correct Marks: 3)

---

## Correct Answer
**Correct Options:** A, B, C, and D (saare TRUE hain)

---

## Concept Used
- 📘 **`this.constructor.name`:** Har object ka `constructor` property us **class/function** ko point karti hai jisne use bana ya. `.name` us constructor function ka **naam string form me** deta hai (jaise `"Object"` plain objects ke liye, `"Window"` window object ke liye).
- 📘 **Regular Function as Method (`normalMethod: normalFunc`):** Jab `normalFunc` ko `objj.normalMethod()` ki tarah **call** kiya jaata hai, `this = objj`. `objj` ek **plain object literal** hai, jiska constructor **`Object`** hai (default). Isliye `this.constructor.name = "Object"`.
- 📘 **Arrow Function as "Method" (`arrowMethod: arrowFunc`):** `arrowFunc` top-level pe **arrow function** ki tarah define hua hai — iska `this` **top-level/lexical scope** se aata hai (`window`, browser me), **object ka nahi**, chahe ise `objj.arrowMethod` naam diya gaya ho. Isliye `this.constructor.name` = `window.constructor.name` = **`"Window"`** (browser me).
- 📘 **Nested Closure (`outer` returns arrow function):** `outer` ek **regular function** hai, jab `objj.outer()` call hota hai, `this = objj`. `outer` ke andar jo **arrow function return** hoti hai, uska `this` = `outer`'s `this` (lexically inherited) = `objj`. Isliye jab `inner()` (jo `outer()` se return hui thi) **kahi bhi/kaise bhi** call ho, uska `this` hamesha `objj` hi rahega (arrow function ka `this` badalta nahi call-site se).
- 📘 **Arrow Functions Cannot be Constructors:** Arrow functions ke pass **`[[Construct]]` internal method nahi hota** (jo `new` keyword ke sath object banane ke liye zaroori hai) — isliye `new arrowFunc()` likhna **`TypeError: arrowFunc is not a constructor`** throw karega.

---

## Step-by-Step Solution
1. **Statement A: `objj.normalMethod()` prints "Object"**
   - `normalFunc` `objj.` ke through call hui, isliye `this = objj`.
   - `objj` ek plain object literal hai (`{}` syntax se bana), iska default constructor `Object` hai.
   - `this.constructor.name` → `objj.constructor.name` → `"Object"`.
   - **TRUE** ✅

2. **Statement B: `objj.arrowMethod()` prints "Window" or throws error in strict mode**
   - `arrowFunc` ek **top-level arrow function** hai — iska `this` object literal ke andar likhe jaane ke bawajood **top-level scope** se aata hai (arrow functions object literal se scope nahi lete, sirf enclosing **function/module** scope se lete hain).
   - Non-strict browser script me: `this = window` (top-level `this`). `window.constructor.name = "Window"`.
   - Strict mode/module context me: `this = undefined` at top level, aur `undefined.constructor` access karte hi **TypeError** throw hoga.
   - **TRUE** ✅ (dono scenarios cover kiye gaye hain option me — "prints Window **or** throws error")

3. **Statement C: `inner()` prints "Test"**
   - `objj.outer()` call hota hai — `outer` regular function hai, `this = objj` (call-site se).
   - `outer` ke andar arrow function return hoti hai — is arrow function ka `this` = `outer`'s `this` (lexically captured) = `objj`. Ye binding **permanent** hai, kyunki arrow function ka `this` kabhi badalta nahi.
   - `inner = objj.outer();` — `inner` ab is arrow function ko refer karta hai, jiska `this` already `objj` pe **fix** ho chuka hai.
   - `inner();` — Chahe ye **plain call** ho (bina kisi object ke through), arrow function ka `this` **badalta nahi** — wo already `objj` pe locked hai.
   - `this.name` → `objj.name` = `"Test"`.
   - **TRUE** ✅

4. **Statement D: `new arrowFunc()` throws an error because arrow functions cannot be used as constructors**
   - Arrow functions specifically JS specification ke hisaab se **constructors nahi ban sakte** — unke pass `[[Construct]]` internal capability nahi hoti.
   - `new arrowFunc()` likhte hi JavaScript turant **TypeError** throw karega: "arrowFunc is not a constructor".
   - **TRUE** ✅

---

## Final Answer
**All four statements (A, B, C, and D) are TRUE.**

---

## Why Other Options are Wrong?
Is question me **koi bhi option galat nahi hai** — saare statements sahi hain. Ye ek **"select all correct"** type ka MSQ hai jaha JavaScript ke `this` binding ke multiple important concepts (regular function methods, arrow function methods, nested closures, aur constructor restrictions) ek sath test kiye gaye hain.

---

## Important Exam Notes
- ✅ Regular function method call → `this` = calling object.
- ✅ Arrow function "method" (top-level defined) → `this` = top-level/lexical scope, **object ka nahi**.
- ✅ Regular function ke andar return hui arrow function → `this` **permanently** us regular function ke `this` pe lock ho jaati hai (kabhi badalta nahi, chahe kaise bhi call ho).
- ✅ Arrow functions **kabhi bhi constructors nahi** ban sakte (`new` ke sath use nahi ho sakte).
- ⚠️ Common Mistake: Arrow function ko object literal ke andar likhe jaane ki wajah se "object ka method" samajh lena — arrow function ka `this` **kabhi bhi object literal se nahi aata**, sirf enclosing function/module scope se aata hai.
- 💡 Trick: "Arrow function jaha bhi ho, uska `this` **sirf outer function ka hoga**, object literal `{}` scope create nahi karti!"

---

## Similar Question Pattern
`this` binding ke saath regular vs arrow functions ke multiple scenarios (direct method, nested closures, constructors) ke combination wale comprehensive MSQ questions bahut common hain — inme JavaScript ke `this` ke **saare rules ek sath** test hote hain.

---

## Revision Box
`normalMethod()` (regular fn, object method) → `this=objj`, constructor="Object" — TRUE. `arrowMethod()` (arrow fn, top-level defined) → `this=window/undefined` (object ka nahi) — TRUE ("Window" ya error). `inner()` (arrow returned from regular fn, permanently bound to `objj`) → `this.name="Test"` — TRUE. `new arrowFunc()` → arrow functions constructors nahi ban sakte — TypeError — TRUE. **Sab TRUE hain.**

---
---

# Question 124

## Original Question
**Comprehension Context:** Vue 2 (via CDN) app for a login form. Show welcome message after user enters name and clicks "Login". Bind input using `v-model`.

```html
<body>
<div id="app">
  <input v-model="username">
  <button @click="loggedIn = true">Login</button>

  <p v-if="loggedIn">
    Welcome, {{ username }}!
  </p>
</div>

<script>
  new Vue({
    el: '#app',
    data: {
      username: '',
      loggedIn: false
    }
  });
</script>
</body>
</html>
```
> Which line of code should be changed to make this code work properly?
>
> Options:
> A. `<input v-model="username">` should be replaced with `<input v-bind="username">`
> B. `@click="loggedIn = true"` should be replaced with `v-on:click="login()"`
> C. `v-if="loggedIn"` should be replaced with `v-show="loggedIn"`
> D. The code works correctly; no change is required.

---

## Correct Answer
**Correct Option:** D (The code works correctly; no change is required.)

---

## Concept Used
- 📘 **`v-model`:** `<input v-model="username">` **correct syntax** hai two-way binding ke liye — user jo bhi type karega, `username` data property automatically update hoti rahegi.
- 📘 **Inline Expression in `@click`:** Vue `@click` (jo `v-on:click` ka shorthand hai) ke andar **simple inline JavaScript expressions** allow karta hai, jaise `@click="loggedIn = true"` — ye ek **valid aur common pattern** hai jab logic bahut simple ho (bina kisi method define kiye).
- 📘 **`v-if`:** `<p v-if="loggedIn">` — Ye conditionally element ko DOM me **render/remove** karta hai based on `loggedIn` ki value. Ye is use-case ke liye **perfectly appropriate** hai.

---

## Step-by-Step Solution
1. **Step 1: `v-model="username"` check karo** — Ye syntax **bilkul sahi** hai. `v-model` specifically form inputs (jaise `<input>`) ke liye design kiya gaya hai two-way binding ke liye. Koi problem nahi hai.
2. **Step 2: `@click="loggedIn = true"` check karo** — Vue `@click` ke andar directly simple assignment expressions likhne deta hai, ye **valid syntax** hai. Jab user button click karega, `loggedIn` data property directly `true` ho jaayegi.
   - *Reason:* Complex logic ke liye method use karna better practice hai, lekin simple assignments ke liye inline expression bhi **completely valid** hai — koi syntax error nahi hai.
3. **Step 3: `v-if="loggedIn"` check karo** — Ye **conditionally element render** karta hai jab `loggedIn` `true` ho jaaye. Ye exact requirement (welcome message dikhana login ke baad) ko sahi tarike se fulfill karta hai.
4. **Step 4: Poora code trace karo:**
   - Initial state: `username = ''`, `loggedIn = false` → welcome message **nahi** dikhega (`v-if="loggedIn"` false hai).
   - User input box me naam type karta hai → `v-model` automatically `username` update kar deta hai.
   - User "Login" click karta hai → `loggedIn = true` set ho jaata hai.
   - `v-if="loggedIn"` ab **true** hai, isliye `<p>` element **render** hota hai, aur `{{ username }}` correctly display karega jo user ne type kiya tha.
5. **Step 5:** Poora flow **sahi kaam kar raha hai**, koi bug nahi hai — isliye koi change ki zaroorat nahi.

**Shortcut:** Jab bhi "which line is wrong" type ka question ho, **har line ko individually verify karo** against Vue ke standard syntax rules — agar sab kuch standard/valid pattern follow kar raha hai, to answer "no change required" ho sakta hai.

---

## Final Answer
**"The code works correctly; no change is required."**

---

## Why Other Options are Wrong?
### Option A (`v-model` → `v-bind`)
Ye **galat suggestion** hai — `v-bind` sirf **one-way** binding karta hai (data → attribute), agar `v-model` ko `v-bind` se replace kiya jaaye, to **two-way binding toot jaayegi** — user input box me type karega, lekin `username` data update **nahi** hogi. Ye code ko **worse** banayega, behtar nahi.

### Option B (`@click="loggedIn=true"` → `v-on:click="login()"`)
Ye technically **possible improvement** hai (better practice ke liye), lekin question puch raha hai "kya code kaam nahi kar raha", aur current syntax (`@click="loggedIn=true"`) **already valid aur working** hai. Isko replace karna zaroorat nahi hai functionality ke liye — `login()` method bhi define nahi kiya gaya hai is option me, jo isse aur bhi galat banata hai (agar aisa method exist hi nahi karta to error aayega).

### Option C (`v-if` → `v-show`)
Ye bhi galat hai — `v-show` bhi kaam kar sakta tha (CSS display toggle karke), lekin `v-if` **already sahi tarike se kaam kar raha hai** is use-case ke liye. Koi functional problem nahi hai jo `v-show` se solve ho.

---

## Important Exam Notes
- ✅ `v-model` = two-way binding, form inputs ke liye correct choice.
- ✅ `@click="expression"` = simple inline expressions allowed hain (assignment, function calls, etc.).
- ✅ `v-if` = conditional DOM rendering, is scenario ke liye appropriate hai.
- ⚠️ Common Mistake: "Kuch change hona chahiye" assume kar lena jab question puche "kya galat hai" — kabhi kabhi answer hota hai ki **kuch galat hai hi nahi**.
- 💡 Trick: Comprehension-based "find the bug" questions me **poora code trace karo end-to-end** pehle, phir decide karo koi bug hai ya nahi.

---

## Similar Question Pattern
"Find the bug/error in this Vue code" type comprehension questions common hain — inme dhyan rakhna hai ki **har baar bug hona zaroori nahi**, code already correct bhi ho sakta hai.

---

## Revision Box
Code me `v-model` (two-way binding), `@click` (inline expression), aur `v-if` (conditional rendering) — teeno **correctly** use hue hain apne intended purpose ke liye. Poora login flow sahi kaam karta hai. **Koi change zaroori nahi hai.**

---
---

# Question 125

## Original Question
**Same comprehension context as Question 124.**

> If you wanted to make this app more maintainable by moving logic to methods, which line would you replace?
>
> Options:
> A. Replace `@click="loggedIn = true"` with `@click="login"` and define a method
> B. Replace `<input v-model="username">` with `v-on:input="updateUsername"`
> C. Replace `v-if="loggedIn"` with `v-else="loggedIn"`
> D. This code is best, no changes required.

---

## Correct Answer
**Correct Option:** A (Replace `@click="loggedIn = true"` with `@click="login"` and define a method)

---

## Concept Used
- 📘 **Code Maintainability — Methods vs Inline Expressions:** Chhoti/simple expressions (jaise `loggedIn = true`) inline `@click` me likhna theek hai chhote projects ke liye, lekin **maintainability aur scalability** ke liye, logic ko **`methods`** object me define karna **best practice** hai — isse code **reusable**, **testable**, aur **readable** banta hai, especially jab logic complex ho jaaye future me (jaise validation add karna, API call karna, etc.).
- 📘 **`methods` Object:** Vue me `methods: { login() { this.loggedIn = true; } }` define karke, template me sirf `@click="login"` likha ja sakta hai — logic **template se separate** ho jaata hai, jo cleaner architecture hai.
- 📘 **`v-else` Concept (Option C ka Concept):** `v-else` ek **standalone directive** hai jo hamesha ek `v-if` (ya `v-else-if`) ke **turant baad** aana chahiye, bina kisi value/expression ke (jaise `v-else`, `v-else="loggedIn"` nahi). Isliye Option C conceptually bhi galat hai.

---

## Step-by-Step Solution
1. **Step 1:** Current code me login logic **directly template me** likha hai: `@click="loggedIn = true"`.
   - *Reason:* Ye chhota sa assignment hai, isliye abhi ke liye "kaam" kar raha hai, lekin agar future me login ke sath aur logic add karna ho (jaise validation, API call, error handling), to template **messy** ho jaayega.
2. **Step 2:** Best practice ye hai ki is logic ko ek **named method** me move kiya jaaye:
   ```js
   methods: {
     login() {
       this.loggedIn = true;
     }
   }
   ```
   - Template me phir sirf: `@click="login"` likhna hoga.
   - *Reason:* Isse template **clean** rehta hai, aur logic ko **easily test/modify/reuse** kiya ja sakta hai bina template ko touch kiye.
3. **Step 3:** Baaki lines (`v-model="username"`, `v-if="loggedIn"`) already apne purpose ke liye **optimal** hain — inhe methods me move karne ki koi zaroorat nahi (ye directives hi hain, "logic" nahi jo method banaya ja sake).

**Shortcut:** Jab bhi "maintainability ke liye kya improve karo" wala question ho, socho — "**kaunsi line me actual JavaScript logic/assignment ho raha hai template ke andar (na ki sirf directive/binding)**" — wahi line method me move karne layak hoti hai.

---

## Final Answer
**"Replace `@click='loggedIn = true'` with `@click='login'` and define a method"**

---

## Why Other Options are Wrong?
### Option B (`v-model` → `v-on:input="updateUsername"`)
Ye galat hai — `v-model` **already** two-way binding provide karta hai bahut cleanly, isse manually `v-on:input` handler se replace karna **unnecessary complexity** add karega (aapko manually `event.target.value` handle karna padega) — ye maintainability **kam** karega, badhayega nahi.

### Option C (`v-if` → `v-else`)
Ye **conceptually galat** hai — `v-else` ek **value-less directive** hai jo `v-if`/`v-else-if` ke turant baad aata hai, isse ek **standalone condition** ki tarah use nahi kiya ja sakta (jaise `v-else="loggedIn"` — ye **invalid syntax** hai). Ye option maintainability se related bhi nahi hai, ye ek syntax error introduce karega.

### Option D (No changes required)
Ye galat hai kyunki question **specifically** "maintainability improve karne" ke baare me puch raha hai — `@click="loggedIn = true"` ko method me move karna ek **standard best practice** hai jo maintainability behtar banati hai, isliye "no change" sahi answer nahi hai is specific context me.

---

## Important Exam Notes
- ✅ Simple inline expressions (`@click="x = true"`) chhote cases ke liye valid hain, lekin **methods me move karna** better maintainability practice hai.
- ✅ `v-model` already best practice hai two-way binding ke liye — ise replace karna avoid karo.
- ✅ `v-else` **value nahi le sakta** — ye hamesha `v-if`/`v-else-if` ke sath standalone use hota hai.
- ⚠️ Common Mistake: `v-model` ko manually `v-on:input` se replace karne ki koshish karna — ye unnecessary hai jab `v-model` already available hai.
- 💡 Trick: "Template me **logic/assignment** dikhe → method me move karo. Directives (`v-model`, `v-if`) already optimal hain, unhe touch mat karo."

---

## Similar Question Pattern
"Best practices/maintainability improve karna" type ke Vue code-refactoring questions common hain — inme samajhna zaroori hai kya **actually improve** karne layak hai vs kya **already optimal** hai.

---

## Revision Box
Maintainability ke liye, template ke andar ka **inline logic** (`@click="loggedIn = true"`) ko `methods` object me move karna best practice hai (`@click="login"` + `methods: { login() { this.loggedIn = true; } }`). `v-model` aur `v-if` already optimal hain, unhe touch karne ki zaroorat nahi.

---
---

# Question 126

## Original Question
**HTML:**
```html
<div id="app">
    <div :class="[baseClass, { highlighted: isHighlighted,
            'btn-primary': isPrimary, 'btn-disabled': !isEnabled }]">
        {{ buttonText }}
    </div>
    <button @click="toggleState">Toggle State</button>
</div>
```
**Script:**
```js
const app = new Vue({
    el: '#app',
    data: {
        buttonText: 'Click Me',
        baseClass: 'btn',
        isHighlighted: false,
        isPrimary: true,
        isEnabled: true
    },
    methods: {
        toggleState() {
            this.isHighlighted = !this.isHighlighted;
            this.isPrimary = !this.isPrimary;
            this.isEnabled = !this.isEnabled;
        }
    }
})
```
> What classes will be applied to the `<div>` initially (before any button clicks)?
>
> Options:
> A. `btn, highlighted, btn-primary`
> B. `btn, btn-primary`
> C. `baseClass, btn-primary`
> D. `btn, highlighted, btn-primary, btn-disabled`

---

## Correct Answer
**Correct Option:** B (`btn, btn-primary`)

---

## Concept Used
- 📘 **Array Syntax for `:class` (Dynamic Class Binding):** Vue me `:class="[...]"` **array syntax** allow karta hai multiple class-sources ko combine karne ke liye. Array ke andar aap **strings** (direct class names) aur **objects** (conditional classes) dono mix kar sakte ho.
- 📘 **String Element in Array (`baseClass`):** Array ka pehla element `baseClass` (jo `'btn'` string hai) **hamesha directly apply** hoga — ye koi condition nahi follow karta, bas uski **current value** class ban jaati hai.
- 📘 **Object Element in Array (`{ className: condition }`):** Array ke andar ka object **conditional classes** define karta hai — is object ki **har key ek class name** hai, aur **value ek boolean condition** hai. Agar condition `true` hai, to wo class **apply** hoti hai; agar `false` hai, to **nahi** hoti.

**Example:**
```js
:class="['base', { active: isActive, disabled: !isEnabled }]"
// Agar isActive=true, isEnabled=true → classes: "base active"
// Agar isActive=false, isEnabled=false → classes: "base disabled"
```

---

## Step-by-Step Solution
1. **Step 1: Initial data values check karo:**
   - `baseClass = 'btn'`
   - `isHighlighted = false`
   - `isPrimary = true`
   - `isEnabled = true`
2. **Step 2: Array ka pehla element (`baseClass`) evaluate karo:**
   - `baseClass = 'btn'` — Ye **hamesha directly apply** hoga, koi condition nahi hai.
   - **Applied: `btn`**
3. **Step 3: Object ke andar `highlighted: isHighlighted` check karo:**
   - `isHighlighted = false` → condition **false** hai, isliye `highlighted` class **apply nahi** hogi.
4. **Step 4: Object ke andar `'btn-primary': isPrimary` check karo:**
   - `isPrimary = true` → condition **true** hai, isliye `btn-primary` class **apply** hogi.
   - **Applied: `btn-primary`**
5. **Step 5: Object ke andar `'btn-disabled': !isEnabled` check karo:**
   - `isEnabled = true`, isliye `!isEnabled = !true = false` → condition **false** hai, isliye `btn-disabled` class **apply nahi** hogi.
6. **Step 6: Final applied classes combine karo:**
   - `btn` (always) + `btn-primary` (condition true) = **`"btn btn-primary"`**
   - `highlighted` aur `btn-disabled` **applied nahi** hain (unki conditions false thi).

**Shortcut:** Aise `:class` array-with-object questions me **ek table banao** — har class ke saamne uski condition ki current value likho, phir sirf **true** wali classes ko final list me rakho.

| Class | Condition | Value | Applied? |
|---|---|---|---|
| `btn` | (always, string) | - | ✅ Yes |
| `highlighted` | `isHighlighted` | `false` | ❌ No |
| `btn-primary` | `isPrimary` | `true` | ✅ Yes |
| `btn-disabled` | `!isEnabled` | `!true = false` | ❌ No |

---

## Final Answer
**`btn, btn-primary`**

---

## Why Other Options are Wrong?
### Option A (`btn, highlighted, btn-primary`)
Galat hai — `highlighted` class **apply nahi** honi chahiye kyunki `isHighlighted = false` hai initially.

### Option C (`baseClass, btn-primary`)
Galat hai — Ye ek **common misconception** hai jaha `baseClass` (variable **naam**) ko literal class name samajh liya gaya, jabki actually `baseClass` **variable ki value** (`'btn'`) apply hoti hai, uska naam nahi.

### Option D (`btn, highlighted, btn-primary, btn-disabled`)
Galat hai — Ye **saari possible classes** ko apply maan raha hai bina conditions check kiye. Actually sirf `true` conditions wali classes apply hoti hain — `highlighted` aur `btn-disabled` dono ki conditions **false** hain initially.

---

## Important Exam Notes
- ✅ `:class="[stringVar, {conditionalClasses}]"` — array me string hamesha apply hoti hai, object ke andar conditional classes hoti hain.
- ✅ Object ke andar key = class name, value = boolean condition.
- ⚠️ Common Mistake: Variable **naam** (jaise `baseClass`) ko literal class samajh lena, jabki variable ki **value** use hoti hai.
- 💡 Trick: Table banao — har conditional class ke aage uski current boolean value likho, sirf `true` wali classes final list me jaayengi.

---

## Similar Question Pattern
Vue `:class` binding (array syntax, object syntax, ya dono ka combination) ke trace-based questions common hain — dhyan rakhna hai kaunsi classes conditionally apply ho rahi hain.

---

## Revision Box
`:class="[baseClass, {highlighted: isHighlighted, 'btn-primary': isPrimary, 'btn-disabled': !isEnabled}]"` — `baseClass` ('btn') hamesha apply. Object ke andar sirf `true` conditions wali classes apply hoti hain. Initial values: `isHighlighted=false` (skip), `isPrimary=true` (apply), `!isEnabled=false` (skip). Final: **btn, btn-primary**.

---
---

# Question 127

## Original Question
**Same code as Question 126.**

> What classes will be applied to the `<div>` after clicking the "Toggle State" button once?
>
> Options:
> A. `btn, highlighted`
> B. `btn, btn-disabled`
> C. `btn, highlighted, btn-disabled`
> D. `baseClass, highlighted, btn-disabled`

---

## Correct Answer
**Correct Option:** C (`btn, highlighted, btn-disabled`)

---

## Concept Used
- 📘 **State Toggling with `!` (Logical NOT) Operator:** `toggleState()` method **teeno boolean values ko flip** karta hai using `!` operator — jo bhi current value hai, uska **opposite** ban jaata hai (`true → false`, `false → true`).
- 📘 **Reactivity Recalculation:** Jab `isHighlighted`, `isPrimary`, `isEnabled` change hoti hain, Vue **automatically** `:class` binding ko **re-evaluate** karta hai naye values ke sath — DOM me classes turant update ho jaati hain.

---

## Step-by-Step Solution
1. **Step 1: Initial values (Question 126 se):**
   - `isHighlighted = false`, `isPrimary = true`, `isEnabled = true`.
2. **Step 2: "Toggle State" button 1 baar click hota hai** — `toggleState()` method chalta hai:
   - `this.isHighlighted = !this.isHighlighted` → `!false = true` → `isHighlighted` ab **`true`** hai.
   - `this.isPrimary = !this.isPrimary` → `!true = false` → `isPrimary` ab **`false`** hai.
   - `this.isEnabled = !this.isEnabled` → `!true = false` → `isEnabled` ab **`false`** hai.
   - *Reason:* `!` operator boolean value ko **flip** kar deta hai — teeno properties ek sath toggle ho gayi.
3. **Step 3: Naye values ke sath `:class` binding phir se evaluate karo:**
   - `baseClass = 'btn'` → **hamesha applied** (koi condition nahi).
   - **Applied: `btn`**
4. **Step 4: `highlighted: isHighlighted` check karo:**
   - `isHighlighted = true` (ab, toggle ke baad) → condition **true** → `highlighted` class **apply** hogi.
   - **Applied: `highlighted`**
5. **Step 5: `'btn-primary': isPrimary` check karo:**
   - `isPrimary = false` (ab, toggle ke baad) → condition **false** → `btn-primary` class **apply nahi** hogi.
6. **Step 6: `'btn-disabled': !isEnabled` check karo:**
   - `isEnabled = false` (ab, toggle ke baad), isliye `!isEnabled = !false = true` → condition **true** → `btn-disabled` class **apply** hogi.
   - **Applied: `btn-disabled`**
7. **Step 7: Final applied classes combine karo:**
   - `btn` + `highlighted` + `btn-disabled` = **`"btn highlighted btn-disabled"`**

**Shortcut:** Same table approach use karo jaise Q126 me, bas ab **toggled (opposite) values** ke sath:

| Class | Condition | Value (After Toggle) | Applied? |
|---|---|---|---|
| `btn` | (always) | - | ✅ Yes |
| `highlighted` | `isHighlighted` | `true` (flipped from false) | ✅ Yes |
| `btn-primary` | `isPrimary` | `false` (flipped from true) | ❌ No |
| `btn-disabled` | `!isEnabled` | `!false = true` (isEnabled flipped to false) | ✅ Yes |

---

## Final Answer
**`btn, highlighted, btn-disabled`**

---

## Why Other Options are Wrong?
### Option A (`btn, highlighted`)
Galat hai — `btn-disabled` ko bhi apply hona chahiye tha, kyunki toggle ke baad `isEnabled = false`, isliye `!isEnabled = true`, jo `btn-disabled` ki condition **true** banata hai. Ye option `btn-disabled` ko miss kar raha hai.

### Option B (`btn, btn-disabled`)
Galat hai — `highlighted` ko bhi apply hona chahiye tha, kyunki toggle ke baad `isHighlighted = true`. Ye option `highlighted` ko miss kar raha hai.

### Option D (`baseClass, highlighted, btn-disabled`)
Galat hai — same misconception jaisa Q126 me tha, `baseClass` (variable **naam**) ko literal class samajh liya, jabki uski **value** (`'btn'`) apply honi chahiye.

---

## Important Exam Notes
- ✅ `!` operator boolean value ko flip karta hai — `true↔false`.
- ✅ Ek button click **teeno properties ek sath toggle** kar deta hai (`isHighlighted`, `isPrimary`, `isEnabled`).
- ✅ `:class` binding **automatically re-evaluate** hoti hai jab bhi dependent reactive properties change hoti hain.
- ⚠️ Common Mistake: Purani (pre-toggle) values ke basis pe classes calculate kar lena, naya toggle apply na karna.
- 💡 Trick: Har button click ke baad **naya table banao** with updated (flipped) values, phir se classes calculate karo.

---

## Similar Question Pattern
Multiple related boolean states ko ek sath toggle karne wale (`toggleState` jaisa method) aur unke `:class` binding pe impact ke trace-output questions common hain — dhyan rakhna hai **saari properties simultaneously flip** hoti hain.

---

## Revision Box
`toggleState()` teeno booleans flip karta hai: `isHighlighted: false→true`, `isPrimary: true→false`, `isEnabled: true→false`. Naye values se classes recalculate: `btn` (always), `highlighted` (true ab), `btn-primary` (false ab, skip), `btn-disabled` (`!false=true`, apply). Final: **btn, highlighted, btn-disabled**.

---
---

# 🎯 Overall Quick Revision Summary

| Q.No | Topic | Key Concept |
|---|---|---|
| 111 | Exam Instructions | Subject confirmation (0 marks) |
| 112 | Vue Reactivity | Virtual DOM diffing + efficient patching |
| 113 | Vue Methods + Computed | Sequential state tracking (add/remove clicks) |
| 114 | `.map()` thisArg | Second argument sets callback's `this` |
| 115 | Vue 2 Reactivity Caveats | Array direct-index assignment NOT reactive |
| 116 | `var` in Loops + Closures | Shared variable, final value after loop ends |
| 117 | Computed Properties | Must be synchronous, `async` is INCORRECT |
| 118 | Method Chaining + Lookup | filter→map→sort→forEach, string/number key match |
| 119 | Nested `this` (Regular vs Arrow) | Call-site vs lexical-scope `this` |
| 120 | Vue Props | Literal prop value vs parent data (unrelated) |
| 121 | Closures (Module Pattern) | Independent instances, shared state within instance |
| 122 | `this` + `bind()`/`call()` | Bind is permanent, detached method loses `this` |
| 123 | `this` (Comprehensive) | Regular/Arrow methods, closures, constructor restriction |
| 124 | Vue Code Review | Sometimes "no bug" is the correct answer |
| 125 | Vue Best Practices | Moving inline logic to `methods` |
| 126 | Vue `:class` Array Syntax | Conditional class binding (initial state) |
| 127 | Vue `:class` Array Syntax | Conditional class binding (after toggle) |

---
last updated: 18 July 2026
