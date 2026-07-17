# 📘 App Dev 2 (MAD 2) — Quiz 1 (Sep 2025) PYQ Detailed Notes


# Question 162

## Original Question
> Which of the following Vue 2 template syntaxes are valid?
>
> Options:
> 1. `{{ message }}` for interpolation
> 2. `v-bind:title="msg"` for binding attributes
> 3. `v-on:@click="doSomething"` for click event binding
> 4. `<input v-model="inputValue"></input>` for two-way binding

(Multiple Select Question, Correct Marks: 2)

---

## Correct Answer
**Correct Options:** 1, 2, and 4

---

## Concept Used
- 📘 **Interpolation (`{{ }}`):** Ye Vue ka **"mustache" syntax** hai jo data property ki value ko HTML text content ke andar directly show karta hai.
- 📘 **`v-bind`:** Ye directive HTML **attributes** (jaise `title`, `src`, `class`) ko dynamically Vue data se bind karta hai. Shorthand: `:title="msg"`.
- 📘 **`v-on`:** Ye directive **event listeners** attach karta hai (jaise click, input). Shorthand: `@click="doSomething"`. **Important:** `v-on:` khud ek shorthand replacement hai `@` ka — dono ko **sath me use nahi kar sakte** (`v-on:@click` invalid syntax hai, ye redundant/wrong hai).
- 📘 **`v-model`:** Ye directive **two-way data binding** provide karta hai form inputs (jaise `<input>`, `<textarea>`, `<select>`) aur Vue data properties ke beech.

**Example:**
```html
<!-- Correct -->
<p>{{ message }}</p>
<img v-bind:src="imageUrl" />
<button v-on:click="handleClick">Click</button>
<input v-model="username" />

<!-- Incorrect -->
<button v-on:@click="handleClick">Click</button> <!-- v-on: aur @ dono ek sath galat hai -->
```

---

## Step-by-Step Solution
1. **Syntax 1: `{{ message }}`** → **Valid**. Ye standard interpolation syntax hai text content dikhane ke liye.
2. **Syntax 2: `v-bind:title="msg"`** → **Valid**. Ye correct tareeka hai attribute ko dynamically bind karne ka.
3. **Syntax 3: `v-on:@click="doSomething"`** → **Invalid**. `v-on:` khud "event listener" directive hai, aur `@` uska shorthand hai. Dono ko ek sath likhna galat hai — sahi syntax hoga ya to `v-on:click="doSomething"` ya `@click="doSomething"`, dono nahi.
4. **Syntax 4: `<input v-model="inputValue"></input>`** → **Valid**. `v-model` two-way binding ke liye correct directive hai form elements pe.

**Shortcut:** Jab bhi `v-on:@` ya `v-bind::` jaisa "double prefix" dikhe, samajh jao — ye **invalid syntax** hai, kyunki `@` aur `:` khud shorthand hain apne full directive names ke.

---

## Final Answer
**Valid Syntaxes: 1, 2, and 4**

---

## Why Other Options are Wrong?
### Syntax 3 (`v-on:@click`)
Wrong hai kyunki `@` symbol khud `v-on:` ka shorthand hai. Dono ko combine karna syntax error create karta hai — Vue engine isse valid directive ke roop me recognize nahi karega.

---

## Important Exam Notes
- ✅ `{{ }}` = text interpolation.
- ✅ `v-bind:attr` ya `:attr` = attribute binding.
- ✅ `v-on:event` ya `@event` = event binding.
- ✅ `v-model` = two-way binding (form elements).
- ⚠️ Common Mistake: Shorthand aur full syntax dono ek sath use karna (`v-on:@click`, `v-bind::title`).
- 💡 Trick: "Shorthand OR Full, never BOTH!"

---

## Similar Question Pattern
Vue directives ki syntax validity check karne wale MSQ questions common hain — dhyan rakhna hai shorthand vs full syntax ka.

---

## Revision Box
`{{ }}` text ke liye, `v-bind`/`:` attributes ke liye, `v-on`/`@` events ke liye, `v-model` two-way binding ke liye. Shorthand aur full syntax ko kabhi mix mat karo (`v-on:@click` galat hai).

---
---

# Question 163

## Original Question
> Which of the following statements about Vue 2 templates are correct?
>
> Options:
> 1. `v-if` removes elements from DOM when false
> 2. `v-show` toggle initiates re-render of the Vue component
> 3. `v-bind` can bind attributes and class dynamically
> 4. Vue 2 templates must use double curly braces for all text interpolation

(Multiple Select Question, Correct Marks: 2)

---

## Correct Answer
**Correct Options:** 1 and 3

---

## Concept Used
- 📘 **`v-if`:** Ye directive condition **false** hone par element ko **DOM se completely remove** kar deta hai (aur true hone par wapas add karta hai). Ye "**conditional rendering**" karta hai.
- 📘 **`v-show`:** Ye directive element ko DOM me hamesha **rakhta hai**, sirf uska CSS `display` property toggle karta hai (`display: none` vs `display: block`). Isme koi **re-render** nahi hota — sirf ek CSS style change hota hai, jo bahut **lightweight** operation hai.
- 📘 **`v-bind`:** Ye attributes (`src`, `href`, `title`) aur `class`/`style` dono ko dynamically Vue data se bind kar sakta hai.
- 📘 **Interpolation Options:** `{{ }}` sirf **text content** ke liye use hota hai. Attributes ke liye `v-bind` use hota hai, `{{ }}` attributes ke andar directly nahi likha ja sakta (jaise `<div title="{{ msg }}">` invalid hai — sahi hoga `<div :title="msg">`).

**Example:**
```html
<p v-if="isVisible">Ye element DOM se remove ho jayega agar isVisible false hai</p>
<p v-show="isVisible">Ye element DOM me rahega, sirf CSS display change hoga</p>
```

---

## Step-by-Step Solution
1. **Statement 1: "`v-if` removes elements from DOM when false"** → **True**. `v-if` false hone par element **DOM tree se hi hata deta hai** — ye "add/remove" logic follow karta hai.
   - *Reason:* `v-if` conditional rendering ka real (structural) toggle karta hai.
2. **Statement 2: "`v-show` toggle initiates re-render of the Vue component"** → **False**. `v-show` sirf CSS `display` property change karta hai, isme **koi re-render nahi hota** — element hamesha DOM me maujood rehta hai.
   - *Reason:* `v-show` ka poora purpose hi ye hai ki wo re-render avoid kare aur sirf visibility toggle kare — ye `v-if` se **cheaper (performance-wise)** hota hai jab bar-bar toggle karna ho.
3. **Statement 3: "`v-bind` can bind attributes and class dynamically"** → **True**. `v-bind` attributes ke sath-sath `class` aur `style` bhi dynamically bind kar sakta hai (jaise `:class="{ active: isActive }"`).
4. **Statement 4: "Vue 2 templates must use double curly braces for all text interpolation"** → **False**. `{{ }}` sirf text content ke liye hai. Attributes ke liye `v-bind` use hota hai — "for all" wala claim galat hai kyunki attributes me `{{ }}` use nahi hota.

---

## Final Answer
**Correct Statements: 1 and 3**

---

## Why Other Options are Wrong?
### Statement 2 (`v-show` causes re-render)
Wrong hai kyunki `v-show` sirf CSS `display` property toggle karta hai, element DOM me rehta hai — koi re-render/re-mount nahi hota.

### Statement 4 (double curly braces for ALL interpolation)
Wrong hai kyunki attributes ke liye `{{ }}` use hi nahi hota — unke liye `v-bind` (ya shorthand `:`) use hota hai.

---

## Important Exam Notes
- ✅ `v-if` = DOM se add/remove (structural toggle, expensive but true conditional rendering).
- ✅ `v-show` = sirf CSS `display` toggle (no DOM removal, no re-render, cheap for frequent toggling).
- ✅ `v-bind`/`:` = attributes, class, style dynamically bind karne ke liye.
- ⚠️ Common Mistake: `v-if` aur `v-show` ko same samajh lena — dono ka internal mechanism bilkul alag hai.
- 💡 Trick: "**v-if** = **i**n/out of DOM (heavy), **v-show** = **s**how/hide via CSS (light)"

---

## Similar Question Pattern
`v-if` vs `v-show` performance/behavior comparison questions bahut common hain Vue ke conceptual MCQs/MSQs me.

---

## Revision Box
`v-if` = element DOM se remove/add hota hai (true conditional rendering). `v-show` = element hamesha DOM me rehta hai, sirf CSS `display` toggle hota hai (no re-render). `v-bind` attributes + class + style sab bind kar sakta hai. `{{ }}` sirf text ke liye hai, attributes ke liye nahi.

---
---

# Question 164

## Original Question
```js
let employees = [
  { name: "Rahul", age: 28 },
  { name: "Priya", age: 24 },
  { name: "Amit", age: 32 }
];
employees.sort((a, b) => a.age - b.age);
console.log(employees.map(e => e.name).join(" - "));
```
> What will be printed on the console?
>
> Options: A. Rahul - Priya - Amit  B. Priya - Rahul - Amit  C. Amit - Rahul - Priya  D. Priya - Amit - Rahul

---

## Correct Answer
**Correct Option:** B (Priya - Rahul - Amit)

---

## Concept Used
- 📘 **`Array.prototype.sort()`:** Ye array ke elements ko **sort** karta hai using ek **comparator function**. Comparator function do elements `a` aur `b` leta hai aur ek number return karta hai:
  - Agar return value **negative** hai → `a` `b` se pehle aayega.
  - Agar return value **positive** hai → `b` `a` se pehle aayega.
  - Agar return value **0** hai → order same rehta hai (relative position unchanged).
- 📘 **`a.age - b.age`:** Ye numeric subtraction comparator hai jo **ascending order** (chhote se bade) me sort karta hai.
- 📘 **`.map()`:** Array ke har element ko transform karke ek **naya array** banata hai — yaha objects ko unke `name` property me convert kar raha hai.
- 📘 **`.join(" - ")`:** Array ke elements ko ek **string** me combine karta hai, given separator (`" - "`) ke sath.

**Example:**
```js
[5, 2, 8].sort((a, b) => a - b); // [2, 5, 8] (ascending)
[5, 2, 8].sort((a, b) => b - a); // [8, 5, 2] (descending)
```

---

## Step-by-Step Solution
1. **Step 1:** Original array: `[{Rahul,28}, {Priya,24}, {Amit,32}]`.
2. **Step 2:** `.sort((a, b) => a.age - b.age)` chalta hai — ye ages ko **ascending order** me sort karega.
   - Ages: Rahul(28), Priya(24), Amit(32) → ascending order: **24, 28, 32** → matlab **Priya, Rahul, Amit**.
   - *Reason:* Jab `a.age - b.age` negative hoga (matlab `a` chhota hai `b` se), `a` pehle aayega — ye classic ascending sort pattern hai.
3. **Step 3:** Sorted array: `[{Priya,24}, {Rahul,28}, {Amit,32}]`.
4. **Step 4:** `.map(e => e.name)` → `["Priya", "Rahul", "Amit"]` (sirf names extract kiye).
5. **Step 5:** `.join(" - ")` → `"Priya - Rahul - Amit"`.

**Shortcut:** `a.age - b.age` yaad rakho as "**a minus b = ascending**" (chhota pehle). `b.age - a.age` hota to descending (bada pehle) hota.

---

## Final Answer
**"Priya - Rahul - Amit"**

---

## Why Other Options are Wrong?
### Option A (Rahul - Priya - Amit)
Ye **original (unsorted)** order hai — is option me sorting ka effect hi consider nahi kiya gaya.

### Option C (Amit - Rahul - Priya)
Ye **descending order** (bade se chhota) hota agar comparator `b.age - a.age` hota — lekin diya gaya comparator ascending hai.

### Option D (Priya - Amit - Rahul)
Ye ek random/galat order hai jo na ascending hai na descending — comparator logic ko galat apply karne se aisi galti ho sakti hai.

---

## Important Exam Notes
- ✅ `a - b` comparator = Ascending order.
- ✅ `b - a` comparator = Descending order.
- ⚠️ Common Mistake: `.sort()` bina comparator ke use karna — default sort **lexicographic (string-based)** hota hai, numbers ke liye galat result de sakta hai (jaise `[10, 2, 1].sort()` → `[1, 10, 2]`, numeric order nahi).
- 💡 Trick: "**A**scending = **A**-minus-B" yaad rakho.

---

## Similar Question Pattern
Array `.sort()` with custom comparator + chaining `.map()`/`.join()` ke trace-output questions bahut common hain — dhyan rakhna hai comparator ka direction (ascending/descending).

---

## Revision Box
`sort((a,b) => a.age - b.age)` ascending order deta hai. Sorted by age: Priya(24) → Rahul(28) → Amit(32). `.map()` names nikalta hai, `.join(" - ")` unhe string bana deta hai: **"Priya - Rahul - Amit"**.

---
---

# Question 165

## Original Question
```js
const users = [
  { name: "Amit", age: 25 },
  { name: "Bhavna", age: 20 },
  { name: "Chirag", age: 30 },
];

const names = users
  .filter((u) => u.age >= 25)
  .map((u) => u.name.toUpperCase())
  .sort();

console.log(JSON.stringify(names));
```
> What is logged on the console?
>
> Options:
> A. `['Amit', 'Chirag']`
> B. `['CHIRAG', 'AMIT']`
> C. `['AMIT', 'CHIRAG']`
> D. `['AMIT', 'Bhavna', 'CHIRAG']`

---

## Correct Answer
**Correct Option:** C (`['AMIT', 'CHIRAG']`)

---

## Concept Used
- 📘 **Method Chaining:** JavaScript me array methods (`.filter()`, `.map()`, `.sort()`) ko **chain** kiya ja sakta hai — har method ek naya array return karta hai jispe agla method apply hota hai.
- 📘 **`.filter()`:** Ek naya array banata hai jisme sirf wahi elements hote hain jo given condition ko **true** satisfy karte hain.
- 📘 **`.map()`:** Har element ko transform karta hai — yaha `.toUpperCase()` se string ko **capital letters** me convert kiya ja raha hai.
- 📘 **`.sort()` (bina comparator ke):** Default JavaScript sort **lexicographic (dictionary/alphabetical) order** follow karta hai (strings ko unicode code point ke hisaab se compare karta hai) — numbers ke liye galat ho sakta hai, lekin strings ke liye ye theek se kaam karta hai.

**Example:**
```js
["Banana", "Apple", "Cherry"].sort(); // ["Apple", "Banana", "Cherry"] (alphabetical)
```

---

## Step-by-Step Solution
1. **Step 1:** Original array: `[{Amit,25}, {Bhavna,20}, {Chirag,30}]`.
2. **Step 2:** `.filter((u) => u.age >= 25)` — sirf wahi users rakho jinki age **25 ya usse zyada** ho.
   - Amit(25) → `25 >= 25` → **true** → included
   - Bhavna(20) → `20 >= 25` → **false** → excluded
   - Chirag(30) → `30 >= 25` → **true** → included
   - Result: `[{Amit,25}, {Chirag,30}]`
   - *Reason:* `.filter()` sirf condition-satisfying elements rakhta hai, baaki hata deta hai.
3. **Step 3:** `.map((u) => u.name.toUpperCase())` — har remaining user ka naam **uppercase** me convert karo.
   - `"Amit"` → `"AMIT"`
   - `"Chirag"` → `"CHIRAG"`
   - Result: `["AMIT", "CHIRAG"]`
   - *Reason:* `.toUpperCase()` string method hai jo sabhi letters ko capital bana deta hai.
4. **Step 4:** `.sort()` — bina comparator ke, default **alphabetical order** apply hota hai.
   - `"AMIT"` vs `"CHIRAG"` → alphabetically `A` `C` se pehle aata hai, isliye order already sahi hai: `["AMIT", "CHIRAG"]`.
   - *Reason:* Alphabet me "A" "C" se pehle aata hai, isliye koi swap nahi hota.
5. **Step 5:** `JSON.stringify(names)` → Array ko JSON string format me print karta hai: `["AMIT","CHIRAG"]` — jo dikhne me `['AMIT', 'CHIRAG']` jaisa hi hai.

**Shortcut:** Chaining questions me **ek-ek step alag se** likho (filter result, map result, sort result) — combine mat karo, galti hone ka chance kam hoga.

---

## Final Answer
**`['AMIT', 'CHIRAG']`**

---

## Why Other Options are Wrong?
### Option A (`['Amit', 'Chirag']`)
Galat hai kyunki `.map()` step me `.toUpperCase()` apply hua hai — original casing (`Amit`, `Chirag`) nahi rahegi, sab **CAPITAL** ho jayega.

### Option B (`['CHIRAG', 'AMIT']`)
Galat hai — ye order tab hota agar `.sort()` **reverse/descending** alphabetical order follow karta, lekin default `.sort()` **ascending (A→Z)** hota hai, isliye "AMIT" pehle aayega "CHIRAG" se.

### Option D (`['AMIT', 'Bhavna', 'CHIRAG']`)
Galat hai — ye option `.filter()` step ko hi ignore kar raha hai, jabki Bhavna (age 20) `>= 25` condition fail karti hai aur usse **exclude** hona chahiye tha.

---

## Important Exam Notes
- ✅ `.filter()` → condition satisfy karne wale elements rakhta hai.
- ✅ `.map()` → transform karta hai, original array modify nahi hota.
- ✅ `.sort()` bina comparator = default **string/lexicographic ascending** order.
- ⚠️ Common Mistake: `.sort()` bina comparator ke numbers pe use karna (result unexpected ho sakta hai), lekin strings ke liye ye theek kaam karta hai.
- 💡 Trick: Chaining ko step-by-step table me todo — Filter → Map → Sort — confusion kam hoga.

---

## Similar Question Pattern
`.filter().map().sort()` jaisi method chaining ke trace-output questions bahut common hain — inme har method ka result carefully track karna padta hai.

---

## Revision Box
`.filter(age>=25)` → Amit, Chirag rehte hain (Bhavna exclude). `.map(toUpperCase)` → "AMIT", "CHIRAG". `.sort()` (default alphabetical) → order same rehta hai kyunki A < C. Final: `['AMIT', 'CHIRAG']`.

---
---

# Question 166

## Original Question
```js
let subject = "Dog";

function camera() {
  let subject = "Deer";
  function click() {
    console.log(`clicked picture of ${subject}`);
  }
  subject = "Cat";
  return click;
}

let cameraPicture = camera();
cameraPicture();
```
> What is logged to the console?
>
> Options: A. clicked picture of Dog  B. clicked picture of Deer  C. clicked picture of Cat  D. clicked picture of undefined

---

## Correct Answer
**Correct Option:** C (clicked picture of Cat)

---

## Concept Used
- 📘 **Closures:** Ek inner function (`click`) apne outer function (`camera`) ke variables ko **"yaad"** rakhta hai — chahe outer function ka execution complete ho chuka ho. Ye link **reference** ke through hota hai, **value ke snapshot ke through nahi**.
- 📘 **IMPORTANT — Closures "live reference" rakhte hain, "copy" nahi:** Jab `click` function `subject` ko access karta hai, wo `camera` function ke **local `subject` variable ka current/latest value** dekhta hai — jo bhi value us waqt ho jab `click()` **actually call** ho, na ki jab `click` function **define** hua tha.
- 📘 **Variable Shadowing:** `camera()` ke andar `let subject = "Deer"` ek **naya local variable** banata hai jo outer `let subject = "Dog"` ko **shadow (chhupa)** kar deta hai.

**Example:**
```js
function outer() {
  let x = 1;
  function inner() { console.log(x); }
  x = 2; // x update hone ke baad bhi closure "latest" value dekhega
  return inner;
}
outer()(); // 2 (not 1!)
```

---

## Step-by-Step Solution
1. **Step 1:** `let subject = "Dog";` — Ye **global/outer** scope ka `subject` hai.
2. **Step 2:** `camera()` function call hota hai:
   - `let subject = "Deer";` — Ye camera function ke andar ek **naya local `subject`** banata hai (value `"Deer"`), jo outer wale `subject` ("Dog") ko **shadow** kar deta hai.
   - *Reason:* `let` ki wajah se ye local scope me confine hai, outer `subject` se koi lena dena nahi.
3. **Step 3:** `function click() { console.log(...) }` define hota hai — is function ke andar `subject` reference hai, jo **closure** ke through `camera()` ke local `subject` ko point karega (kyunki `click` `camera` ke andar hi defined hai — nearest scope rule).
   - *Reason:* Scope chain follow karte hue, `click` ke andar `subject` sabse pehle `camera`'s local `subject` me dhoondha jaayega — wo mil jaata hai, isliye outer/global `subject` tak jaane ki zaroorat nahi.
4. **Step 4:** `subject = "Cat";` — Ye `camera()` ke local `subject` ko **reassign** kar raha hai (Dog/Deer se) `"Cat"` me. Dhyan do — ye `click()` call hone se **pehle** ho raha hai.
   - *Reason:* Closures **latest value** dekhte hain, is line ke execute hone ke baad `subject` ki current value `"Cat"` ho chuki hai.
5. **Step 5:** `return click;` — `click` function (bina call kiye) return hota hai. `cameraPicture` ab is function ko refer karta hai.
6. **Step 6:** `cameraPicture();` — Ab `click` function **actually call** hota hai. Jab ye chalta hai, `subject` ki **current value** dekhta hai — jo already `"Cat"` set ho chuki thi (Step 4 me).
   - *Reason:* Closure ek **live link** hai variable ki taraf, koi "frozen snapshot" nahi — isliye latest value (`"Cat"`) hi milegi.
7. **Step 7:** Output: `"clicked picture of Cat"`.

**Shortcut:** Jab bhi function definition aur function call alag-alag lines pe ho, dhyan do — closure hamesha **call-time ki latest value** dekhta hai, definition-time ki nahi.

---

## Final Answer
**"clicked picture of Cat"**

---

## Why Other Options are Wrong?
### Option A (clicked picture of Dog)
Galat hai — ye outer/global `subject` hai. `click` function `camera()` ke andar defined hai, isliye scope chain rule se ye **local `subject` (Deer→Cat)** ko refer karega, global `subject` ("Dog") ko nahi.

### Option B (clicked picture of Deer)
Galat hai — ye ek common mistake hai jaha students sochte hain closure "definition-time" ki value capture kar leta hai (`"Deer"`, jab `click` function likha gaya). Lekin actually closure **reference** rakhta hai, aur call hone tak `subject` `"Cat"` me update ho chuka tha.

### Option D (clicked picture of undefined)
Galat hai — `subject` kabhi bhi `undefined` nahi hota is code me, kyunki ye pehle se hi `"Deer"` se initialize hua tha aur baad me `"Cat"` reassign hua — koi hoisting/TDZ issue yaha nahi hai.

---

## Important Exam Notes
- ✅ Closures **live reference** rakhte hain variable ki taraf, "value ka snapshot" nahi.
- ✅ Function call hone par closure **us waqt ki latest value** use karta hai, definition-time ki nahi.
- ⚠️ Common Mistake: Sochna ki closure function define hote hi value "freeze" kar leta hai — galat hai, jab tak explicitly (jaise IIFE se) snapshot na liya jaaye.
- 💡 Trick: "Closure = Live Camera Feed, not a Photograph" — hamesha current value dikhata hai.

---

## Similar Question Pattern
Closures + variable reassignment (before function call) ke trace-output questions bahut common hain — dhyan rakhna hai kab variable define hua, kab reassign hua, aur kab function call hua.

---

## Revision Box
`camera()` ke andar local `subject` "Deer"→"Cat" hota hai `click()` call hone se pehle hi. Closures live reference rakhte hain, isliye `cameraPicture()` call hone par latest value `"Cat"` milti hai — output: **"clicked picture of Cat"**.

---
---

# Question 167

## Original Question
> Match the following Vue 2 directives in Column A with their correct functionality in Column B:

| Column A | Column B |
|---|---|
| 1. v-bind | A. Creates two-way data binding between form inputs and Vue data properties |
| 2. v-model | B. Conditionally shows or hides elements using CSS display property |
| 3. v-on | C. Binds HTML attributes or component properties to Vue data expressions |
| 4. v-show | D. Attaches event listeners to DOM elements for handling user interactions |
| 5. v-cloak | E. Prevents the flash of un-compiled template content before Vue initializes |

> Options:
> A. 1-C, 2-D, 3-A, 4-E, 5-B
> B. 1-A, 2-C, 3-B, 4-D, 5-E
> C. 1-C, 2-A, 3-D, 4-B, 5-E
> D. 1-E, 2-A, 3-D, 4-C, 5-B

---

## Correct Answer
**Correct Option:** C (1-C, 2-A, 3-D, 4-B, 5-E)

---

## Concept Used
- 📘 **`v-bind`:** HTML attributes (jaise `src`, `title`, `class`) ko Vue data expressions se **dynamically bind** karta hai.
- 📘 **`v-model`:** Form input elements (`<input>`, `<select>`, `<textarea>`) aur Vue data ke beech **two-way data binding** create karta hai — matlab input change hone se data update hota hai, aur data change hone se input bhi update hota hai.
- 📘 **`v-on`:** DOM elements pe **event listeners** attach karta hai (jaise click, submit, keyup) taaki user interactions handle ho sakein.
- 📘 **`v-show`:** Element ko conditionally show/hide karta hai, lekin **DOM se remove nahi karta** — sirf CSS `display` property toggle karta hai.
- 📘 **`v-cloak`:** Ye ek special directive hai jo tab tak element ko **hidden** rakhta hai jab tak Vue instance apna compilation complete na kar le — isse "**flash of uncompiled template**" (jaha user ko `{{ message }}` jaisa raw text dikh jaata hai ek pal ke liye page load hote waqt) rokta hai.

---

## Step-by-Step Solution
1. **1. v-bind → C:** "Binds HTML attributes or component properties to Vue data expressions" — Ye directly `v-bind` ki definition hai.
2. **2. v-model → A:** "Creates two-way data binding between form inputs and Vue data properties" — `v-model` ka primary use hi ye hai forms ke liye.
3. **3. v-on → D:** "Attaches event listeners to DOM elements for handling user interactions" — `v-on` events handle karne ke liye hi banaya gaya hai.
4. **4. v-show → B:** "Conditionally shows or hides elements using CSS display property" — `v-show` sirf CSS `display: none/block` toggle karta hai.
5. **5. v-cloak → E:** "Prevents the flash of un-compiled template content before Vue initializes" — `v-cloak` specifically is problem ko solve karne ke liye design hua hai.

---

## Final Answer
**1-C, 2-A, 3-D, 4-B, 5-E**

---

## Why Other Options are Wrong?
### Option A (1-C, 2-D, 3-A, 4-E, 5-B)
`v-model` (2) ko "event listeners" (D) se match karna galat hai — `v-model` two-way binding ke liye hai, event listening uska primary purpose nahi. Similarly `v-on` (3) ko "two-way binding" (A) se match karna bhi galat hai.

### Option B (1-A, 2-C, 3-B, 4-D, 5-E)
`v-bind` (1) ko "two-way binding" (A) se match karna galat hai — `v-bind` **one-way** binding hai (data → attribute), two-way nahi. `v-model` (2) ko "attribute binding" (C) se match karna bhi galat hai.

### Option D (1-E, 2-A, 3-D, 4-C, 5-B)
`v-bind` (1) ko "flash prevention" (E, jo `v-cloak` ka kaam hai) se match karna completely galat hai. `v-show` (4) ko "attribute binding" (C) se match karna bhi galat hai.

---

## Important Exam Notes
- ✅ `v-bind` = one-way attribute binding.
- ✅ `v-model` = two-way binding (forms).
- ✅ `v-on` = event handling.
- ✅ `v-show` = CSS display toggle (conditional visibility, DOM me rehta hai).
- ✅ `v-cloak` = prevents flash of uncompiled `{{ }}` syntax.
- ⚠️ Common Mistake: `v-bind` aur `v-model` ko dono "binding" naam ki wajah se confuse kar dena — `v-bind` one-way hai, `v-model` two-way.
- 💡 Trick: "**M**odel = **M**utual (two-way), **B**ind = **B**asic (one-way)"

---

## Similar Question Pattern
Vue directives ki matching-based questions bahut common hain — har directive ka **primary distinguishing feature** yaad rakhna zaroori hai.

---

## Revision Box
`v-bind` (C) = attribute binding. `v-model` (A) = two-way form binding. `v-on` (D) = event listeners. `v-show` (B) = CSS display toggle. `v-cloak` (E) = flash-of-uncompiled-template prevention.

---
---

# Question 168

## Original Question
```html
<body>
    <div id="app">
        <p>Coffee</p>
        <button @click="orderCoffee">Order</button>
        <p>Total Cost: {{totalCost}}</p>
        <p>Order Count: {{orderCount}}</p>
    </div>
    <script>
        new Vue({
            el: '#app',
            data() {
                return {
                    totalCost: '0',
                    unitPrice: 25,
                    orderCount: 0
                }
            },
            methods: {
                orderCoffee() {
                    this.totalCost += this.unitPrice;
                    this.orderCount++;
                }
            }
        })
    </script>
</body>
```
> What will be displayed for "Total Cost" and "Order Count" when the Order button is clicked three times?
>
> Options:
> A. Total Cost: 75, Order Count: 3
> B. Total Cost: 02525, Order Count: 3
> C. Total Cost: 252525, Order Count: 3
> D. Total Cost: 0252525, Order Count: 3

---

## Correct Answer
**Correct Option:** D (Total Cost: 0252525, Order Count: 3)

---

## Concept Used
- 📘 **Data Type Trap — String vs Number:** `totalCost: '0'` **STRING** hai (quotes me), jabki `unitPrice: 25` ek **number** hai. Ye ek bahut important trap hai!
- 📘 **`+=` Operator with mixed types (String + Number):** Jab `+=` (jo `+` operator use karta hai internally) **string** aur **number** ke beech use hota hai, JavaScript **number ko string me convert** kar deta hai aur **concatenation (jodna)** karta hai, addition nahi.
  - `"0" + 25` → `"025"` (string concatenation, arithmetic addition nahi)
- 📘 **Contrast:** Agar `totalCost: 0` (bina quotes, number) hota, to `0 + 25 + 25 + 25 = 75` (normal arithmetic addition) hota.

**Example:**
```js
let a = "5";
a += 3; // "53" (string concatenation, NOT 8)

let b = 5;
b += 3; // 8 (normal number addition)
```

---

## Step-by-Step Solution
1. **Step 1:** Initial state: `totalCost = '0'` (STRING), `unitPrice = 25` (number), `orderCount = 0`.
   - *Reason:* Dhyan do `totalCost` quotes me hai (`'0'`), matlab ye ek **string** hai, number nahi!
2. **Step 2: 1st Click** — `orderCoffee()` call hota hai:
   - `this.totalCost += this.unitPrice` → `'0' + 25` → JavaScript `25` ko string `"25"` me convert karta hai aur **concatenate** karta hai → `'0' + '25' = '025'`.
   - `this.orderCount++` → `0 → 1`.
   - *Reason:* String `+` Number = string concatenation (number automatically string me convert hota hai).
3. **Step 3: 2nd Click:**
   - `this.totalCost += this.unitPrice` → `'025' + 25` → `'025' + '25' = '02525'`.
   - `this.orderCount++` → `1 → 2`.
4. **Step 4: 3rd Click:**
   - `this.totalCost += this.unitPrice` → `'02525' + 25` → `'02525' + '25' = '0252525'`.
   - `this.orderCount++` → `2 → 3`.
5. **Step 5:** Final displayed values: `Total Cost: 0252525`, `Order Count: 3`.

**Shortcut:** Jab bhi initial value **quotes me** dikhe (jaise `'0'`), turant flag karo "**ye string hai, isse hone wali +=  concatenation hogi, addition nahi**!"

---

## Final Answer
**"Total Cost: 0252525, Order Count: 3"**

---

## Why Other Options are Wrong?
### Option A (Total Cost: 75, Order Count: 3)
Ye tab sahi hota agar `totalCost` **number** hota (`totalCost: 0` bina quotes ke) — tab `0+25+25+25 = 75` normal arithmetic addition hoti. Lekin yaha `totalCost` **string** hai, isliye concatenation hoti hai.

### Option B (Total Cost: 02525, Order Count: 3)
Ye sirf **2 clicks** ke baad ki value hai (`'0'+25+25 = '02525'`), final (3 clicks) ki nahi — ek click miss ho gaya calculation me.

### Option C (Total Cost: 252525, Order Count: 3)
Ye galat hai — initial `'0'` ko completely ignore kar diya gaya, jabki wo starting value ka hissa hai aur concatenation me shamil hoga.

---

## Important Exam Notes
- ✅ String + Number (using `+` or `+=`) = **String Concatenation**, not addition.
- ✅ Number + Number = normal arithmetic addition.
- ⚠️ Common Mistake: Data type ko dhyan se na dekhna — quotes (`'0'`) vs bina quotes (`0`) me bahut fark padta hai.
- 💡 Trick: "String + Anything = String" — JavaScript hamesha string concatenation ki taraf jhukta hai jab ek operand string ho `+` operator ke saath.

---

## Similar Question Pattern
Data type coercion (implicit type conversion) ke sath Vue reactive data update wale trace-output questions common hain — dhyan rakhna hai initial data type (`'0'` vs `0`).

---

## Revision Box
`totalCost` STRING hai (`'0'`), `unitPrice` number hai (25). `+=` string ke sath concatenation karta hai, addition nahi. 3 clicks: `'0'→'025'→'02525'→'0252525'`. Final: **Total Cost: 0252525, Order Count: 3**.

---
---

# Question 169

## Original Question
```js
let result = "";
for (let i = 0; i < 2; i++) {
  for (var j = 0; j < 2; j++) {
    result += i + j;
  }
}
console.log(result);
```
> Which of the following statements is/are true?
>
> Options:
> A. The output is "0011".
> B. The inner loop runs 2 times for each outer loop iteration.
> C. The output is "0123"
> D. The values are concatenated into a string, not added as numbers.

(Multiple Select Question, Correct Marks: 3)

---

## Correct Answer
**Correct Options:** B and D

---

## Concept Used
- 📘 **Operator Precedence — `+=` with mixed operations:** `result += i + j` ka matlab hai `result = result + (i + j)`. Yaha **pehle `i + j` (dono numbers) calculate hota hai** (arithmetic addition, kyunki dono `i` aur `j` numbers hain), **uske baad** us result ko `result` (jo string hai) ke sath **concatenate** kiya jaata hai.
- 📘 **`for` loop mechanics:** `for (let i = 0; i < 2; i++)` — outer loop `i = 0, 1` (2 baar chalega). `for (var j = 0; j < 2; j++)` — inner loop **har outer iteration ke start me phir se `j=0` se shuru hota hai** aur `j = 0, 1` tak chalta hai (2 baar), chahe `var` use ho ya `let` — kyunki loop ka **init clause (`var j = 0`)** har baar outer loop body re-enter karne pe re-execute hota hai.
- 📘 **String Concatenation vs Number Addition:** `""` (empty string) + number = string concatenation shuru ho jaati hai us point se. Ek baar `result` string ban jaaye, uske baad `+=` hamesha **concatenation** karega (chahe right side `i+j` khud number ho).

---

## Step-by-Step Solution
1. **Step 1:** `let result = "";` — `result` ek **empty string** se shuru hota hai.
2. **Step 2: Outer loop i=0:**
   - **Inner loop j=0:** `result += i + j` → `i+j = 0+0 = 0` (pehle numeric addition hui, kyunki `i`,`j` dono numbers hain) → `result = "" + 0 = "0"`.
   - **Inner loop j=1:** `i+j = 0+1 = 1` → `result = "0" + 1 = "01"`.
   - Inner loop total **2 baar** chala (`j=0` aur `j=1`).
3. **Step 3: Outer loop i=1:**
   - **Inner loop j=0** (phir se 0 se shuru, kyunki `var j=0` init clause re-run hota hai): `i+j = 1+0 = 1` → `result = "01" + 1 = "011"`.
   - **Inner loop j=1:** `i+j = 1+1 = 2` → `result = "011" + 2 = "0112"`.
   - Inner loop is baar bhi **2 baar** chala.
4. **Step 4:** Final `result = "0112"`.

5. **Ab statements check karo:**
   - **Statement A: "output is '0011'"** → **False**. Actual output `"0112"` hai, `"0011"` nahi.
   - **Statement B: "inner loop runs 2 times for each outer loop iteration"** → **True**. Har outer iteration (`i=0` aur `i=1`) ke liye inner loop `j=0` se `j=1` tak, yaani **2 baar** chalta hai.
   - **Statement C: "output is '0123'"** → **False**. Actual output `"0112"` hai, `"0123"` nahi (ye tab hota agar values individually bina addition ke concatenate hoti — jaise `i` aur `j` alag-alag jodte, na ki `i+j` pehle calculate karke).
   - **Statement D: "values are concatenated into a string, not added as numbers"** → **True** (partially nuanced) — `i+j` khud **number addition** hai (kyunki `i`,`j` numbers hain), lekin us result ko `result` (string) me **concatenate** kiya jaata hai — overall operation string concatenation banti hai, pure numeric addition nahi (jaisa `0+1+1+2=4` hota agar sab kuch number hi rehta).

**Shortcut:** Jab bhi `result += i + j` jaisa expression dikhe jaha `result` empty string se shuru hota hai, dhyan do — `i+j` pehle **number ki tarah add** hota hai (parentheses jaisa priority), phir wo poora number **string me concatenate** hota hai.

---

## Final Answer
**Correct Statements: B and D** — Inner loop har outer iteration me 2 baar chalta hai, aur final values numbers ki tarah add hone ke bawajood string me concatenate hoti hain. Actual output = `"0112"`.

---

## Why Other Options are Wrong?
### Statement A (output "0011")
Galat hai — ye tab hota agar `i` aur `j` **individually** (bina `i+j` compute kiye) string me concatenate hote (jaise `result += i; result += j;` alag-alag) — lekin yaha `i+j` pehle ek number ban raha hai, phir wo concatenate ho raha hai.

### Statement C (output "0123")
Galat hai — ye output pattern tab milta agar loop **sequential counter** print kar raha hota (0,1,2,3), lekin yaha `i+j` ka combination different hai — `(0,0)→0, (0,1)→1, (1,0)→1, (1,1)→2` — dusra aur teesra value dono `1` hain, `"0123"` nahi ban sakta.

---

## Important Exam Notes
- ✅ `result += i + j` → pehle `i+j` numeric addition, phir string concatenation.
- ✅ Inner `for` loop ka init clause (`var j=0`) har outer iteration pe re-execute hota hai — inner loop hamesha `0` se restart hota hai (chahe `var` ho ya `let`).
- ⚠️ Common Mistake: Sochna ki `i+j` bhi string concatenation hi karega (jaise `"0"+"1"="01"`) — galat hai, `i` aur `j` dono abhi bhi **numbers** hain jab tak unhe empty string se add na kiya jaaye.
- 💡 Trick: Operator precedence yaad rakho — `a += b + c` hamesha matlab hai `a = a + (b+c)`, `b+c` pehle evaluate hota hai.

---

## Similar Question Pattern
Nested loops + string concatenation + operator precedence ke trace-output MSQ questions common hain — dhyan rakhna hai kaunsi operation pehle ho rahi hai (number addition ya string concat).

---

## Revision Box
Outer loop 2 baar chalta hai (`i=0,1`), har baar inner loop bhi 2 baar chalta hai (`j=0,1`, kyunki init clause re-runs). `i+j` pehle number ki tarah add hota hai, phir `result` string me concatenate hota hai. Final output: **"0112"**. Correct statements: B (inner loop 2x per outer) aur D (values string-concatenated).

---
---

# Question 170

## Original Question
```html
<button id="btn">Click Me</button>

<script>
    const btn = document.getElementById('btn');
    btn.addEventListener('click', () => console.log('A'));
    btn.onclick = () => console.log('B');
    btn.addEventListener('click', () => console.log('C'));
    btn.click();
</script>
```
> Among the following console log statements, which ones will be logged when the code executes? (Select **all** that apply, irrespective of the order.)
>
> Options: A. A  B. B  C. C  D. None

---

## Correct Answer
**Correct Options:** A, B, and C

---

## Concept Used
- 📘 **`addEventListener()`:** Ye method ek element pe **multiple event handlers** attach karne deta hai — sab handlers **independently** store hote hain aur jab event trigger hota hai, **saare** call hote hain (registration order me).
- 📘 **`element.onclick = fn`:** Ye ek **DOM property-based** event handler assignment hai. Ye `addEventListener` se **alag mechanism** hai — dono ek sath co-exist kar sakte hain, ek dusre ko **override/replace nahi karte** (jab tak `onclick` ko dusri baar assign na kiya jaaye, jo purane `onclick` ko replace karega).
- 📘 **Important:** `addEventListener` se register kiye gaye multiple listeners **kabhi ek dusre ko replace nahi karte** — sab saath rehte hain. Lekin `onclick = fn` agar **do baar** assign kiya jaaye, to dusra wala pehle wale ko **replace** kar dega (kyunki ye ek property hai, array nahi).
- 📘 **`element.click()`:** Ye programmatically **click event trigger** karta hai, jaise user ne khud click kiya ho — isse saare registered handlers (chahe `addEventListener` se ho ya `onclick` property se) **fire** hote hain.

**Example:**
```js
btn.addEventListener('click', () => console.log('First'));
btn.onclick = () => console.log('Second');
btn.addEventListener('click', () => console.log('Third'));
btn.click();
// Output: First, Second, Third (sab fire honge, koi replace nahi hua)
```

---

## Step-by-Step Solution
1. **Step 1:** `btn.addEventListener('click', () => console.log('A'));` — Ye **pehla listener** register hota hai jo `'A'` log karega.
   - *Reason:* `addEventListener` ek naya handler add karta hai, kisi ko replace nahi karta.
2. **Step 2:** `btn.onclick = () => console.log('B');` — Ye `onclick` **property** set kar raha hai jo `'B'` log karega.
   - *Reason:* Ye `addEventListener` se bilkul alag mechanism hai — dono **saath-saath** exist kar sakte hain, ek dusre ko touch nahi karte.
3. **Step 3:** `btn.addEventListener('click', () => console.log('C'));` — Ye **doosra listener** register hota hai jo `'C'` log karega.
   - *Reason:* Ye pehle wale `addEventListener` listener (A) ko replace **nahi** karta — dono independent hain, event trigger hone pe dono chalenge.
4. **Step 4:** `btn.click();` — Ye programmatically click event trigger karta hai. Is se **saare registered handlers fire honge**:
   - `addEventListener` wala pehla handler → `'A'`
   - `onclick` property wala handler → `'B'`
   - `addEventListener` wala doosra handler → `'C'`
5. **Step 5:** Isliye console pe `'A'`, `'B'`, aur `'C'` — **teeno** log honge (order me thoda variation ho sakta hai based on internal registration sequence, lekin question "irrespective of order" bol raha hai).

**Shortcut:** Yaad rakho — `addEventListener` **kabhi replace nahi karta**, `onclick =` **sirf khud ko replace karta hai (agar dusri baar assign ho)** — lekin `addEventListener` ko touch nahi karta. Dono mechanisms **parallel** chalte hain.

---

## Final Answer
**A, B, and C — teeno log honge.**

---

## Why Other Options are Wrong?
### Option D (None)
Galat hai — `btn.click()` explicitly call kiya gaya hai code me, jo saare registered handlers (chahe `addEventListener` se ho ya `onclick` se) ko **trigger** karta hai. Isliye kam se kam kuch to log hoga hi — "None" ka matlab hoga koi bhi handler trigger na ho, jo galat hai.

---

## Important Exam Notes
- ✅ `addEventListener` = multiple handlers ek event pe, sab independently fire hote hain.
- ✅ `onclick = fn` = single property-based handler, ye `addEventListener` ke saath co-exist karta hai.
- ✅ `element.click()` = programmatically event trigger karta hai, jaise real user click.
- ⚠️ Common Mistake: Sochna ki `onclick = fn` `addEventListener` se register kiye handlers ko **overwrite** kar dega — galat hai, dono alag mechanisms hain.
- 💡 Trick: "addEventListener = Multiple Friends, onclick = Single Property (but friends and property don't conflict)"

---

## Similar Question Pattern
Event handling mechanisms (`addEventListener` vs `onclick` property vs inline `onclick=""` attribute) ke combination wale trace-output questions common hain.

---

## Revision Box
`addEventListener` multiple listeners allow karta hai jo ek dusre ko replace nahi karte. `onclick` property ek alag mechanism hai jo `addEventListener` ke sath co-exist karta hai. `btn.click()` saare registered handlers ko trigger karta hai — isliye A, B, aur C teeno log honge.

---
---

# Question 171

## Original Question
**index.html:**
```html
<div id="app">
  <p v-for="n in numbers">{{ n }}</p>
</div>
<script src="app.js" />
```

**app.js:**
```js
var app = new Vue({
  el: "#app",
  data: { numbers: [1, 2, 3] },
});

app.data.numbers.push(4);
```
> How many `<p>` elements will be rendered? (Response Type: Numeric)

---

## Correct Answer
**Correct Answer:** `3`

---

## Concept Used
- 📘 **Vue Instance Data Exposure:** Jab hum `data: {...}` Vue constructor me pass karte hain, Vue us data object ko **reactive** bana kar uski properties ko **directly Vue instance pe proxy** kar deta hai. Matlab `numbers` array ko access karne ka sahi tarika hai `app.numbers` (seedha), ya `app.$data.numbers` (`$data` special property se).
- 📘 **`app.data` — Ye INVALID access hai!** Vue instance ke pass koi `.data` naam ki **direct property nahi hoti** (ye sirf constructor option ka naam tha, instance pe expose nahi hota is naam se). Isliye `app.data` → `undefined` hoga.
- 📘 **`undefined.numbers.push(4)`:** Kyunki `app.data` khud `undefined` hai, uske upar `.numbers` access karne ki koshish **runtime error (TypeError)** throw karegi: `"Cannot read properties of undefined (reading 'numbers')"`.
- 📘 **v-for Directive:** `v-for="n in numbers"` array ke har element ke liye ek `<p>` element render karta hai — array ki **length** jitne `<p>` tags banenge.

**Example (Correct way to access reactive data):**
```js
app.numbers.push(4);      // ✅ Correct — reactive update hoga
app.$data.numbers.push(4); // ✅ Correct — $data special property hai
app.data.numbers.push(4);  // ❌ Wrong — 'data' Vue instance pe exist nahi karta
```

---

## Step-by-Step Solution
1. **Step 1:** Vue instance banti hai `numbers: [1, 2, 3]` ke saath — matlab initial array me **3 elements** hain.
2. **Step 2:** Component **mount** hota hai, aur `v-for="n in numbers"` template render karta hai — is waqt `numbers = [1, 2, 3]`, isliye **3 `<p>` elements** banate hain (values 1, 2, 3 dikhate hue).
   - *Reason:* Mounting ke time jo bhi array ki current state hoti hai, usi ke hisaab se DOM elements create hote hain.
3. **Step 3:** `app.data.numbers.push(4);` line execute hoti hai — lekin **`app.data` khud `undefined` hai** (Vue instance pe koi `.data` naam ki property expose nahi hoti).
   - *Reason:* Vue apna data object seedha instance pe merge kar deta hai (`app.numbers` ke through directly accessible), `app.data` naam ka koi wrapper object nahi banaya jaata.
4. **Step 4:** Isliye `app.data.numbers` access karne ki koshish karte hi — `undefined.numbers` — JavaScript **TypeError** throw kar degi: "Cannot read properties of undefined".
   - *Reason:* Jab kisi `undefined` value pe property access karne ki koshish hoti hai, JS runtime turant error throw karta hai.
5. **Step 5:** Ye error is line pe hi script ko **rok deta hai** — `push(4)` kabhi execute hi nahi hota, isliye `numbers` array **abhi bhi `[1,2,3]`** rehta hai (4 add hi nahi hua).
6. **Step 6:** Kyunki `push` fail ho gaya (error ki wajah se), reactive array update trigger hi nahi hua — DOM me koi naya `<p>` element add nahi hoga. Jo already render ho chuka tha (3 elements), wahi rahega.

**Shortcut:** Jab bhi `app.data.xyz` jaisa access dikhe (Vue instance ke context me), turant flag karo — "**ye galat hai, Vue instance pe `.data` naam ki koi property nahi hoti**" — sahi tareeka hai `app.xyz` ya `app.$data.xyz`.

---

## Final Answer
**3 `<p>` elements** render honge.

---

## Why Other Options are Wrong?
Ye Numeric Short Answer Question hai, lekin common **galat answers**:

### "4"
Galat hai — students soch sakte hain ki `push(4)` successfully chal gaya aur array me 4th element add ho gaya, jabki actually `app.data` hi `undefined` hone ki wajah se poora `push` statement **error throw karke fail** ho jaata hai, kabhi execute hi nahi hota.

### "0"
Galat hai — initial mounting ke time array `[1,2,3]` already valid hai, isliye 3 `<p>` elements to render honge hi hone chahiye, error baad me aata hai.

---

## Important Exam Notes
- ✅ Vue reactive data ko access karne ka sahi tarika: `app.propertyName` ya `app.$data.propertyName`.
- ✅ `app.data` (bina `$` ke) **INVALID** hai Vue instance ke liye — ye `undefined` hoga.
- ⚠️ Common Mistake: `app.data` ko sahi access samajh lena — bahut common confusion hai kyunki constructor option ka naam `data` hota hai, lekin instance pe wo directly expose nahi hota isi naam se.
- 💡 Trick: "**$data** = safe access, **data** (without $) = undefined trap!"

---

## Similar Question Pattern
Vue instance ki internal data structure (`$data`, `$el`, `$options`) ke access-related trace-output questions common hain — dhyan rakhna hai kaunse property names actually valid hain.

---

## Revision Box
Vue instance pe `.data` naam ki koi property expose nahi hoti — sahi access `app.numbers` ya `app.$data.numbers` hai. `app.data.numbers.push(4)` error throw karta hai (`app.data` = undefined), isliye push kabhi hota hi nahi. Initial 3 elements (`[1,2,3]`) hi render hote hain — Answer: **3**.

---
---

# Question 172

## Original Question
```js
const products = [
  { name: 'Laptop', price: 1200, category: 'Electronics', inStock: true },
  { name: 'Book', price: 25, category: 'Education', inStock: false },
  { name: 'Phone', price: 800, category: 'Electronics', inStock: true },
  { name: 'Pen', price: 5, category: 'Stationery', inStock: true },
  { name: 'Tablet', price: 600, category: 'Electronics', inStock: false }
];

const result = products
  .filter(item => item.category === 'Electronics' && item.inStock)
  .map(item => ({ ...item, discountedPrice: item.price * 0.9 }))
  .filter(item => item.discountedPrice > 500);

console.log(result.length);
console.log(result[0].name);
```
> What will be the output of the above code on the browser's console?
>
> Options:
> A. `3` / `Laptop`  B. `2` / `Laptop`  C. `1` / `Phone`  D. `2` / `Phone`

---

## Correct Answer
**Correct Option:** B (`2` / `Laptop`)

---

## Concept Used
- 📘 **Multi-step Method Chaining:** Is question me `.filter()` → `.map()` → `.filter()` ka **teen-step chain** hai. Har step ka output agle step ka input banta hai.
- 📘 **Spread Operator (`...item`):** `{ ...item, discountedPrice: ... }` — Ye original object ke **saare properties copy** karta hai naye object me, aur ek **naya property** (`discountedPrice`) add karta hai. Ye original object ko modify nahi karta, ek **naya object** banata hai (immutability pattern).
- 📘 **Logical AND (`&&`) in filter condition:** `item.category === 'Electronics' && item.inStock` — dono conditions **true** honi chahiye tabhi element filter me pass hoga.

---

## Step-by-Step Solution
1. **Step 1: Original array (5 products):**
   | Name | Price | Category | inStock |
   |---|---|---|---|
   | Laptop | 1200 | Electronics | true |
   | Book | 25 | Education | false |
   | Phone | 800 | Electronics | true |
   | Pen | 5 | Stationery | true |
   | Tablet | 600 | Electronics | false |

2. **Step 2: `.filter(item => item.category === 'Electronics' && item.inStock)`** — sirf wahi products rakho jo **Electronics** category ke hon **AND** stock me hon:
   - Laptop: Electronics ✅ + inStock true ✅ → **included**
   - Book: Education ❌ → excluded
   - Phone: Electronics ✅ + inStock true ✅ → **included**
   - Pen: Stationery ❌ → excluded
   - Tablet: Electronics ✅ but inStock **false** ❌ → **excluded** (dono conditions true honi chahiye thi)
   - Result: `[Laptop(1200), Phone(800)]`
   - *Reason:* Tablet Electronics hai lekin stock me nahi hai, isliye `&&` condition fail karti hai.

3. **Step 3: `.map(item => ({ ...item, discountedPrice: item.price * 0.9 }))`** — har remaining product me `discountedPrice` naam ka naya field add karo (price ka 90%):
   - Laptop: `discountedPrice = 1200 * 0.9 = 1080`
   - Phone: `discountedPrice = 800 * 0.9 = 720`
   - Result: `[{Laptop, discountedPrice:1080}, {Phone, discountedPrice:720}]`
   - *Reason:* `.map()` original array ko modify nahi karta, naya array with extra field banata hai.

4. **Step 4: `.filter(item => item.discountedPrice > 500)`** — sirf wahi products rakho jinki discountedPrice **500 se zyada** ho:
   - Laptop: `1080 > 500` → **true** → included
   - Phone: `720 > 500` → **true** → included
   - Result: `[Laptop, Phone]` — dono qualify karte hain.
   - *Reason:* Dono products ki discounted price already 500 se kaafi zyada hai.

5. **Step 5:** `result.length` → **2** (Laptop aur Phone dono hain final result me).
6. **Step 6:** `result[0].name` → Array ka **pehla element** hamesha wahi rehta hai jo filter/map ke baad **original relative order** maintain karta hai — Laptop original array me Phone se **pehle** tha, isliye `result[0]` = **"Laptop"**.
   - *Reason:* `.filter()` aur `.map()` dono **order preserve** karte hain — wo elements ko reorder nahi karte, sirf remove/transform karte hain.

**Shortcut:** Method chaining questions me **table bana kar** har step ka result track karo — filter step, map step, filter step — confusion nahi hoga.

---

## Final Answer
**`2` (length) aur `Laptop` (result[0].name)**

---

## Why Other Options are Wrong?
### Option A (`3` / `Laptop`)
Galat hai — length `3` tab hota agar Tablet bhi qualify kar jaata (uska `inStock: false` hone ki wajah se wo pehle hi filter step me exclude ho jaata hai), ya koi extra product count ho jaata — actual valid count sirf 2 hai (Laptop, Phone).

### Option C (`1` / `Phone`)
Galat hai — length `1` galat hai kyunki dono Laptop aur Phone `discountedPrice > 500` condition satisfy karte hain (1080 aur 720 dono 500 se bade hain). Aur `result[0]` "Phone" hona bhi galat hai — order **Laptop pehle** hi rehta hai (original array order preserve hota hai).

### Option D (`2` / `Phone`)
Length `2` sahi hai, lekin `result[0].name` "Phone" galat hai — Laptop original array me pehle position pe tha, filter/map methods element ka **order badalte nahi**, isliye Laptop hi `result[0]` rahega, Phone nahi.

---

## Important Exam Notes
- ✅ `.filter()` aur `.map()` dono **order preserve** karte hain (elements ko reorder nahi karte).
- ✅ Spread operator (`{...item, newProp: value}`) original object copy karke naya field add karta hai — **immutable update pattern**.
- ✅ Logical `&&` dono conditions true honi chahiye tabhi pass hoga.
- ⚠️ Common Mistake: Ye assume karna ki koi bhi transformation array ka order badal sakta hai — `.filter()`/`.map()` order maintain karte hain, sirf `.sort()` order change karta hai.
- 💡 Trick: "Filter/Map = Order Preserved, Sort = Order Changed"

---

## Similar Question Pattern
Multi-step chaining (`filter→map→filter`) with spread operator ke trace-output questions common hain, especially real-world data processing scenarios (e-commerce, filtering datasets) ke context me.

---

## Revision Box
`filter(Electronics && inStock)` → `[Laptop, Phone]` (Tablet exclude hua kyunki stock me nahi). `map(add discountedPrice)` → dono ki discounted price calculate hui (1080, 720). `filter(discountedPrice>500)` → dono qualify karte hain. Final: `length=2`, `result[0].name="Laptop"` (order preserved).

---
---

# Question 173

## Original Question
```js
const globalVar = 50;

const object1 = {
    globalVar: 100,
    method1: function() {
        console.log(globalVar, this.globalVar);
        return () => {
            console.log(globalVar, this.globalVar);
        }
    }
}

const object2 = {
    globalVar: 200,
    method2: () => {
        console.log(globalVar, this.globalVar);
        return function() {
            console.log(globalVar, this.globalVar);
        }
    }
}

const fn1 = object1.method1();
fn1();
const fn2 = object2.method2();
fn2();
```
> What will be the output of the above program on the browser's console, if executed?

---

## Correct Answer
**Correct Output:**
```
50 100
50 100
50 undefined
50 undefined
```

---

## Concept Used
- 📘 **`globalVar` (outer const) vs `this.globalVar` (object property):** Code me `globalVar` (bina `this.`) hamesha **outer/global scope ka `const globalVar = 50`** ko refer karega — object ke andar `globalVar: 100` sirf ek **property** hai, koi naya `globalVar` variable nahi bana raha (ye JS variable scoping nahi hai, ye object property hai).
- 📘 **Regular Function `this` binding:** Regular function (`function() {...}`) ka `this` us object pe depend karta hai jispe use **call** kiya gaya — `object1.method1()` call hone se `this = object1`.
- 📘 **Arrow Function `this` binding:** Arrow function ka apna `this` nahi hota — wo apne **defining/enclosing lexical scope** ka `this` "inherit" karta hai.
  - `object1.method1` ke andar defined arrow function → uska `this` = `method1`'s `this` (jo `object1` hai, kyunki `method1()` `object1.` ke through call hua).
  - `object2.method2` khud ek **arrow function** hai (top-level pe define hua, object property ki tarah) — iska `this` object2 ka nahi hoga, balki **top-level/module scope ka `this`** hoga (jo browser script me `window`/undefined ho sakta hai depending on strict mode).

---

## Step-by-Step Solution
1. **Step 1:** `const fn1 = object1.method1();` — `method1()` **call** hota hai `object1.` ke through, isliye is regular function ke andar `this = object1`.
   - **Line A:** `console.log(globalVar, this.globalVar);`
     - `globalVar` → outer scope ka **50** (object property se koi relation nahi).
     - `this.globalVar` → `this = object1`, isliye `object1.globalVar = 100`.
     - **Output: `50 100`**
   - `method1` ek **arrow function return** karta hai. Arrow function ka `this` = enclosing scope ka `this` = `method1`'s `this` = `object1` (kyunki arrow apna `this` nahi banata, method1 ka `this` "borrow" karta hai).
   - `fn1` ab is arrow function ko refer karta hai.

2. **Step 2:** `fn1();` — Arrow function call hoti hai. Chunki arrow function ka `this` **already fix** hai (`object1`, jo method1 se inherit hua tha define hone ke waqt), call kaise bhi ho, `this` same rahega.
   - **Line B (arrow function ke andar):** `console.log(globalVar, this.globalVar);`
     - `globalVar` → outer **50** (same as before).
     - `this.globalVar` → `this = object1` (fixed), isliye `object1.globalVar = 100`.
     - **Output: `50 100`**

3. **Step 3:** `const fn2 = object2.method2();` — `method2` khud ek **arrow function** hai (object property ke roop me define hui). Arrow function ka `this` uske **defining lexical scope** se aata hai — chunki `method2` top-level pe (kisi function ke andar nahi) define hui hai, iska `this` = **top-level/global `this`** (object2 ka nahi!).
   - Top-level `this` browser script me generally `window` object hota hai (non-module, non-strict context) — lekin `window.globalVar` **exist nahi karta** (kyunki top-level `const globalVar` window pe attach nahi hota — sirf `var` attach hota hai).
   - **Line C:** `console.log(globalVar, this.globalVar);`
     - `globalVar` → outer **50**.
     - `this.globalVar` → `this` = top-level this (window, jispe `globalVar` property nahi hai) → **undefined**.
     - **Output: `50 undefined`**
   - `method2` ek **regular function** return karta hai. Is regular function ka `this` abhi **decide nahi hua** — wo call hone ke tarike pe depend karega.
   - `fn2` ab is regular function ko refer karta hai.

4. **Step 4:** `fn2();` — Regular function **plain call** hoti hai (`object2.` ke through nahi, seedha `fn2()`). Plain call me regular function ka `this` = **global object (`window`)** (non-strict mode me) ya **`undefined`** (strict mode me) — dono cases me `window.globalVar` ya `undefined.globalVar` effectively **`globalVar` property nahi milegi**.
   - **Line D (regular function ke andar):** `console.log(globalVar, this.globalVar);`
     - `globalVar` → outer **50**.
     - `this.globalVar` → `this` = window (jispe globalVar property nahi hai, kyunki top-level const attach nahi hota) → **undefined**.
     - **Output: `50 undefined`**

**Final combined output:**
```
50 100
50 100
50 undefined
50 undefined
```

**Shortcut:** Jab bhi arrow function object property ki tarah define ho (`method2: () => {...}`), turant flag karo — "**iska `this` object ka nahi, balki jaha ye likha gaya hai uss lexical/enclosing scope ka hoga**" — ye ek bahut common exam trap hai!

---

## Final Answer
```
50 100
50 100
50 undefined
50 undefined
```

---

## Why Other Options are Wrong?
### Option (50 100 / 50 100 / 50 undefined / 50 200)
Galat hai — last line `50 200` galat hai. Ye tab hota agar `fn2` ka `this` kisi tarah `object2` ko refer kar raha hota — lekin `fn2` ek **regular function hai jo plain call ho raha hai** (`fn2()`, na ki `object2.fn2()`), isliye uska `this` object2 nahi ban sakta.

### Option (100 50 / 100 50 / 200 50 / 200 50)
Galat hai — ye poora reversed hai (`this.globalVar` pehle, `globalVar` baad me), jo code me actual order se match nahi karta. Bhi, values bhi galat calculate hui hain — `globalVar` (bina this) hamesha outer `50` hi rahega, kabhi `100`/`200` nahi ban sakta.

### Option (50 100 / 50 100 / 50 undefined / 50 this)
Galat hai — `this.globalVar` print karne se literal shabd `"this"` kabhi print nahi hoga — ya to `this.globalVar` ki actual value milegi (jo yaha `undefined` hai) ya koi valid value.

---

## Important Exam Notes
- ✅ Regular function ka `this` = **call-site** pe depend karta hai (`obj.method()` → `this=obj`; plain call → `this=window/undefined`).
- ✅ Arrow function ka `this` = **definition-site (lexical scope)** pe depend karta hai — kabhi bhi call-site se change nahi hota.
- ✅ Object property ke roop me likha gaya `const globalVar` top-level pe **`window` object pe attach nahi hota** (sirf `var` attach hota hai) — isliye `this.globalVar` (jab `this=window`) hamesha `undefined` milega.
- ⚠️ Common Mistake: Arrow function ko "object method" samajh kar uska `this` object maan lena — galat hai agar arrow function **object literal ke andar directly property ki tarah** likhi gayi ho.
- 💡 Trick: "Arrow function **jaha likha gaya** uska `this` leta hai, **jaha call hua** wahan ka nahi!"

---

## Similar Question Pattern
`this` binding ke saath regular vs arrow functions ke combination, object methods, aur returned functions ke trace-output questions high-mark (4-5 marks) me common hain — inme patience se `this` ko track karna padta hai har function level pe.

---

## Revision Box
`method1` (regular fn) → `this=object1` (call-site based). Returned arrow function bhi `this=object1` use karti hai (lexically inherited from method1). `method2` (arrow property) → `this` = top-level scope ka `this` (object2 nahi!), isliye `this.globalVar=undefined`. Returned regular function (`fn2`) → plain call se `this=window`, phir se `undefined`. Output: `50 100 / 50 100 / 50 undefined / 50 undefined`.

---
---

# Question 174

## Original Question
```js
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
    super(region);
    this.name = name;
  }
  describe() {
    return `${this.name} is in ${this.region}.`;
  }
}

let r = new Region("Asia");
let c = new Country("Europe", "Germany");
let f = c.describe;
```
> Which of the following statements are **true**?
>
> Options:
> A. Calling `r.describe()` prints "Asia is a region."
> B. Calling `c.describe()` prints "Germany is in Europe."
> C. Calling `f()` directly will still print "Germany is in Europe."
> D. The `Country` class overrides both the `constructor` and the `describe()` method from `Region`.
> E. The call to `super(region)` inside `Country`'s constructor is not required to initialize `this.region`.

(Multiple Select Question, Correct Marks: 4.5)

---

## Correct Answer
**Correct Options:** A, B, and D

---

## Concept Used
- 📘 **Class Inheritance (`extends`):** `Country` class `Region` ko extend karti hai — matlab `Country` ki instances ko `Region` ke properties/methods bhi **inherit** hote hain, jab tak `Country` unhe explicitly **override** na kare.
- 📘 **Method Overriding:** `Country` apna khud ka `describe()` method define karti hai jo `Region` ke `describe()` ko **completely replace/override** kar deta hai `Country` instances ke liye.
- 📘 **`super(region)`:** Ye parent class (`Region`) ka **constructor call** karta hai — is call ke through hi `this.region = region` set hota hai (jo `Region`'s constructor me likha hai). Derived (child) class me `this` use karne se **pehle** `super()` call karna **mandatory** hai — bina `super()` call kiye, JavaScript **ReferenceError** throw karega agar `this` use kiya jaaye constructor me.
- 📘 **Detached Method Call (`let f = c.describe;`):** Jab kisi method ko object se "detach" karke ek variable me store kiya jaata hai (bina call kiye), aur baad me **plain function** ki tarah call kiya jaata hai (`f()`), to `this` binding **lost** ho jaati hai — class methods strict mode me hote hain, isliye `this = undefined` hoga plain call me, jo `this.name`/`this.region` access karte hi **TypeError** throw karega.

---

## Step-by-Step Solution
1. **Statement A: `r.describe()` prints "Asia is a region."**
   - `r` `Region` class ka instance hai, `r.region = "Asia"` (constructor se set hua).
   - `r.describe()` → `Region`'s `describe()` chalega (kyunki `r` `Country` ka instance nahi hai) → `` `${this.region} is a region.` `` → `"Asia is a region."`
   - **TRUE** ✅

2. **Statement B: `c.describe()` prints "Germany is in Europe."**
   - `c` `Country` class ka instance hai. Constructor me `super("Europe")` call hota hai (jo `this.region = "Europe"` set karta hai), phir `this.name = "Germany"` set hota hai.
   - `c.describe()` → `Country`'s **overridden** `describe()` chalega (na ki `Region`'s) → `` `${this.name} is in ${this.region}.` `` → `"Germany is in Europe."`
   - **TRUE** ✅
   - *Reason:* Method overriding ki wajah se `Country` ka apna `describe()` priority leta hai.

3. **Statement C: `f()` directly will still print "Germany is in Europe."**
   - `let f = c.describe;` — Ye `describe` method ka **reference** store kar raha hai, bina call kiye, aur bina `c` ke context ke.
   - Jab `f()` **plain function** ki tarah call hoga (bina `c.` ke through), `this` binding **lost** ho jaati hai. Class methods automatically **strict mode** me hote hain, isliye plain call pe `this = undefined`.
   - Function body `this.name` access karne ki koshish karega → `undefined.name` → **TypeError: Cannot read properties of undefined**.
   - **FALSE** ❌ — Ye print **nahi** karega "Germany is in Europe.", balki **error throw** karega.

4. **Statement D: `Country` class overrides both `constructor` and `describe()` method from `Region`.**
   - `Country` apna khud ka `constructor(region, name) {...}` define karti hai (jo `super()` call karta hai plus extra `name` set karta hai) — ye `Region`'s default constructor se **different/overridden** hai.
   - `Country` apna khud ka `describe() {...}` bhi define karti hai jo `Region`'s `describe()` ko **override** karta hai.
   - **TRUE** ✅ — Dono (constructor aur describe) `Country` me overridden hain.

5. **Statement E: `super(region)` is not required to initialize `this.region`.**
   - Ye **galat** hai — `super(region)` hi wo call hai jo parent (`Region`) ke constructor ko trigger karta hai, jisme `this.region = region;` likha hua hai.
   - Agar `super(region)` call **na kiya jaaye**, to `Region`'s constructor kabhi chalega hi nahi, aur `this.region` set hi nahi hoga.
   - Bhi, JavaScript ka rule hai — derived class ke constructor me `this` use karne se **pehle** `super()` call karna **mandatory** hai, warna `ReferenceError` aata hai.
   - **FALSE** ❌ — `super(region)` **zaroori** hai `this.region` initialize karne ke liye.

---

## Final Answer
**A, B, and D are TRUE; C and E are FALSE.**

---

## Why Other Options are Wrong?
### Option C
`f()` detached call hai — `this` binding lost ho jaati hai, isliye "Germany is in Europe" print **nahi** hoga, balki `this.name`/`this.region` access karte hi **TypeError** throw hoga.

### Option E
`super(region)` hi wo mechanism hai jiske through parent constructor ka `this.region = region` logic chalta hai. Isko skip karna galat hoga — `this.region` initialize hi nahi hoga (aur `this` use karna bhi ReferenceError degа bina super() ke).

---

## Important Exam Notes
- ✅ Child class apna `constructor`/`describe()` define karke parent class ke corresponding members ko **override** kar sakti hai.
- ✅ `super(args)` parent class ka constructor call karta hai — derived class me `this` use karne se pehle **mandatory** hai.
- ✅ Method ko object se "detach" karke plain call karne se `this` binding **lost** ho jaati hai → TypeError.
- ⚠️ Common Mistake: Sochna ki detached method call automatically original object ka context "yaad" rakhega — galat hai, `this` call-site pe decide hota hai (regular functions/methods ke liye).
- 💡 Trick: "Detached method = Lost `this`, unless explicitly bound with `.bind()`"

---

## Similar Question Pattern
Class inheritance + method overriding + `super()` requirement + detached method calls ke combination wale high-mark MSQ questions common hain — inme dhyan se check karna hota hai kaunsa method kis class se chal raha hai, aur `this` binding kaha lost ho rahi hai.

---

## Revision Box
`r.describe()` = "Asia is a region." (base class method). `c.describe()` = "Germany is in Europe." (overridden method in Country). `f()` (detached call) = TypeError (this binding lost), NOT "Germany is in Europe.". `Country` overrides both constructor and describe() — TRUE. `super(region)` IS required to set `this.region` — Statement E is FALSE.

---
---

# Question 175

## Original Question
> Consider the following JavaScript function definitions. Which of the following will correctly return the product of three numbers and handle the case where any parameter is undefined by treating it as 1?
>
> Options:
>
> **A.**
> ```js
> let multiply = (a, b, c) => {
>     a = a || 1;
>     b = b || 1;
>     c = c || 1;
>     return a * b * c;
> }
> ```
>
> **B.**
> ```js
> function multiply(a = 1, b = 1, c = 1) {
>     return a * b * c;
> }
> ```
>
> **C.**
> ```js
> let multiply = (a, b, c) => {
>     return (a ?? 1) * (b ?? 1) * (c ?? 1);
> }
> ```
>
> **D.**
> ```js
> const multiply = function(a, b, c) {
>     if(a === undefined) a = 1;
>     if(b === undefined) b = 1;
>     if(c === undefined) c = 1;
>     a * b * c;
> }
> ```

(Multiple Select Question, Correct Marks: 4.5)

---

## Correct Answer
**Correct Options:** B and C

---

## Concept Used
- 📘 **Default Parameters (`function(a = 1)`):** Ye ek **built-in** JavaScript feature hai jo automatically parameter ki value `1` set kar deta hai **agar aur sirf agar** argument `undefined` pass kiya jaaye (ya bilkul na diya jaaye). Ye specifically `undefined` ko target karta hai.
- 📘 **Logical OR (`||`) — GALAT approach for this case:** `a || 1` — Ye tab `1` return karega jab `a` koi bhi **falsy value** ho, na ki sirf `undefined`. Falsy values me shamil hain: `undefined`, `null`, `0`, `""`, `NaN`, `false`. Isliye agar koi legitimate `0` pass kare (jaise `multiply(0, 5, 5)`), to `0 || 1` galti se `1` bana dega — jo **bug** hai.
- 📘 **Nullish Coalescing (`??`) — SAHI approach:** `a ?? 1` — Ye sirf tab `1` return karta hai jab `a` **`null` ya `undefined`** ho — baaki saare falsy values (`0`, `""`, `false`) ko **as-is respect** karta hai. Ye `||` se zyada **precise** hai jab specifically "undefined/null" handle karna ho.
- 📘 **Missing `return` statement — Common Bug:** Agar function ke andar `a * b * c;` sirf ek statement ki tarah likha ho **bina `return` keyword ke**, to function implicitly `undefined` return karega — calculation to hoga, lekin uska result **kahi store/return nahi hoga**.

---

## Step-by-Step Solution
1. **Option A Analysis:** `a = a || 1;`
   - Ye **sirf undefined nahi**, balki saare **falsy values** (`0`, `""`, `NaN`, `false`, `null`) ko bhi `1` se replace kar dega.
   - Problem: Agar koi `multiply(0, 5, 5)` call kare (jaha `0` ek **legitimate/valid number** hai), to `0 || 1` → `1` ban jayega — jo **galat behavior** hai (question specifically "undefined" handle karne ko bol raha hai, `0` ko nahi).
   - **INCORRECT** ❌ — technically kaam karta hai `undefined` case ke liye, lekin side-effect ke sath jo `0` input ko bhi galat treat karta hai — is wajah se ye "correctly handle sirf undefined" requirement se match nahi karta.

2. **Option B Analysis:** `function multiply(a = 1, b = 1, c = 1) { return a * b * c; }`
   - **Default parameters** specifically tabhi apply hote hain jab argument `undefined` ho (ya diya hi na jaaye) — `0` jaisi valid falsy value ko **replace nahi karte**.
   - `return` statement bhi correctly present hai.
   - **CORRECT** ✅ — Ye sabse **clean aur precise** tarika hai `undefined` handle karne ka.

3. **Option C Analysis:** `(a ?? 1) * (b ?? 1) * (c ?? 1)`
   - **Nullish coalescing (`??`)** sirf `null`/`undefined` ko target karta hai, `0` ko as-is rakhta hai.
   - `return` statement bhi correctly present hai.
   - **CORRECT** ✅ — Ye bhi precise tarika hai, `||` ke bug se bacha hua.

4. **Option D Analysis:**
   ```js
   if(a === undefined) a = 1;
   if(b === undefined) b = 1;
   if(c === undefined) c = 1;
   a * b * c;  // <-- Yaha 'return' MISSING hai!
   ```
   - Logic (undefined check) **sahi** hai — sirf `undefined` ko `1` se replace kar raha hai, `0` ko touch nahi karta.
   - **Lekin BUG hai** — last line `a * b * c;` sirf ek **expression statement** hai, iske aage `return` keyword **nahi** hai! Isliye function **implicitly `undefined` return** karega, calculation hone ke bawajood.
   - **INCORRECT** ❌ — kyunki actual **return value hamesha `undefined`** hoga, calculation kaam nahi karta as intended (function correctly product return nahi karta).

**Shortcut:** Jab bhi function definition dikhe, sabse pehle **check karo `return` statement hai ya nahi** — ye ek bahut common trap hai jaha logic sahi hoti hai lekin `return` missing hone ki wajah se poora function useless ho jaata hai.

---

## Final Answer
**Options B and C are correct.**

---

## Why Other Options are Wrong?
### Option A
`||` operator sirf `undefined` nahi, balki **saare falsy values** (`0`, `""`, `NaN`, `false`) ko bhi replace kar deta hai — jo `0` jaise legitimate input ke liye **incorrect behavior** create karta hai.

### Option D
Logic (`if(x === undefined) x = 1`) bilkul sahi hai, lekin **`return` keyword missing** hai last line pe — isliye function hamesha `undefined` return karega, chahe calculation internally sahi ho bhi jaaye.

---

## Important Exam Notes
- ✅ **Default Parameters** (`a = 1`) = sirf `undefined` ke liye specifically design kiya gaya, sabse clean approach.
- ✅ **Nullish Coalescing (`??`)** = sirf `null`/`undefined` target karta hai, `0`/`""`/`false` ko chhedta nahi.
- ✅ **Logical OR (`||`)** = saare falsy values ko target karta hai — `undefined`-specific handling ke liye **risky** hai.
- ⚠️ Common Mistake: `||` aur `??` ko same samajh lena — dono me bahut fark hai jab `0`, `""`, ya `false` jaise valid falsy values input ho sakte hain.
- ⚠️ Common Mistake: Function ke last statement me `return` bhool jaana — calculation hoti hai lekin result kahi return nahi hota.
- 💡 Trick: "**`??`** = **N**ullish only (null/undefined), **`||`** = **A**ny falsy (0, '', false, null, undefined, NaN)"

---

## Similar Question Pattern
Default parameter handling, `||` vs `??` operator differences, aur missing `return` statement bugs — ye sab common JavaScript "gotcha" concepts hain jo MSQ format me combine karke puche jaate hain.

---

## Revision Box
Default parameters (`a=1`) aur nullish coalescing (`a ?? 1`) dono sirf `undefined`/`null` ko target karte hain — sahi approach hai. `||` operator saare falsy values (`0` samet) ko replace kar deta hai — buggy hai is use-case ke liye. Missing `return` statement function ko hamesha `undefined` return karwata hai chahe calculation sahi ho. Correct Options: **B and C**.

---
---

# Question 176

## Original Question
**Comprehension Context:** Parent binds an input with `v-model` and passes the value as a prop to a child component; the child contains a named slot that shows the message in uppercase.

```html
<div id="app">
  <p>{{ message }}</p>
  <input v-model="message" />
  <child-component :title="message">
    <template slot="header">
      <h3>{{ message.toUpperCase() }}</h3>
    </template>
  </child-component>
</div>

<script>
Vue.component('child-component', {
  props: ['title'],
  template: `
    <div>
      <h2>{{ title }}</h2>
      <slot name="header"></slot>
      <p>Child Content</p>
    </div>
  `
});

new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue'
  }
});
</script>
```
> If the user types "hi" into the input box, what happens on the page, choose the closest to correct?
>
> Options:
> A. Only `<p>` updates to "hi", but `<h2>` remains "Hello Vue".
> B. Both `<p>` and `<h2>` update and display "hi".
> C. Only `<h3>` changes to "HI", but `<p>` and `<h2>` remain unchanged.
> D. Nothing updates at all.

---

## Correct Answer
**Correct Option:** B (Both `<p>` and `<h2>` update and display "hi")

---

## Concept Used
- 📘 **`v-model` (Two-way binding):** Jab user input box me type karta hai, `v-model` automatically Vue ke `message` data property ko **update** kar deta hai (aur vice-versa).
- 📘 **Reactivity Propagation:** Jab `message` data change hoti hai, Vue ke **reactive system** ki wajah se **saari jagah** jaha `message` use ho raha hai, wo automatically **re-render/update** ho jaati hai — chahe wo parent template me ho ya props ke through child me pass ho raha ho.
- 📘 **Props Passing (`:title="message"`):** Parent `message` ko child component ko `title` prop ke through pass kar raha hai. Jab parent ki `message` change hoti hai, ye change **automatically child ke `title` prop tak propagate** ho jaata hai (Vue's reactive prop system).
- 📘 **Slot Content Scope:** Named slot (`<template slot="header">`) ke andar jo content hai (`{{ message.toUpperCase() }}`), wo actually **parent ke scope** me evaluate hota hai (kyunki slot content **parent ke template** me define hua hai) — isliye ye bhi `message` ke change hone par reactive rehta hai aur update hoga.

---

## Step-by-Step Solution
1. **Step 1:** Initial state — `message = "Hello Vue"`. Page pe dikh raha hai:
   - `<p>Hello Vue</p>` (interpolation)
   - `<input>` me "Hello Vue" (v-model se)
   - Child component ke andar `<h2>Hello Vue</h2>` (prop `title` se)
   - `<h3>HELLO VUE</h3>` (named slot content, uppercase)
2. **Step 2:** User input box me type karta hai "hi" (poora text replace karke).
   - *Reason:* `v-model` do-tarfa binding hai — jab user input change karta hai, `message` data property **automatically update** ho jaati hai.
3. **Step 3:** `message` ab `"hi"` ho gaya hai. Vue ke **reactivity system** ki wajah se, jaha bhi `message` use ho raha hai template me, wo saari jagah **re-evaluate** hoti hai:
   - `<p>{{ message }}</p>` → update hokar `<p>hi</p>` ban jaata hai.
   - *Reason:* Direct interpolation hai, seedha reactive update hoga.
4. **Step 4:** `:title="message"` bhi reactive prop binding hai — jab parent ka `message` change hota hai, ye change **child component ke `title` prop tak automatically propagate** ho jaata hai.
   - `<h2>{{ title }}</h2>` (child ke andar) → update hokar `<h2>hi</h2>` ban jaata hai.
   - *Reason:* Vue props reactive hote hain — parent data change hote hi child props bhi turant update hote hain.
5. **Step 5 (Bonus insight):** Named slot ka content (`{{ message.toUpperCase() }}`) bhi **parent ke scope** me hi evaluate hota hai (kyunki slot content structurally parent ke template me likha gaya hai, sirf child ke andar "render" hota hai) — isliye `message` change hone se ye bhi reactive update lega aur `<h3>HI</h3>` ban jayega.
   - *Reason:* Slot content ka data-scope hamesha **parent** ka hota hai (jab tak "scoped slots" explicitly use na kiye jaayein) — isliye ye bhi automatically update hoga.
6. **Final State:** `<p>` = "hi", `<h2>` = "hi" (via reactive prop), aur `<h3>` bhi "HI" ho jayega (uppercase, via reactive slot content) — **saari jagah reactivity propagate hoti hai**.

**Shortcut:** Jab bhi ek hi data property (`message`) multiple jagah use ho rahi ho (direct interpolation, props, slots), yaad rakho — Vue ki **reactivity ek central data source se automatically saari dependent UI parts ko update kar deti hai**, chahe wo kitni bhi "nested" kyu na ho.

---

## Final Answer
**"Both `<p>` and `<h2>` update and display 'hi'."** (aur reactivity ki wajah se `<h3>` bhi "HI" update hoga, jo is option me explicitly mention nahi hua lekin ye sabse **"closest to correct"** hai kyunki ye galat exclusive claims nahi karta jaise doosre options).

---

## Why Other Options are Wrong?
### Option A
Galat hai — `<h2>` (jo child ke `title` prop se aata hai) bhi update hoga, kyunki props reactive hote hain aur parent ka `message` change hone se automatically propagate hota hai. Ye option galti se sochta hai ki props "static/frozen" hain.

### Option C
Galat hai — Ye claim karta hai ki **sirf** `<h3>` update hoga aur `<p>`, `<h2>` unchanged rahenge — ye ulta galat hai, kyunki actually **saari jagah** (`<p>`, `<h2>`, aur `<h3>`) reactive update leti hain, sirf `<h3>` nahi.

### Option D
Galat hai — Vue ki poori reactivity system hi is baat pe based hai ki data change hone se UI automatically update ho — "kuch update na hona" Vue ke fundamental working principle ke against hai.

---

## Important Exam Notes
- ✅ `v-model` = two-way binding, input change se data update, data change se input update.
- ✅ Props reactive hote hain — parent data change hone se child props automatically update hote hain.
- ✅ Named slot content parent ke **data scope** me evaluate hota hai, isliye wo bhi reactive rehta hai.
- ⚠️ Common Mistake: Sochna ki props ya slot content ek baar "set" hone ke baad static/frozen ho jaate hain — galat hai, Vue ki reactivity poori chain me propagate hoti hai.
- 💡 Trick: "One data source, many reactive consumers — sab automatically sync rehte hain!"

---

## Similar Question Pattern
Vue reactivity propagation (v-model → data → props → slots) ke conceptual/trace-based comprehension questions common hain — inme samajhna zaroori hai ki data flow kaise multiple UI parts tak pahunchta hai.

---

## Revision Box
`v-model` input ko `message` se bind karta hai (two-way). `message` change hone se — `<p>` (direct interpolation), `<h2>` (child prop `title`), aur `<h3>` (parent-scoped slot content) — **sab reactive update** lete hain. Best matching option: "Both `<p>` and `<h2>` update and display 'hi'."

---
---

# Question 177

## Original Question
> Which Vue.js features are shown in the code?
>
> Options:
> A. Two-way data binding using `v-model`.
> B. Use of a named `slot` inside a child component.
> C. Passing values from parent to child using `props`.
> D. Use of lifecycle hooks.

(Multiple Select Question, Correct Marks: 2)

---

## Correct Answer
**Correct Options:** A, B, and C

---

## Concept Used
- 📘 **`v-model`:** Ye code me `<input v-model="message" />` ke through use hua hai — ye **two-way data binding** feature hai.
- 📘 **Named Slots:** Code me `<slot name="header"></slot>` (child template me) aur `<template slot="header">` (parent usage me) — ye **named slot** feature hai jo parent ko child ke specific placeholder me content inject karne deta hai.
- 📘 **Props:** Code me `props: ['title']` (child component definition me) aur `:title="message"` (parent se pass karte waqt) — ye **props** feature hai, parent-to-child data passing ka standard tarika.
- 📘 **Lifecycle Hooks:** Ye special functions hote hain (`created`, `mounted`, `beforeMount`, `updated`, etc.) jo Vue component ki life ke different stages pe automatically call hote hain — is diye gaye code me **koi bhi lifecycle hook define nahi kiya gaya hai**.

---

## Step-by-Step Solution
1. **Feature Check A: `v-model`** → Code me `<input v-model="message" />` clearly present hai. Ye definitely **two-way data binding** demonstrate kar raha hai.
   - **TRUE** ✅
2. **Feature Check B: Named Slot** → Code me child component ke template me `<slot name="header"></slot>` hai, aur parent ise `<template slot="header">...</template>` se use kar raha hai.
   - **TRUE** ✅
3. **Feature Check C: Props** → Child component `props: ['title']` define karta hai, aur parent `:title="message"` se value pass kar raha hai.
   - **TRUE** ✅
4. **Feature Check D: Lifecycle Hooks** → Code ko poora dhyan se dekho — kahi bhi `created()`, `mounted()`, `beforeMount()`, ya koi bhi lifecycle hook **define nahi** kiya gaya hai, na child component me na parent Vue instance me.
   - **FALSE** ❌ — Ye feature is code me **use hi nahi hui hai**.

---

## Final Answer
**A, B, and C are the features shown; D (lifecycle hooks) is NOT used in this code.**

---

## Why Other Options are Wrong?
### Option D
Code me kahi bhi koi lifecycle hook method (jaise `mounted()`, `created()`) define nahi hui hai — sirf `data`, `props`, aur `template` options use hue hain, koi lifecycle-related code nahi hai.

---

## Important Exam Notes
- ✅ `v-model` = two-way binding.
- ✅ Named slots = specific content placeholders with a `name` attribute.
- ✅ Props = parent-to-child one-way data passing.
- ⚠️ Common Mistake: Kisi bhi Vue code me "kuch feature dikh raha hai" wale distractor options ko bina verify kiye select kar lena — hamesha code ko carefully scan karo ki wo specific syntax/keyword actually present hai ya nahi.
- 💡 Trick: Lifecycle hooks ke liye specific method names (`created`, `mounted`, etc.) dhoondo — agar wo nahi milte, to lifecycle hooks use nahi hue.

---

## Similar Question Pattern
"Code me kaunse features use hue hain" type ke identification-based MSQ questions common hain Vue ke conceptual assessment me — inme carefully code scan karna padta hai har feature ke liye.

---

## Revision Box
Code me `v-model` (two-way binding), named `slot` (child content placeholder), aur `props` (parent→child data passing) — teeno features clearly present hain. Lifecycle hooks (jaise `mounted`, `created`) **kahi bhi define nahi** hue is code me.

---
---

# 🎯 Overall Quick Revision Summary

| Q.No | Topic | Key Concept |
|---|---|---|
| 160 | K-Means Clustering | Convergence iteration count |
| 161 | Exam Instructions | Subject confirmation (0 marks) |
| 162 | Vue Syntax Validity | Shorthand vs full directive syntax |
| 163 | v-if vs v-show | DOM removal vs CSS display toggle |
| 164 | Array `.sort()` | Ascending order comparator (`a-b`) |
| 165 | Array Chaining | `.filter().map().sort()` step tracking |
| 166 | Closures | Live reference, latest value at call-time |
| 167 | Vue Directives Matching | v-bind, v-model, v-on, v-show, v-cloak |
| 168 | Type Coercion | String `+=` Number = concatenation |
| 169 | Nested Loops + Concatenation | Operator precedence in `+=` |
| 170 | Event Handling | `addEventListener` vs `onclick` co-existence |
| 171 | Vue Instance Data Access | `app.data` invalid, use `app.$data`/`app.propName` |
| 172 | Method Chaining + Spread | `filter→map→filter`, order preservation |
| 173 | `this` Binding (Regular vs Arrow) | Lexical vs dynamic `this` |
| 174 | Class Inheritance | Overriding, `super()`, detached method calls |
| 175 | Default Params vs `\|\|` vs `??` | Undefined handling + missing `return` bug |
| 176 | Vue Reactivity Propagation | v-model → data → props → slots |
| 177 | Vue Feature Identification | v-model, slots, props vs lifecycle hooks |

---

