# 📘 App Dev 2 (MAD 2) — Quiz 1 (Jan 2025) PYQ Detailed Notes
### IIT Madras BS in Data Science and Applications
**Language:** Hinglish | **Level:** Beginner-friendly | **Questions Covered:** 114–130

---

# Question 114

## Original Question
> THIS IS QUESTION PAPER FOR THE SUBJECT "DIPLOMA LEVEL : MODERN APPLICATION DEVELOPMENT II (COMPUTER BASED EXAM)"
>
> ARE YOU SURE YOU HAVE TO WRITE EXAM FOR THIS SUBJECT?
> CROSS CHECK YOUR HALL TICKET TO CONFIRM THE SUBJECTS TO BE WRITTEN.
>
> Options: (A) YES (B) NO

---

## Correct Answer
**Correct Option:** A (YES)

---

## Concept Used
Ye ek **administrative/instructional question** hai, koi technical concept nahi hai. Isme **0 marks** milte hain — ye sirf confirm karta hai ki student sahi subject ka exam attempt kar raha hai.

---

## Step-by-Step Solution
1. **Step 1:** Question confirm kar raha hai ki student MAD 2 subject ka exam de raha hai.
2. **Step 2:** Hall ticket check karo — agar subject match karta hai to "YES" select karo.

---

## Final Answer
**YES**

---

## Why Other Options are Wrong?
### Option B (NO)
"NO" select karne ka matlab hoga galat subject ka paper khula hai, jo normal scenario me sahi nahi hai.

---

## Important Exam Notes
- ⚠️ 0-marks confirmation question.
- 💡 Hamesha hall ticket cross-check karo exam start se pehle.

---

## Similar Question Pattern
Har exam paper ke start me ye type ka confirmation question repeat hota hai.

---

## Revision Box
Subject confirmation step — answer hamesha "YES" hota hai jab subject match kare.

---
---

# Question 115

## Original Question
> Which of the following statements is true regarding "v-if" and "v-show" directives in VueJS?
>
> Options:
> A. The "v-if" directive adds/removes elements from the DOM, while v-show only toggles visibility using CSS.
> B. The "v-show" directive adds/removes elements from the DOM, while v-if only toggles visibility using CSS.
> C. Both the "v-if" and "v-show" directives add/remove elements from the DOM.
> D. Both the "v-if" and "v-show" directives toggle visibility using CSS.

---

## Correct Answer
**Correct Option:** A

---

## Concept Used
- 📘 **`v-if`:** Ye directive condition **false** hone par element ko **DOM se completely remove** kar deta hai (aur true hone par wapas add karta hai). Ye "**real/conditional rendering**" karta hai — element structurally exist hi nahi karta jab condition false ho.
- 📘 **`v-show`:** Ye directive element ko **hamesha DOM me rakhta hai**, sirf uska CSS `display` property toggle karta hai (`display: none` vs default). Element **structurally hamesha present** rehta hai, sirf visually hide/show hota hai.
- 📘 **Performance Difference (Intuition):** `v-if` **expensive** hai (DOM add/remove) lekin **initial render cost kam** hota hai (agar false ho to render hi nahi hota). `v-show` **hamesha render** hota hai (chahe hidden ho), lekin **toggle karna sasta** hai (sirf CSS class/style change). Isliye:
  - Agar condition **kabhi-kabhi** change hoti ho → `v-if` use karo.
  - Agar condition **baar-baar toggle** hoti ho → `v-show` use karo (performance behtar).

**Example:**
```html
<p v-if="isVisible">Ye DOM se hata di jaayegi agar isVisible false ho</p>
<p v-show="isVisible">Ye hamesha DOM me rahegi, sirf CSS display toggle hoga</p>
```

---

## Step-by-Step Solution
1. **Step 1:** `v-if` ka behavior check karo — false hone par element **DOM tree se hi nikal diya** jaata hai, true hone par **naya element create** karke add kiya jaata hai.
   - *Reason:* Ye "conditional rendering" ka **true structural** implementation hai.
2. **Step 2:** `v-show` ka behavior check karo — element **hamesha DOM me maujood** rehta hai, sirf `style="display: none"` add/remove hota hai CSS ke through.
   - *Reason:* Ye sirf **visual toggle** hai, structural change nahi.
3. **Step 3:** Statement A ye exact difference bataता hai — `v-if` = DOM add/remove, `v-show` = CSS visibility toggle. Ye **sahi** hai.

**Shortcut:** "**v-if = In/Out of DOM**" aur "**v-show = Show/Hide via CSS**" — naam se hi yaad rakh sakte ho.

---

## Final Answer
**"The 'v-if' directive adds/removes elements from the DOM, while v-show only toggles visibility using CSS."**

---

## Why Other Options are Wrong?
### Option B
Ye statements **reversed/ulta** hai — actually `v-if` DOM add/remove karta hai aur `v-show` CSS toggle karta hai, is option me ulta likha hai.

### Option C
Galat hai — sirf `v-if` DOM se add/remove karta hai, `v-show` **nahi** karta (wo hamesha DOM me rehta hai).

### Option D
Galat hai — sirf `v-show` CSS visibility toggle karta hai, `v-if` **structurally DOM change** karta hai, sirf CSS toggle nahi.

---

## Important Exam Notes
- ✅ `v-if` = DOM se add/remove (structural, "heavy" toggle cost, "light" initial render if false).
- ✅ `v-show` = sirf CSS `display` toggle (always in DOM, "light" toggle cost, "heavy" initial render always).
- ⚠️ Common Mistake: `v-if` aur `v-show` ko interchangeably use kar lena bina performance implications samjhe.
- 💡 Trick: Frequently toggle karna ho → `v-show`. Rarely change hota ho → `v-if`.

---

## Similar Question Pattern
`v-if` vs `v-show` comparison Vue ke sabse common conceptual questions me se ek hai — har quiz me repeat hota hai.

---

## Revision Box
`v-if` = element ko DOM se **add/remove** karta hai (true conditional rendering). `v-show` = element **hamesha DOM me** rehta hai, sirf CSS `display` **toggle** hota hai.

---
---

# Question 116

## Original Question
```js
let x = 5;
(function () {
  console.log(x);
  let x = 10;
})();
```
> What will be the output of the above program?
>
> Options: A. 5  B. 10  C. undefined  D. Reference Error

---

## Correct Answer
**Correct Option:** D (Reference Error)

---

## Concept Used
- 📘 **Temporal Dead Zone (TDZ):** `let` aur `const` se declare kiye gaye variables **hoisted to hote hain**, lekin unhe `undefined` se initialize **nahi** kiya jaata (jaise `var` hota hai). Iski jagah, ye variables ek "**dead zone**" me rehte hain — declaration line tak pahunchne se **pehle** agar unhe access kiya jaaye, to JavaScript **`ReferenceError`** throw karta hai.
- 📘 **Variable Shadowing + TDZ Combination:** Is code me function ke andar `let x = 10;` likha hai — chunki `let` **block-scoped** hai, is IIFE (Immediately Invoked Function Expression) ke andar `x` naam ka ek **naya local variable** hoist ho jaata hai (TDZ me), jo **outer `x` (5) ko shadow kar deta hai** poore function block ke liye — chahe local `x` ki declaration line **baad me** ho.
- 📘 **Important Insight:** JavaScript engine pehle **poore scope ko scan** karta hai variable declarations dhoondhne ke liye (hoisting phase). Jaise hi `let x = 10` kahi bhi function ke andar milta hai, JS **turant is `x` ko us poore function-block ke liye "local" maan leta hai** — matlab `console.log(x)` (jo declaration se **pehle** likha hai) bhi is **local `x`** ko hi refer karega, outer/global `x` ko **nahi**.

**Example:**
```js
let y = 100;
{
  console.log(y); // ReferenceError! (not 100)
  let y = 200;
}
```

---

## Step-by-Step Solution
1. **Step 1:** `let x = 5;` — Ye **outer/global** scope ka `x` hai, value `5`.
2. **Step 2:** IIFE `(function() {...})()` turant execute hota hai. Iske andar do lines hain: `console.log(x);` aur `let x = 10;`.
3. **Step 3: Hoisting Phase (JS Engine ka internal step):** JS engine function ke andar scan karta hai aur dekhta hai ki `let x = 10;` bhi likha hai. Isliye is **poore function block ke liye `x` ek local variable ban jaata hai** (hoisted, lekin TDZ me — uninitialized state).
   - *Reason:* Ye scanning **execution se pehle** hoti hai — JS pehle poora scope "parse" karta hai, phir line-by-line execute karta hai.
4. **Step 4:** Ab jab `console.log(x);` **execute** hoti hai, JS is `x` ko **local `x`** samajhta hai (kyunki wo already function-scope me "reserved" ho chuka hai Step 3 me) — lekin is local `x` ki **declaration line abhi tak nahi aayi** (`let x = 10` neeche hai), isliye ye **Temporal Dead Zone** me hai.
   - *Reason:* `let` variables declaration line tak access **nahi** kiye ja sakte, chahe wo hoisted ho chuke hon.
5. **Step 5:** Isliye `console.log(x);` execute hote hi **`ReferenceError: Cannot access 'x' before initialization`** throw hota hai.

**Shortcut:** Jab bhi kisi function/block ke andar `let`/`const` se koi variable **baad me** declare ho, lekin usi naam ka variable **pehle access** kiya ja raha ho **usi block ke andar**, turant socho — "**TDZ trap ho sakta hai!**" — outer variable access nahi hoga, local wala hi (uninitialized state me) target hoga.

---

## Final Answer
**Reference Error**

---

## Why Other Options are Wrong?
### Option A (5)
Ye galat hai — students soch sakte hain ki `console.log(x)` **outer `x` (5)** ko access karega (kyunki local `x` abhi declare hi nahi hua uss line tak) — lekin **hoisting ki wajah se** local `x` already "reserved" ho chuka hota hai poore function-block ke liye, isliye outer `x` **accessible hi nahi** hai is function ke andar.

### Option B (10)
Ye galat hai — `let x = 10;` **abhi execute nahi hui** jab `console.log(x)` chal raha hai (wo baad ki line hai) — isliye `x` ki value `10` set hone se **pehle** hi error aa jaata hai.

### Option C (undefined)
Ye galat hai — ye tab hota agar `var x = 10;` hota (kyunki `var` `undefined` se initialize hota hai hoisting ke time). Lekin `let` **TDZ** follow karta hai, `undefined` nahi deta — access karte hi error throw karta hai.

---

## Important Exam Notes
- ✅ `let`/`const` **hoisted hote hain but TDZ me rehte hain** — declaration line se pehle access karna error deta hai.
- ✅ `var` hoisted hota hai **aur `undefined` se initialize** hota hai — declaration se pehle access karne pe `undefined` milta hai, error nahi.
- ⚠️ Common Mistake: Sochna ki agar variable declare hi nahi hua (abhi tak) to outer scope wala variable use hoga — galat hai agar same naam ka `let`/`const` **usi block me kahi bhi** declare hua ho.
- 💡 Trick: "**Same block me `let` dikhe kahi bhi = us poore block me wo naam TDZ me hai jab tak declaration na aaye**"

---

## Similar Question Pattern
TDZ (Temporal Dead Zone) + variable shadowing ke trace-output questions bahut common hain — ye ek classic JS "gotcha" concept hai jo exam me baar-baar test hota hai.

---

## Revision Box
`let x = 10;` function ke andar hone ki wajah se, poore function-block me `x` **local variable ban jaata hai** (hoisted, TDZ me). `console.log(x)` declaration se **pehle** chalti hai, isliye **ReferenceError** aata hai — outer `x` (5) access nahi hota.

---
---

# Question 117

## Original Question
```js
console.log(myVar);
console.log(myFunc());

var myVar = "hello";
function myFunc() {
    return "world";
}
```
> What is the output of this code?
>
> Options: A. hello, world  B. undefined, world  C. ReferenceError  D. undefined, undefined

---

## Correct Answer
**Correct Option:** B (undefined, world)

---

## Concept Used
- 📘 **`var` Hoisting:** `var` se declare kiye gaye variables **hoisted** hote hain aur **`undefined`** se **initialize** ho jaate hain (declaration se pehle bhi access karne par `undefined` milta hai, error nahi).
- 📘 **Function Declaration Hoisting:** `function myFunc() {...}` jaisi **function declarations** (function expressions nahi) **poori tarah hoist** hoti hain — matlab **poora function body** bhi upar chala jaata hai, na ki sirf naam. Isliye function declaration ko uski actual likhi hui line se **pehle** bhi call kar sakte ho.
- 📘 **Difference Yaad Rakhna Zaroori:**
  - `var myVar` → sirf naam hoist hota hai, value `undefined`.
  - `function myFunc(){}` → **naam + poora function body** dono hoist hote hain, isliye turant call karne layak hota hai.

**Example:**
```js
sayHi(); // "Hi!" — works! (function declaration fully hoisted)
console.log(x); // undefined (var hoisted, value undefined)
var x = 5;
function sayHi() { console.log("Hi!"); }
```

---

## Step-by-Step Solution
1. **Step 1 (Hoisting Phase):** JS engine code run karne se pehle poore scope ko scan karta hai:
   - `var myVar;` → hoisted, `undefined` se initialize.
   - `function myFunc() {...}` → **poora function** (naam + body) hoisted, turant usable.
2. **Step 2:** `console.log(myVar);` — Is line tak `myVar` sirf **hoisted hua hai, `"hello"` assign nahi hui** (wo assignment line neeche hai, abhi tak execute nahi hui). Isliye `myVar` ki current value **`undefined`** hai.
   - *Reason:* `var` hoisting sirf declaration hoist karta hai, assignment (`= "hello"`) apni original jagah pe hi execute hoti hai.
3. **Step 3:** `console.log(myFunc());` — `myFunc` **poori tarah hoisted** ho chuka hai (function declaration), isliye ise **abhi bhi safely call** kiya ja sakta hai, chahe uski likhi hui line code me neeche ho.
   - `myFunc()` chalta hai, `return "world";` execute hota hai.
   - **Output: `"world"`**
4. **Step 4:** Final combined output: `undefined, world`.

**Shortcut:** Yaad rakho — "**var = sirf naam hoist (undefined), function declaration = pura body hoist (turant usable)**"

---

## Final Answer
**"undefined, world"**

---

## Why Other Options are Wrong?
### Option A (hello, world)
Galat hai — `myVar` ki value `"hello"` **abhi assign nahi hui thi** jab `console.log(myVar)` chala (assignment line code me baad me hai) — isliye `undefined` milega, `"hello"` nahi.

### Option C (ReferenceError)
Galat hai — ye tab hota agar `myVar` `let`/`const` se declare hui hoti (TDZ trigger hoti). Lekin `var` **hoisting + `undefined` initialization** follow karta hai, error throw nahi karta.

### Option D (undefined, undefined)
Galat hai — `myFunc` ek **function declaration** hai, jo **poori tarah hoist** hoti hai (poora body samet), isliye ye turant **call ho sakti hai aur "world" return** karegi — `undefined` nahi dega.

---

## Important Exam Notes
- ✅ `var` hoisting = naam hoist + `undefined` initial value.
- ✅ Function declaration hoisting = naam + poora body hoist, turant callable.
- ✅ Function **expression** (`var myFunc = function(){}`) `var` jaisa hi behave karega — sirf naam hoist, function value nahi.
- ⚠️ Common Mistake: Function declaration aur function expression ke hoisting behavior ko same samajh lena — dono **alag** hain.
- 💡 Trick: "Function **declaration** = poora hoist. Function **expression** = sirf naam (var jaisa) hoist."

---

## Similar Question Pattern
`var` hoisting vs function declaration hoisting ke combination wale trace-output questions common hain.

---

## Revision Box
`var myVar` hoisted with `undefined` — pehla `console.log` **undefined** deta hai. `function myFunc(){}` poori tarah hoisted (body samet) — dusra `console.log` **"world"** deta hai (safely call ho sakta hai declaration se pehle bhi). Output: **undefined, world**.

---
---

# Question 118

## Original Question
> Which of the following statements is **true** about the let, const, and var keywords in JavaScript?
>
> Options:
> A. var is block-scoped, while let and const are function-scoped.
> B. const variables must always be initialized at the time of declaration.
> C. Variables declared with let and const are hoisted and initialized with undefined.
> D. let, const, and var have identical scoping rules.

---

## Correct Answer
**Correct Option:** B (const variables must always be initialized at the time of declaration.)

---

## Concept Used
- 📘 **`const` Ki Mandatory Initialization Rule:** `const` se declare kiya gaya variable ek **immutable binding** banata hai — iska matlab hai declaration ke waqt **hi** value deni padti hai, warna JavaScript **SyntaxError** throw karega (`Missing initializer in const declaration`).
- 📘 **Scoping Rules (Correct Understanding):**
  - `var` = **function-scoped** (poore function ke andar accessible, chahe kisi bhi block `{}` ke andar declare ho).
  - `let`/`const` = **block-scoped** (sirf us `{}` block ke andar accessible jisme declare hue).
- 📘 **Hoisting Behavior (Correct Understanding):**
  - `var` = hoisted **aur `undefined`** se initialize hota hai.
  - `let`/`const` = hoisted hote hain lekin **TDZ (Temporal Dead Zone)** me rehte hain — `undefined` se initialize **nahi** hote, declaration se pehle access karne par **error** aata hai.

**Example:**
```js
const x; // ❌ SyntaxError: Missing initializer in const declaration
const y = 10; // ✅ Valid
```

---

## Step-by-Step Solution
1. **Statement A Check:** "var is block-scoped, while let and const are function-scoped" → **False**. Ye **ulta** hai — `var` **function-scoped** hai, `let`/`const` **block-scoped** hain.
2. **Statement B Check:** "const variables must always be initialized at the time of declaration" → **True**. `const` ek immutable binding hai jiski value declare karte waqt **hi** deni padti hai — bina initialization ke `const` likhna **SyntaxError** deta hai.
   - *Reason:* `const` ka poora concept hi ye hai ki uski value **kabhi reassign na ho** — isliye starting value declaration ke waqt hi zaroori hai.
3. **Statement C Check:** "Variables declared with let and const are hoisted and initialized with undefined" → **False**. `let`/`const` hoist to hote hain, lekin `undefined` se initialize **nahi** hote — wo TDZ me rehte hain (declaration se pehle access karne pe error aata hai, `undefined` nahi milta).
4. **Statement D Check:** "let, const, and var have identical scoping rules" → **False**. In teeno ke scoping rules **bilkul alag** hain (`var`=function-scoped, `let`/`const`=block-scoped).

**Shortcut:** Yaad rakho — "**const = Compulsory initialization**" (naam me hi "const" hai, jo "constant/fixed" ka sense deta hai, isliye starting value fix karni hi hogi).

---

## Final Answer
**"const variables must always be initialized at the time of declaration."**

---

## Why Other Options are Wrong?
### Option A
Scoping rules **reversed** hain is statement me — `var` function-scoped hai (block-scoped nahi), `let`/`const` block-scoped hain (function-scoped nahi).

### Option C
`let`/`const` `undefined` se initialize **nahi** hote — wo TDZ me rehte hain. Ye statement `var` ke behavior ko `let`/`const` pe galat apply kar raha hai.

### Option D
`var`, `let`, aur `const` ke scoping rules **bilkul different** hain — `var` function-scoped, baaki do block-scoped. Ye statement bilkul galat generalization hai.

---

## Important Exam Notes
- ✅ `const` = declaration ke waqt initialization **mandatory**.
- ✅ `var` = function-scoped, hoisted with `undefined`.
- ✅ `let`/`const` = block-scoped, hoisted but in TDZ (no `undefined`).
- ⚠️ Common Mistake: `let` aur `const` ke hoisting behavior ko `var` jaisa samajh lena.
- 💡 Trick: "**const** naam hi bata raha hai — **constant** (fixed) rehna hai, isliye shuru se hi value chahiye."

---

## Similar Question Pattern
`var`/`let`/`const` ke scoping, hoisting, aur initialization rules ke conceptual comparison MCQs bahut common hain — ye JS fundamentals ka core topic hai.

---

## Revision Box
`const` = **mandatory initialization** at declaration (varna SyntaxError). `var` = function-scoped, hoisted with `undefined`. `let`/`const` = block-scoped, hoisted but TDZ (no `undefined`, error on early access).

---
---

# Question 119

## Original Question
```js
function outer() {
  let count = 0;
  return function inner() {
    return ++count;
  };
}

const counter1 = outer();
const counter2 = outer();

console.log(counter1());
console.log(counter1());
console.log(counter2());
```
> What will be the output of the above program?
>
> Options:
> A. 1, 2, 3
> B. 1, 1, 1
> C. 1, 2, 1
> D. 0, 1, 0
> E. 0, 1, 1

---

## Correct Answer
**Correct Option:** C (1, 2, 1)

---

## Concept Used
- 📘 **Closures:** Har baar `outer()` call hota hai, ek **naya, independent** `count` variable banta hai jo **`inner` function ke through "yaad" rakha** jaata hai — chahe `outer()` ka execution khatam ho chuka ho.
- 📘 **Pre-Increment (`++count`):** Ye **pehle value ko increment** karta hai, **phir naya (incremented) value return** karta hai. Ye post-increment (`count++`) se **bilkul ulta** hai.
  - `++count` (pre): pehle badhao, phir return karo.
  - `count++` (post): pehle return karo, phir badhao.
- 📘 **Independent Closures per Instance:** `counter1` aur `counter2` **do alag `outer()` calls** se bane hain — dono ka apna **alag `count`** hai, ek dusre se **completely independent**.

**Example:**
```js
function counter() {
  let n = 0;
  return () => ++n;
}
const a = counter();
console.log(a()); // 1 (pre-increment: pehle 0->1, phir 1 return)
console.log(a()); // 2 (pehle 1->2, phir 2 return)
```

---

## Step-by-Step Solution
1. **Step 1:** `const counter1 = outer();` — `outer()` call hota hai, `count = 0` banta hai is closure ke andar. `counter1` ab `inner` function ko refer karta hai jo is `count` (0) ko "yaad" rakhta hai.
2. **Step 2:** `const counter2 = outer();` — Ek **naya, alag** `outer()` call hota hai. Isme bhi `count = 0` banta hai, lekin ye **counter1 wale count se bilkul different variable** hai.
3. **Step 3:** `console.log(counter1());` — `counter1` (jo `inner` function hai) call hota hai. `++count` execute hota hai jab `count = 0` hai counter1 ke closure me:
   - Pehle `count` **increment** hota hai: `0 → 1`.
   - Phir **naya value (1) return** hoti hai.
   - **Output: `1`**
4. **Step 4:** `console.log(counter1());` — Doosri baar `counter1` call ho raha hai. Ab is closure ka `count` already `1` hai (pichhli call se).
   - `++count`: `count` pehle `1 → 2` hota hai, phir **2 return** hoti hai.
   - **Output: `2`**
5. **Step 5:** `console.log(counter2());` — Ye `counter2` hai, jiska **apna alag `count`** hai jo abhi tak `0` hi hai (kyunki `counter2` pehli baar call ho raha hai — `counter1` ki calls ka isse koi lena dena nahi).
   - `++count`: `count` pehle `0 → 1` hota hai, phir **1 return** hoti hai.
   - **Output: `1`**

**Shortcut:** Jab bhi `outer()` do baar call ho (do alag const jaise `counter1`, `counter2`), turant socho — "dono ka apna **separate** closure hai, ek dusre ko affect nahi karte." Aur `++count` (pre-increment) hamesha **increment ke baad** ki value deta hai.

---

## Final Answer
**1, 2, 1**

---

## Why Other Options are Wrong?
### Option A (1, 2, 3)
Galat hai — ye tab hota agar `counter1` aur `counter2` **same `count`** share kar rahe hote (jaise agar `count` global hota) — lekin har `outer()` call apna independent `count` banata hai.

### Option B (1, 1, 1)
Galat hai — ye tab hota agar `++count` **har baar reset** ho jaata (jaise agar `count` function ke andar declare hone ki jagah, closure ke bahar reset ho raha ho) — lekin closures **latest updated value** hi "yaad" rakhte hain persistently.

### Option D (0, 1, 0)
Galat hai — ye pattern `count++` (**post-increment**) ka hota, `++count` (**pre-increment**) ka nahi. `++count` **pehle increment karta hai, phir return karta hai** — isliye pehli call `0` nahi, `1` deti hai.

### Option E (0, 1, 1)
Galat hai — same reason, pre-increment ki wajah se pehli call `1` deni chahiye, `0` nahi. Aur `counter2` ka apna independent counter hona chahiye.

---

## Important Exam Notes
- ✅ `++count` (pre-increment) = pehle increment, phir new value return.
- ✅ `count++` (post-increment) = pehle current value return, phir increment.
- ✅ Har `outer()` call apna independent closure/`count` banata hai.
- ⚠️ Common Mistake: Pre-increment (`++x`) aur post-increment (`x++`) ko confuse karna.
- 💡 Trick: "**++ pehle likha ho to pehle hoga (increment), baad me likha ho to baad me hoga**"

---

## Similar Question Pattern
Closures + counters (with pre vs post increment variations) ke trace-output questions bahut common hain — dhyan rakhna hai `++x` vs `x++` ka difference.

---

## Revision Box
Har `outer()` call apna independent closure/`count` banata hai. `++count` (pre-increment) **pehle badhata hai, phir return karta hai**. `counter1`: 0→1(return 1), 1→2(return 2). `counter2`: 0→1(return 1) (independent se). Output: **1, 2, 1**.

---
---

# Question 120

## Original Question
**Code Snippet 1:**
```js
let x = 10;
const y;
```
**Code Snippet 2:**
```js
let { a: b } = undefined;
```
**Code Snippet 3:**
```js
const arr = [1, 2];
arr = [3, 4];
```
**Code Snippet 4:**
```js
const a = 10, b = 20;
(() => { return a + b; })();
```
> Which of the above JavaScript code snippet(s) will throw an error?
>
> Options: A. 1 and 2  B. 1, 2 and 3  C. 2, 3 and 4  D. All 4

---

## Correct Answer
**Correct Option:** B (1, 2 and 3)

---

## Concept Used
- 📘 **Snippet 1 — `const` bina initialization ke:** `const` variable ko declare karte waqt **value dena mandatory** hai. `const y;` (bina `= value`) likhna **SyntaxError** deta hai.
- 📘 **Snippet 2 — Destructuring `undefined`:** Destructuring assignment (`let { a: b } = ...`) ke liye **right-hand side ek valid object (ya array) hona chahiye**. Agar RHS `undefined` (ya `null`) ho, to JavaScript **TypeError** throw karta hai (`Cannot destructure property 'a' of 'undefined' as it is undefined`) — kyunki JS `undefined` se koi property nikaal hi nahi sakta.
- 📘 **Snippet 3 — `const` Array Reassignment:** `const` sirf **binding (variable reference)** ko immutable banata hai, **array/object ke andar ke contents ko nahi**. Isliye `arr.push(3)` jaisi cheez **valid** hoti (contents modify ho rahe hain), lekin `arr = [3, 4];` (poori array ko **naye array se replace** karna, matlab binding ko hi reassign karna) **TypeError** deta hai (`Assignment to constant variable`).
- 📘 **Snippet 4 — Arrow Function IIFE with `const`:** `const a = 10, b = 20;` — Ye **valid multi-variable const declaration** hai (dono `a` aur `b` ko initial value di gayi hai). `(() => { return a + b; })();` — Ye ek valid **arrow function IIFE** hai jo `30` return karta hai, koi error nahi.

---

## Step-by-Step Solution
1. **Snippet 1 Check:** `let x = 10; const y;`
   - `let x = 10;` — Valid, koi problem nahi.
   - `const y;` — **INVALID** — `const` ko declaration ke waqt **value deni hi hoti hai**. Bina value ke ye **SyntaxError** deta hai.
   - **Error: YES** ❌
2. **Snippet 2 Check:** `let { a: b } = undefined;`
   - Ye **destructuring** hai — `undefined` (jo RHS pe hai) se `a` property nikaalne ki koshish ho rahi hai, aur use `b` naam ke variable me daalne ki.
   - `undefined` ke pass koi properties nahi hoti, aur JS destructuring internally `undefined`/`null` ko destructure karne ki koshish karte hi **turant TypeError throw** kar deta hai.
   - **Error: YES** ❌
3. **Snippet 3 Check:** `const arr = [1, 2]; arr = [3, 4];`
   - `const arr = [1, 2];` — Valid, `arr` ek array ko point kar raha hai.
   - `arr = [3, 4];` — Ye `arr` **binding ko poori tarah reassign** karne ki koshish kar raha hai (naye array `[3,4]` se). Kyunki `arr` `const` hai, iski **reference ko reassign nahi kiya ja sakta**.
   - **Error: YES** ❌ (`TypeError: Assignment to constant variable`)
   - *Important Nuance:* Agar `arr.push(3)` ya `arr[0] = 100` likha hota, to ye **valid** hota (kyunki hum array ke **andar ke content ko modify** kar rahe hote, `arr` variable ki reference change nahi kar rahe) — lekin yaha poori array **reassign** ki ja rahi hai, jo error deta hai.
4. **Snippet 4 Check:** `const a = 10, b = 20; (() => { return a + b; })();`
   - `const a = 10, b = 20;` — Valid, dono variables ko properly initialize kiya gaya hai.
   - `(() => { return a + b; })();` — Ye ek valid **arrow function** hai jo turant invoke ho rahi hai (IIFE pattern), `a+b = 10+20 = 30` return karti hai. Koi syntax ya runtime error nahi hai.
   - **Error: NO** ✅ (Ye code **valid** hai, bas result kahi store/print nahi ho raha, lekin error nahi aata)

**Shortcut:** Har snippet ko **individually classify karo** — "const bina value" → error, "destructure undefined/null" → error, "const reassignment" → error, "normal arrow IIFE" → no error.

---

## Final Answer
**"1, 2 and 3"** — ye teeno snippets error throw karenge, Snippet 4 valid hai.

---

## Why Other Options are Wrong?
### Option A (1 and 2)
Incomplete hai — Snippet 3 (`const` array reassignment) ko miss kar raha hai, jo bhi definitely error deta hai.

### Option C (2, 3 and 4)
Galat hai — Snippet 1 (`const y;` bina value ke) ko miss kar raha hai (jo error deta hai), aur galti se Snippet 4 ko include kar raha hai (jo actually **valid** code hai, koi error nahi).

### Option D (All 4)
Galat hai — Snippet 4 completely **valid** JavaScript hai, koi error nahi aata usme.

---

## Important Exam Notes
- ✅ `const variable;` (bina value) = **SyntaxError**.
- ✅ Destructuring `undefined`/`null` = **TypeError**.
- ✅ `const` variable ko **reassign** karna (poori reference change karna) = **TypeError**, lekin uske andar ke **contents modify** karna (jaise array `push`, object property update) = **valid**.
- ⚠️ Common Mistake: `const arr.push()` (valid) aur `arr = [...]` (invalid) ko same samajh lena — dono me bahut fark hai.
- 💡 Trick: "**const = binding fix, content flexible**" (agar array/object hai to content change ho sakta hai, reference nahi).

---

## Similar Question Pattern
Multiple code snippets ke error-identification wale "which will throw error" questions common hain — inme har snippet ko **individually analyze** karna padta hai.

---

## Revision Box
Snippet 1: `const` bina value = **Error**. Snippet 2: destructure `undefined` = **Error**. Snippet 3: `const` array **reassign** (reference change) = **Error** (content modify hota to valid hota). Snippet 4: normal arrow IIFE = **No Error**. Answer: **1, 2 and 3**.

---
---

# Question 121

## Original Question
```html
<div id="app">
  <p>{{ counter }}</p>
  <button @click="updateCounter">Click Me</button>
</div>

<script>
new Vue({
  el: '#app',
  data: {
    counter: 1,
  },
  methods: {
    updateCounter() {
      this.counter *= 2;
    },
  },
});
</script>
```
> What will the "p" tag display when the button with the text "Click Me" is clicked three times?
>
> Options: A. 1  B. 4  C. 8  D. 16

---

## Correct Answer
**Correct Option:** C (8)

---

## Concept Used
- 📘 **Vue Methods aur `*=` Operator:** `this.counter *= 2` ka matlab hai `this.counter = this.counter * 2` — matlab **existing value ko 2 se multiply** karke wapas assign karna.
- 📘 **Sequential Clicks — Multiplicative Growth:** Har click pe `counter` **doubling** hoti hai (previous value ka 2x) — ye **exponential growth pattern** banata hai, addition nahi.

---

## Step-by-Step Solution
1. **Step 1:** Initial state: `counter = 1`.
2. **Step 2: 1st Click** — `updateCounter()` chalta hai: `this.counter *= 2` → `1 * 2 = 2`.
3. **Step 3: 2nd Click:** `this.counter *= 2` → `2 * 2 = 4`.
4. **Step 4: 3rd Click:** `this.counter *= 2` → `4 * 2 = 8`.
5. **Step 5:** Final `counter = 8`, isliye `<p>` tag **"8"** display karega.

**Shortcut:** 3 baar doubling ka matlab hai `initial_value × 2³` = `1 × 8 = 8`.

---

## Final Answer
**8**

---

## Why Other Options are Wrong?
### Option A (1)
Ye initial value hai, kisi bhi click ke baad ki value nahi.

### Option B (4)
Ye sirf **2 clicks** ke baad ki value hai (`1×2×2=4`), teesra click miss ho gaya calculation me.

### Option D (16)
Ye tab hota agar **4 baar** doubling hoti (`1×2×2×2×2=16`), lekin sirf **3 baar** click hua hai.

---

## Important Exam Notes
- ✅ `*=` operator = existing value ko multiply karke reassign karna.
- ✅ Repeated doubling = exponential growth (`2ⁿ` pattern), linear addition nahi.
- ⚠️ Common Mistake: `*=` ko `+=` jaisa treat kar lena (addition kar dena, multiplication nahi).
- 💡 Trick: `n` baar doubling = `initial × 2ⁿ`.

---

## Similar Question Pattern
Vue methods ke sath repeated button clicks aur multiplicative/exponential state changes ke trace-output questions common hain.

---

## Revision Box
`counter *= 2` har click pe **current value ko double** karta hai. 3 clicks: `1→2→4→8`. Final display: **8**.

---
---

# Question 122

## Original Question
```js
let length = 5;

function countLen(item){
    console.log(this.length);
}

const data = [countLen, "Apple", length]

data[0]('Script')
```
> What will be output on browser's console for the above JavaScript code?
>
> Options: A. 6  B. 5  C. 4  D. 3

---

## Correct Answer
**Correct Option:** D (3)

---

## Concept Used
- 📘 **Method-Style Function Call (`this` binding):** Jab kisi function ko `object.method()` ya `array[index]()` ki tarah call kiya jaata hai, `this` us **calling object/array** ko refer karta hai. Yaha `data[0]('Script')` — `countLen` ko `data` array ke through call kiya ja raha hai, isliye `this = data` (poori array).
- 📘 **Array ka `.length` Property:** Har array ke pass ek built-in `.length` property hoti hai jo **elements ki total count** batati hai. `data.length` = kitne elements `data` array me hain.
- 📘 **IMPORTANT — Ye `length` Variable Se Alag Hai:** Outer scope ka `let length = 5;` **totally unrelated** hai `this.length` se. `this.length` array ka **built-in property** hai (jo array ke andar kitne elements hain wo count karta hai), na ki outer `length` **variable** ki value.

---

## Step-by-Step Solution
1. **Step 1:** `let length = 5;` — Ye ek **standalone outer variable** hai, `length` naam ki. Iska koi direct connection nahi hai array ke `.length` property se.
2. **Step 2:** `const data = [countLen, "Apple", length]` — Ye array 3 elements ke sath banta hai:
   - Index 0: `countLen` (function reference)
   - Index 1: `"Apple"` (string)
   - Index 2: `length` — chunki `length` yaha ek **variable reference** hai jiski **current value (5)** array me copy ho jaati hai (primitives value-by-value copy hote hain), array ka teesra element **number `5`** hai.
   - Total array: `[countLen, "Apple", 5]` — **3 elements** hain is array me.
   - *Reason:* JavaScript arrays me **koi bhi type** (functions, strings, numbers) mix kiye ja sakte hain.
3. **Step 3:** `data[0]('Script')` — Ye `countLen` function ko **`data` array ke through call** kar raha hai (`data[0]` matlab `countLen`, phir `(...)` call kar raha hai).
   - Kyunki call **`data.` ke through** ho rahi hai (array-index method-call syntax), `this` = **`data`** (poori array).
   - *Reason:* Method-style call me `this` hamesha "immediate caller object/array" ko refer karta hai.
4. **Step 4:** Function ke andar `console.log(this.length);` execute hota hai. `this = data` (array), isliye `this.length` = `data.length`.
   - `data.length` = array me **kitne elements hain** = **3** (`countLen`, `"Apple"`, `5`).
   - *Reason:* `.length` array ka built-in property hai jo elements count karta hai, koi custom variable nahi.
5. **Step 5:** Output: **3**.

**Shortcut:** Jab bhi `this.length` dikhe kisi array-context ke andar, turant socho — "**Ye array ka `.length` property hai (element count), koi outer variable `length` nahi!**"

---

## Final Answer
**3**

---

## Why Other Options are Wrong?
### Option A (6)
Ye galat hai — koi calculation aisi nahi hai jo `6` de. Ye ek distractor value hai.

### Option B (5)
Ye galat hai — ye outer `let length = 5` ki value hai, lekin `this.length` isse **koi relation nahi** rakhta — `this` array hai, aur array ka `.length` property **elements count** karta hai, outer variable ki value copy nahi karta.

### Option C (4)
Ye galat hai — array me sirf **3 elements** hain (`countLen`, `"Apple"`, `5`), 4 nahi.

---

## Important Exam Notes
- ✅ `this.length` array-context me = **array ke elements ki count**, koi outer variable nahi.
- ✅ Method-style call (`data[0]()`) → `this` = **calling array/object**.
- ⚠️ Common Mistake: `this.length` ko outer `length` variable se confuse kar dena — naam same hone ka matlab connection nahi hota.
- 💡 Trick: "**`this.length` array me = element count, hamesha!**"

---

## Similar Question Pattern
`this` binding + array's built-in `.length` property ke combination wale trap questions common hain — dhyan rakhna hai variable naam aur property naam ke coincidental match se confuse na hona.

---

## Revision Box
`data` array me 3 elements hain: `[countLen, "Apple", 5]` (outer `length` variable ki value 5 as a copy). `data[0]('Script')` call `this = data` set karta hai (method-style call). `this.length` = **array ka elements count = 3**, outer `length` variable (5) se koi relation nahi.

---
---

# Question 123

## Original Question
```js
var value = 50;

const mainObj = {
    value: 42,
    getValue: function() {
      return this.value;
    }
};

const nextObj = {
value: 100
};

const getValue1 = mainObj.getValue.bind()(nextObj);
const getValue2 = mainObj.getValue.bind(nextObj)();

console.log(getValue1);
console.log(getValue2);
```
> What will be output on browser's console for the above JavaScript code?

---

## Correct Answer
**Correct Output:**
```
undefined
100
```

---

## Concept Used
- 📘 **`.bind()` — Permanent `this` Locking:** `.bind(thisArg)` ek **naya function** return karta hai jiska `this` **permanently `thisArg`** pe lock ho jaata hai. Ye lock **kabhi bhi override nahi ho sakta**, chahe function ko baad me kaise bhi call karo.
- 📘 **`.bind()` Bina Kisi Argument Ke:** `mainObj.getValue.bind()` — Yaha `bind()` ko **koi argument nahi diya gaya** (empty), matlab `this` **explicitly `undefined`** pe bind ho gaya hai. Function ke andar `this` **kisi bhi valid object ko point nahi** karega.
- 📘 **Extra Arguments After Bind Are Just Function Arguments, Not `this`:** `mainObj.getValue.bind()(nextObj)` — Yaha `bind()` ke baad `(nextObj)` **function ko call** kar raha hai, `nextObj` ko `this` ki jagah ek **normal function argument** ki tarah pass kiya ja raha hai (jo `getValue` use hi nahi karta, kyunki `getValue` koi parameter accept hi nahi karta). Ye `this` ko **change nahi karta** — `this` already `bind()` se lock ho chuka tha.
- 📘 **`.bind(nextObj)()`:** Yaha `.bind(nextObj)` explicitly `this = nextObj` set karta hai — ye **valid binding** hai, isliye function `nextObj.value` return karega.

---

## Step-by-Step Solution
1. **Step 1: `getValue1` calculate karo:**
   - `mainObj.getValue.bind()` — `bind()` **koi argument nahi** le raha, isliye `this` **`undefined`** pe permanently bind ho jaata hai (kisi specific object se connect nahi hota).
   - `(nextObj)` — Ye poore bound function ko **call** kar raha hai, `nextObj` ko as an **argument** pass kar raha hai (jo function use nahi karta) — ye `this` ko **change nahi karta**.
   - Function ke andar `return this.value;` execute hota hai — `this` valid object ko point nahi karta (bind() empty tha), isliye `this.value` access karne ki koshish **`undefined`** deti hai (is context me `this` kisi bhi meaningful object se connected nahi hai, isliye `.value` property nahi milti).
   - **`getValue1 = undefined`**
   - *Reason:* Jab `bind()` ko **bina argument** ke call kiya jaata hai, `this` kisi specific object se **connect nahi hota** — isliye property access karne par **`undefined`** milta hai.
2. **Step 2: `getValue2` calculate karo:**
   - `mainObj.getValue.bind(nextObj)` — `bind(nextObj)` explicitly `this = nextObj` **permanently set** kar deta hai.
   - `()` — Bound function ko **call** kiya ja raha hai bina kisi extra argument ke.
   - Function ke andar `return this.value;` — `this = nextObj`, isliye `this.value` = `nextObj.value` = **`100`**.
   - **`getValue2 = 100`**
   - *Reason:* `bind(nextObj)` ne explicitly `this` ko `nextObj` set kar diya, isliye function correctly `nextObj` ki `value` property return karta hai.
3. **Step 3:** Final output:
   ```
   undefined
   100
   ```

**Shortcut:** Yaad rakho — "**`.bind()` ke andar diya gaya argument hi `this` decide karta hai, bind() ke BAAD `()` me diye gaye arguments sirf normal function parameters hote hain, `this` ko touch nahi karte.**"

---

## Final Answer
```
undefined
100
```

---

## Why Other Options are Wrong?
### Option (50, 100)
Galat hai — pehli value `50` galat hai. Ye tab expect kiya ja sakta hai agar `this` (bind() empty hone ke baad) kisi tarah global/outer scope se connect ho jaata — lekin is exam context me `bind()` **bina argument ke** `this` ko kisi bhi meaningful object se disconnect kar deta hai, isliye property access `undefined` deta hai.

### Option (42, 50)
Galat hai — `42` galat hai kyunki `mainObj.value` **kabhi access hi nahi hota** is calculation me. `50` bhi galat hai `getValue2` ke liye — `getValue2` `nextObj` (`bind(nextObj)`) se explicitly bound hai, isliye `nextObj.value=100` hi milega.

### Option (100, 100)
Galat hai — pehli value `100` galat hai. `getValue1` me `nextObj` sirf ek **normal function argument** ki tarah pass hua hai (jo istemal hi nahi ho raha function body me), `this` ki tarah **nahi** — isliye `this.value` `nextObj.value` nahi de sakta.

---

## Important Exam Notes
- ✅ `.bind(thisArg)` — `bind()` ke andar diya gaya argument hi `this` set karta hai, **permanently**.
- ✅ `.bind()` ke **baad** `()` me diye gaye arguments sirf **normal function parameters** hain, `this` ko affect nahi karte.
- ✅ `.bind()` bina kisi argument ke call karna `this` ko kisi valid object se **disconnect** kar deta hai — property access karne par `undefined` milta hai.
- ⚠️ Common Mistake: Sochna ki `.bind()(someObject)` `someObject` ko `this` bana dega — galat hai, `this` sirf `.bind()` ke **andar diye gaye argument** se set hota hai.
- 💡 Trick: "**bind(X) = this is X, forever. Extra () args after bind = normal function parameters, NOT this.**"

---

## Similar Question Pattern
`.bind()` ke saath extra arguments/empty calls ke combination wale trap questions common hain — inme dhyan rakhna hai ki **`this` sirf `bind()` ke andar hi decide hota hai**.

---

## Revision Box
`mainObj.getValue.bind()` — empty bind, `this` kisi object se connect nahi hota → property access `undefined` deta hai. `(nextObj)` ke through call karna sirf ek **normal argument** pass karta hai, `this` change nahi karta → `getValue1 = undefined`. `mainObj.getValue.bind(nextObj)()` — explicit bind, `this=nextObj` → `getValue2 = nextObj.value = 100`.

---
---

# Question 124

## Original Question
```js
let Tree = {
    name: "Oak",
    size: 5,
    description: function (size) {
        return `A ${this.size > 10 ? 'large' : 'medium'} ${this.name} tree.`;
    }
}

const willow = {
    name: 'Willow'
};

const returnedString = Tree.description.call(willow, 15);

console.log(returnedString)
```
> What will be the output of the code?
>
> Options: A. A large Oak tree  B. A medium Oak tree  C. A large Willow tree  D. A medium Willow tree

---

## Correct Answer
**Correct Option:** D (A medium Willow tree)

---

## Concept Used
- 📘 **`.call(thisArg, arg1, arg2, ...)`:** Ye method ek function ko **turant call** karta hai, explicitly `this` set karke (`thisArg`), aur baaki arguments **normal function parameters** ki tarah pass hote hain.
- 📘 **IMPORTANT TRAP — Parameter `size` vs Property `this.size`:** `description(size)` function ka ek **parameter** hai jiska naam `size` hai. Lekin function ke **body** ke andar `this.size` use ho raha hai — jo **object property** hai, **local parameter `size` nahi**! Dono alag cheezein hain, sirf naam **coincidentally same** hai.
- 📘 **`willow` Object me `size` Property Nahi Hai:** `willow = { name: 'Willow' }` — is object me **sirf `name`** hai, **`size` property missing** hai. Isliye jab `this = willow` set hota hai (`call()` ke through), `this.size` → `willow.size` → **`undefined`** (kyunki property exist hi nahi karti).

---

## Step-by-Step Solution
1. **Step 1:** `Tree.description.call(willow, 15);` — Ye `description` function ko call kar raha hai, `this = willow` (explicitly set `.call()` se), aur `15` ko **parameter `size`** ki value ki tarah pass kar raha hai.
   - *Reason:* `.call(thisArg, arg1)` — pehla argument `this`, doosra function ka **first parameter**.
2. **Step 2:** Function ke andar dekho — parameter `size` **`15`** hai (jo pass hua), lekin function body me **`this.size` use ho raha hai, local `size` parameter NAHI**!
   - *Reason:* Ye ek **classic naming trap** hai — `size` parameter aur `this.size` property **do alag cheezein** hain, sirf naam match karta hai.
3. **Step 3:** `this.size` calculate karo — `this = willow` (Step 1 se). `willow` object me **`size` property define hi nahi** hai (`willow = { name: 'Willow' }` — sirf `name` hai).
   - `this.size` → `willow.size` → **`undefined`** (property exist nahi karti).
4. **Step 4:** Ternary condition evaluate karo: `this.size > 10 ? 'large' : 'medium'`
   - `undefined > 10` → hamesha **`false`** hota hai (undefined ki koi numeric comparison hamesha false deti hai).
   - Isliye condition **false**, result = **`'medium'`**.
5. **Step 5:** `this.name` calculate karo — `this = willow`, `willow.name = 'Willow'`.
6. **Step 6:** Template literal assemble karo: `` `A ${medium} ${Willow} tree.` `` → **`"A medium Willow tree."`**

**Shortcut:** Jab bhi function parameter ka naam **object property** ke naam se **match** kare, turant check karo function body me **kaunsa use ho raha hai** — `this.propName` (object property) ya sirf `propName` (local parameter). Ye ek common exam trap hai.

---

## Final Answer
**"A medium Willow tree."**

---

## Why Other Options are Wrong?
### Option A (A large Oak tree)
Galat hai — `this` **`willow`** hai (`call(willow, ...)` ki wajah se), `Tree`/`Oak` nahi. Aur `this.size` `undefined` hai, isliye "large" nahi ban sakta.

### Option B (A medium Oak tree)
"medium" sahi hai, lekin **"Oak" galat** hai — `this.name` = `willow.name` = `"Willow"` honi chahiye, `"Oak"` nahi.

### Option C (A large Willow tree)
"Willow" sahi hai, lekin **"large" galat** hai — `this.size` (`willow.size`) `undefined` hai, jo ternary condition me `false` deta hai, isliye "medium" hona chahiye.

---

## Important Exam Notes
- ✅ `.call(thisArg, arg1, ...)` — pehla argument `this` set karta hai, baaki normal parameters.
- ✅ Function parameter aur `this.propertyName` **bilkul alag** cheezein hain, chahe naam same ho.
- ✅ `undefined` ke sath koi bhi numeric comparison (`>`, `<`) hamesha **`false`** deta hai.
- ⚠️ Common Mistake: Function body me local parameter `size` use ho raha hoga assume kar lena — lekin actually `this.size` (property) use ho raha hai.
- 💡 Trick: "**Function ke andar `this.x` dikhe to hamesha object property hai, parameter `x` nahi — chahe naam kitna bhi same lage!**"

---

## Similar Question Pattern
`.call()`/`.apply()` ke saath parameter-naming traps (jaha local parameter aur `this.property` same naam share karte hain) common hain — dhyan se function body padhna zaroori hai.

---

## Revision Box
`.call(willow, 15)` → `this=willow`. Function body **`this.size` use karta hai** (`15` wala local parameter `size` **kabhi use nahi hota** body me!). `willow.size = undefined` → ternary condition false → "medium". `this.name = willow.name = "Willow"`. Final: **"A medium Willow tree."**

---
---

# Question 125

## Original Question
```html
<div id="app">
  <div :class="{ active: isActive, 'text-danger': hasError }">
    {{ message }}
  </div>
</div>

<script>
  new Vue({
    el: '#app',
    data: {
      message: 'Hello!',
      isActive: true,
      hasError: false
    }
  })
</script>
```
> Given the Vue.js application above, which classes will be applied to the div containing the message?
>
> Options: A. Only 'active'  B. 'text-danger' and 'active'  C. No classes  D. Only 'text-danger'

---

## Correct Answer
**Correct Option:** A (Only 'active')

---

## Concept Used
- 📘 **Object Syntax for `:class` (Conditional Class Binding):** Vue me `:class="{ className: condition }"` **object syntax** allow karta hai — jahan **object ki har key ek class name** hai, aur **value ek boolean condition** hai. Agar condition `true` ho, to class **apply** hoti hai; agar `false` ho, to **nahi**.

**Example:**
```js
:class="{ active: isActive, error: hasError }"
// Agar isActive=true, hasError=false → sirf "active" class apply hogi
```

---

## Step-by-Step Solution
1. **Step 1: Data values check karo:**
   - `isActive = true`
   - `hasError = false`
2. **Step 2: `active: isActive` check karo:**
   - `isActive = true` → condition **true** → `active` class **apply** hogi.
3. **Step 3: `'text-danger': hasError` check karo:**
   - `hasError = false` → condition **false** → `text-danger` class **apply nahi** hogi.
4. **Step 4: Final classes combine karo:**
   - Sirf **`active`** class apply hogi.

**Shortcut:** Object syntax me `:class="{className: condition}"` — sirf **`true`** wali conditions ki classes final list me aati hain, `false` wali skip ho jaati hain.

---

## Final Answer
**Only 'active'**

---

## Why Other Options are Wrong?
### Option B ('text-danger' and 'active')
Galat hai — `text-danger` apply **nahi** honi chahiye kyunki `hasError = false` hai (condition false).

### Option C (No classes)
Galat hai — `active` class definitely apply hogi kyunki `isActive = true` hai.

### Option D (Only 'text-danger')
Galat hai — `text-danger` ki condition (`hasError`) **false** hai, isliye ye apply hi nahi hogi. Ye option `active` (jiski condition true hai) ko bhi miss kar raha hai.

---

## Important Exam Notes
- ✅ `:class="{className: condition}"` — sirf `true` conditions ki classes apply hoti hain.
- ✅ Multiple classes ek sath conditionally control ki ja sakti hain object syntax se.
- ⚠️ Common Mistake: Saari classes ko apply maan lena bina unki individual conditions check kiye.
- 💡 Trick: Har class ke aage uski condition ki **current boolean value** likho, sirf `true` wali final list me rakho.

---

## Similar Question Pattern
Vue `:class` object syntax ke conditional binding wale trace-based questions common hain.

---

## Revision Box
`:class="{active: isActive, 'text-danger': hasError}"` — `isActive=true` (apply), `hasError=false` (skip). Final applied class: **Only 'active'**.

---
---

# Question 126

## Original Question
**index.html:**
```html
<body>
    <div id="app">
        <h4>{{ state }}</h4>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>
    <script src="./script.js"></script>
</body>
```
**script.js:**
```js
const app = new Vue({
    el: '#app',
    data: {
        terminal: ['3','2','1'],
        curr_status: ["arrived", "departed", "delayed"]
    },
    computed: {
        state: () => {
            return `The flight 103 has ${this.curr_status[1]} from terminal ${this.terminal[2]}.`;
        }
    }
})
```
> What will be the output on the browser?
>
> Options:
> A. The flight 103 has departed from terminal 1.
> B. The flight 103 has arrived from terminal 2.
> C. The flight 103 has delayed from terminal 3.
> D. The flight 103 has from terminal.
> E. ReferenceError

---

## Correct Answer
**Correct Option:** E (ReferenceError — conceptually ye `this` binding failure hai arrow-function computed property ke andar)

---

## Concept Used
- 📘 **Arrow Function as Computed Property — Ek Bahut Bada Trap:** `state: () => {...}` **arrow function** ki tarah define kiya gaya hai computed property ke roop me. Arrow functions ka **apna `this` nahi hota** — wo apne **lexical/enclosing scope** ka `this` "borrow" karte hain.
- 📘 **Vue ke Andar Regular Function vs Arrow Function ka Difference:** Jab computed property **regular function** (`function(){...}` ya ES6 shorthand `state() {...}`) ki tarah likhi jaati hai, Vue internally is function ko is tarah call karta hai ki `this` **Vue instance (`vm`)** ko refer kare — isliye `this.curr_status`, `this.terminal` sahi tarike se Vue ke `data` properties ko access kar paate hain.
- 📘 **Lekin Arrow Function Is Binding Ko "Todh" Deta Hai:** Kyunki `state` yaha **arrow function** hai, iska `this` **kabhi bhi Vue instance nahi banega** — chahe Vue kitni bhi koshish kare, arrow function ka `this` uske **definition ki jagah ke lexical scope** se hi aayega (jo yaha top-level/module scope hai, `script.js` file ke andar, jaha `this` Vue instance se **bilkul unrelated** hai).
- 📘 **Result — `this.curr_status` Access Karne Par Problem:** Kyunki `this` Vue instance nahi hai, `this.curr_status` **`undefined`** hoga. Jab `undefined[1]` access karne ki koshish hoti hai (`this.curr_status[1]`), JavaScript **runtime error** throw karta hai.

---

## Step-by-Step Solution
1. **Step 1:** `computed: { state: () => {...} }` — Dhyan do, `state` **arrow function syntax** (`() => {...}`) me likha gaya hai, na ki regular function shorthand (`state() {...}`).
   - *Reason:* Ye syntax difference bahut important hai — Vue ke computed properties ko **regular functions** hone chahiye taaki Vue unka `this` **apne instance se bind** kar sake.
2. **Step 2:** Jab template `{{ state }}` render hone ki koshish karta hai, Vue is `state` computed property ko **evaluate** karta hai — is arrow function ko call karta hai.
3. **Step 3:** Arrow function ke andar `this` — kyunki ye **arrow function** hai, `this` **Vue instance nahi** banega. Iski jagah, `this` **lexical scope** se aayega — jo `script.js` file ke **top-level/module scope** ka `this` hai.
   - *Reason:* Arrow functions kabhi bhi apna `this` nahi banate, hamesha **enclosing scope** se lete hain — Vue ki internal "this-binding magic" arrow functions pe **kaam nahi karti**.
4. **Step 4:** `this.curr_status` access karne ki koshish hoti hai — kyunki `this` Vue instance nahi hai, **`curr_status`** property wahan **exist nahi karti**.
   - `this.curr_status` → **`undefined`**.
5. **Step 5:** `this.curr_status[1]` — Ab `undefined[1]` access karne ki koshish ho rahi hai. Isse **runtime error** aata hai (property access `undefined` pe karna).
6. **Step 6:** Isliye poora computed property evaluation **fail** ho jaata hai, aur browser me ek **error** dikhta hai (is exam ke answer key ke hisaab se, ise "ReferenceError" ki tarah classify kiya gaya hai).

**Shortcut:** Yaad rakho — "**Vue me computed properties/methods KABHI BHI arrow function ki tarah mat likho** — hamesha regular function (`function(){}` ya shorthand `methodName(){}`) use karo, taaki Vue `this` ko sahi tarike se apne instance se bind kar sake."

---

## Final Answer
**Error occurs** (arrow function computed property ki wajah se `this` Vue instance ko refer nahi karta, isliye `this.curr_status` access karne se error aata hai).

---

## Why Other Options are Wrong?
### Option A, B, C (specific flight status/terminal combinations)
Ye saare galat hain kyunki ye tab valid hote agar `state` computed property **regular function** hoti (jisme `this` sahi tarike se Vue instance ko refer karta). Lekin arrow function hone ki wajah se `this` binding **fail** ho jaati hai, isliye koi valid string output nahi milta.

### Option D (partial/incomplete string)
Galat hai — `this.curr_status[1]` jaisa access **poori tarah error throw karta hai** execution ko rok kar, isliye partial string bhi nahi milega.

---

## Important Exam Notes
- ✅ Vue computed properties/methods **kabhi bhi arrow function** ki tarah mat likho — `this` binding toot jaati hai.
- ✅ Regular function syntax (`function(){}` ya ES6 method shorthand) use karo taaki Vue `this` ko instance se sahi tarike se bind kare.
- ⚠️ Common Mistake: Arrow functions ko "modern/better syntax" samajh kar Vue ke computed/methods/watch me use kar lena — ye ek **bahut common aur dangerous bug** hai real Vue development me bhi.
- 💡 Trick: "**Vue + Arrow Function (for methods/computed) = `this` Disaster!**" — hamesha regular function syntax use karo Vue options ke andar.

---

## Similar Question Pattern
Vue computed properties/methods me arrow function ka galat use, aur uske `this` binding pe impact, ye ek **classic Vue "gotcha"** hai jo exam me baar-baar test hota hai.

---

## Revision Box
Computed property `state: () => {...}` **arrow function** hai — iska `this` Vue instance **nahi** hai (lexical scope se aata hai, Vue ki this-binding magic isse skip ho jaati hai). `this.curr_status` → `undefined`, phir `undefined[1]` access karna → **error**. **Lesson: Vue methods/computed me hamesha regular function syntax use karo, arrow function nahi.**

---
---

# Question 127

## Original Question
**index.html:**
```html
<body>
    <div id="app">
        <my-comp>
            <h3>Exploring Vue JS</h3>
            <template v-slot:ongoing>
                <h3>Learning App Dev 2</h3>
            </template>
        </my-comp>
    </div>
    <script src="./script.js"></script>
</body>
```
**script.js:**
```js
const MyComp = {
    name: 'my-comp',
    props: ['tech'],
    template: `<div class="container">
                    <slot name="complete"><h3>Learned DBMS</h3></slot>
                    <slot name="ongoing"></slot>
                    <slot>Exploring Frontend</slot>
                </div>`
}

const app = new Vue({
    el: '#app',
    components: {
        MyComp
    }
})
```
> What will be rendered on the browser for code setup given above?

---

## Correct Answer
**Correct Output:**
```
Learned DBMS
Learning App Dev 2
Exploring Vue JS
```

---

## Concept Used
- 📘 **Named Slots:** `<slot name="xyz">` child component ke template me ek **specific placeholder** banata hai jaha parent `v-slot:xyz` ke through content de sakta hai.
- 📘 **Default (Unnamed) Slot:** `<slot>...</slot>` (bina naam ke) — parent jo bhi content **bina `v-slot` label ke** deta hai, wo yahan aata hai.
- 📘 **Fallback Content:** Slot tag ke andar likha gaya content **fallback** hota hai — sirf tab dikhta hai jab parent us specific slot ke liye **koi content na de**. Agar parent content deta hai, to wo fallback ko **replace** kar deta hai.
- 📘 **Teen Slots Is Component Me:**
  1. `<slot name="complete">` — fallback: `<h3>Learned DBMS</h3>`
  2. `<slot name="ongoing">` — koi fallback nahi (empty)
  3. `<slot>` (unnamed/default) — fallback: `Exploring Frontend`

---

## Step-by-Step Solution
1. **Step 1: Parent (index.html) me diya gaya content dekho:**
   - `<h3>Exploring Vue JS</h3>` — koi `v-slot` naam nahi diya, isliye ye **default/unnamed slot** ke liye content hai.
   - `<template v-slot:ongoing><h3>Learning App Dev 2</h3></template>` — ye `v-slot:ongoing` label ke sath diya gaya hai, isliye ye **"ongoing" named slot** ke liye content hai.
2. **Step 2: "complete" slot ke liye check karo** — Parent ne is naam ka **koi content nahi diya**, isliye is slot ka **fallback** dikhega: **"Learned DBMS"**.
3. **Step 3: "ongoing" named slot ke liye check karo** — Parent ne `v-slot:ongoing` se `<h3>Learning App Dev 2</h3>` diya hai, isliye ye is slot me **render** hoga.
   - Result: **"Learning App Dev 2"**
4. **Step 4: Default/unnamed slot ke liye check karo** — Parent ne `<h3>Exploring Vue JS</h3>` diya hai bina kisi naam ke, isliye ye **default slot ka fallback ("Exploring Frontend") replace** kar dega.
   - Result: **"Exploring Vue JS"** dikhega, "Exploring Frontend" **nahi**.
5. **Step 5: Final Rendered Content (component template ke order me — complete, ongoing, default):**
   ```
   Learned DBMS
   Learning App Dev 2
   Exploring Vue JS
   ```

**Shortcut:** Component template me slots jis **order** me likhe hain, output bhi **usi order** me render hota hai — har slot ke liye check karo parent ne content diya ya fallback use hoga.

---

## Final Answer
**"Learned DBMS", "Learning App Dev 2", "Exploring Vue JS"** (is order me)

---

## Why Other Options are Wrong?
### Option (Learning App Dev 2 / Exploring Vue JS — sirf 2 lines)
Galat hai — "Learned DBMS" (complete slot ka fallback) ko **miss** kar raha hai, jabki parent ne "complete" slot ke liye koi content nahi diya, isliye fallback zaroor dikhna chahiye.

### Option (Learning App Dev 2 / Exploring Frontend)
Galat hai — "Exploring Frontend" (default slot ka fallback) **nahi** dikhna chahiye, kyunki parent ne default slot ke liye explicitly content diya hai, jo fallback ko replace kar deta hai.

### Option (Learned DBMS / Learning App Dev 2 / Exploring Frontend)
"Learned DBMS" aur "Learning App Dev 2" sahi hain, lekin **"Exploring Frontend" galat** hai — ye default slot ka fallback hai, jo replace ho jaana chahiye tha parent ke diye gaye content ("Exploring Vue JS") se.

---

## Important Exam Notes
- ✅ Named slot: `<slot name="x">` — parent `v-slot:x` se content deta hai.
- ✅ Default slot: `<slot>` — parent bina naam ke seedha content de sakta hai.
- ✅ Fallback content sirf tab dikhta hai jab parent koi content na de us slot ke liye.
- ⚠️ Common Mistake: Fallback aur parent ka content dono ek sath render honge sochna — actually parent ka content fallback ko **completely replace** karta hai.
- 💡 Trick: "Parent gives content for a slot? Fallback goes away for THAT slot only!"

---

## Similar Question Pattern
Vue slots (named, default, fallback content) ke rendering-output questions frequently aate hain — dhyan se dekhna hota hai kaunsa content kis slot me map ho raha hai.

---

## Revision Box
3 slots: "complete" (fallback used — no parent content — "Learned DBMS"), "ongoing" (parent content via `v-slot:ongoing` — "Learning App Dev 2"), default (parent content replaces fallback — "Exploring Vue JS", not "Exploring Frontend"). Order follows component template structure.

---
---

# Question 128

## Original Question
```html
<div id="app1">
  {{ messageA }}
</div>
<div id="app2">
  {{ messageB }}
</div>

<script>
  const vm1 = new Vue({
    el: '#app1',
    data: {
      messageA: 'Hello from App 1'
    }
  })

  const vm2 = new Vue({
    el: '#app2',
    data: {
      messageB: 'Hello from App 2'
    }
  })

  vm1.messageA = 'Updated App 1';
</script>
```
> After the code execution above, what will be displayed on the page?
>
> Options:
> A. "Updated App 1" and "Hello from App 2"
> B. "Hello from App 1" and "Hello from App 2"
> C. "Updated App 1" and blank second div
> D. Both divs will be blank

---

## Correct Answer
**Correct Option:** A (Updated App 1 and Hello from App 2)

---

## Concept Used
- 📘 **Multiple Independent Vue Instances:** Ek hi page pe **multiple `new Vue({...})` instances** banayi ja sakti hain, har ek **apne alag `el` (mount point)** ke sath. Ye instances **completely independent** hoti hain — ek instance ka data doosri instance ko **affect nahi karta**.
- 📘 **Reactivity Applies Even After Creation:** Vue instance ka data **reactive** hota hai — matlab agar aap instance banne ke **baad bhi** uski data property ko directly update karo (jaise `vm1.messageA = '...'`), Vue ye change **detect** kar leta hai aur DOM ko automatically update kar deta hai.

---

## Step-by-Step Solution
1. **Step 1:** `vm1` create hota hai `#app1` element pe mount hoke, `messageA = 'Hello from App 1'` ke sath. `#app1` div **"Hello from App 1"** dikhayega initially.
2. **Step 2:** `vm2` create hota hai `#app2` element pe mount hoke, `messageB = 'Hello from App 2'` ke sath. `#app2` div **"Hello from App 2"** dikhayega — **`vm1` se bilkul independent**.
3. **Step 3:** `vm1.messageA = 'Updated App 1';` — Ye `vm1` ki `messageA` **reactive data property** ko **directly update** kar raha hai.
   - *Reason:* Vue instance object khud reactive data properties ko **directly expose** karta hai, aur inhe update karna **valid aur reactive** operation hai.
4. **Step 4:** Kyunki `messageA` reactive hai, Vue is change ko **detect** karta hai aur `#app1` div ke andar ka content automatically **"Updated App 1"** me update kar deta hai.
5. **Step 5:** `vm2` ka koi data touch nahi hua hai, isliye `#app2` div **"Hello from App 2"** hi dikhata rahega, unaffected.

**Shortcut:** Multiple Vue instances **hamesha independent** hoti hain jab tak explicitly connect na ki jaayein — ek instance ka data update doosri ko touch nahi karta.

---

## Final Answer
**"Updated App 1" and "Hello from App 2"**

---

## Why Other Options are Wrong?
### Option B (Hello from App 1 and Hello from App 2)
Galat hai — Ye assume kar raha hai ki `vm1.messageA = 'Updated App 1'` **kaam nahi karega** — lekin Vue instance ke data properties **directly reactive** hote hain, update karna valid hai aur DOM turant reflect karta hai.

### Option C (Updated App 1 and blank second div)
Galat hai — `vm2` ka data kabhi touch hi nahi hua, isliye `#app2` **blank** hone ka koi reason nahi hai.

### Option D (Both divs blank)
Galat hai — dono Vue instances **properly mount** hui hain apne respective elements pe, aur unka data valid hai.

---

## Important Exam Notes
- ✅ Multiple Vue instances ek page pe **independent** hoti hain.
- ✅ Vue instance ki data properties **direct access se update** ki ja sakti hain, aur ye **reactive** rehta hai.
- ⚠️ Common Mistake: Sochna ki instance creation ke **baad** data update karna reactivity break kar dega.
- 💡 Trick: "Multiple Vue instances = Multiple independent islands — koi connection nahi jab tak explicitly na banaya jaaye."

---

## Similar Question Pattern
Multiple Vue instances ke independence, aur instance creation ke baad data update karne ki reactivity, common conceptual questions hain.

---

## Revision Box
`vm1` aur `vm2` **independent Vue instances** hain apne alag `el` ke sath. `vm1.messageA` ko baad me update karna **reactive** hai. `vm2` unaffected rehta hai. Final: **"Updated App 1" and "Hello from App 2"**.

---
---

# Question 129

## Original Question
```html
<div id="app">
        <input v-model.number="price" type="number">
        <input v-model.number="quantity" type="number">
        <p>Total: {{ total }}</p>
        <p>Last Updated By: {{ lastUpdatedBy }}</p>
</div>

<script>
    new Vue({
        el: '#app',
        data: {
            price: 10,
            quantity: 1,
            total: 10,
            lastUpdatedBy: 'initial'
        },
        watch: {
            price: {
                handler(newVal) {
                    this.total = newVal * this.quantity;
                    this.lastUpdatedBy = 'price';
                }
            },
            quantity: {
                handler(newVal) {
                    this.total = this.price * newVal;
                    this.lastUpdatedBy = 'quantity';
                }
            }
        }
    })
</script>
```
> If the user changes the price to 20 and then immediately changes the quantity to 2, what will be the final values of total and lastUpdatedBy?
>
> Options:
> A. total: 20, lastUpdatedBy: "price"
> B. total: 40, lastUpdatedBy: "quantity"
> C. total: 20, lastUpdatedBy: "quantity"
> D. total: 40, lastUpdatedBy: "price"

---

## Correct Answer
**Correct Option:** B (total: 40, lastUpdatedBy: "quantity")

---

## Concept Used
- 📘 **`v-model.number`:** Ye modifier input value ko automatically **number** me type-cast kar deta hai.
- 📘 **Separate Watchers (Not Combined):** Is code me `price` aur `quantity` ke liye **do alag watchers** hain — dono **independently** apne-apne trigger hone par chalte hain, sequentially.
- 📘 **Sequential State Updates — Order Matters:** Jab `price` change hota hai, uska watcher **turant** chal jaata hai (us waqt ki `quantity` ki **current value** use karke). Jab **uske baad** `quantity` change hota hai, uska watcher chalta hai (us waqt ki `price` ki **naye, updated value** use karke).

---

## Step-by-Step Solution
1. **Step 1:** Initial state: `price=10`, `quantity=1`, `total=10`, `lastUpdatedBy='initial'`.
2. **Step 2: User `price` ko `20` karta hai** — `price` watcher **turant trigger** hota hai:
   - `handler(newVal=20)` chalta hai.
   - `this.total = newVal(20) * this.quantity` — is waqt `quantity` **abhi bhi `1`** hai.
   - `this.total = 20 * 1 = 20`.
   - `this.lastUpdatedBy = 'price'`.
3. **Step 3: Ab state hai:** `price=20`, `quantity=1`, `total=20`, `lastUpdatedBy='price'`.
4. **Step 4: User "immediately" `quantity` ko `2` karta hai** — `quantity` watcher **trigger** hota hai:
   - `handler(newVal=2)` chalta hai.
   - `this.total = this.price * newVal(2)` — is waqt `this.price` **already updated hoke `20`** hai.
   - `this.total = 20 * 2 = 40`.
   - `this.lastUpdatedBy = 'quantity'`.
5. **Step 5:** Final state: `total = 40`, `lastUpdatedBy = 'quantity'`.

**Shortcut:** Jab bhi **sequential changes** ho rahe hon, turant socho — "**jab B ka watcher chalega, A ki VALUE already naye state me hogi (kyunki A ka watcher pehle chal chuka tha)**".

---

## Final Answer
**"total: 40, lastUpdatedBy: 'quantity'"**

---

## Why Other Options are Wrong?
### Option A (total: 20, lastUpdatedBy: "price")
Galat hai — Ye sirf **price change hone ke turant baad** ki state hai, quantity change ko consider nahi kiya gaya.

### Option C (total: 20, lastUpdatedBy: "quantity")
Galat hai — `lastUpdatedBy` sahi hai, lekin `total` **galat** hai — quantity watcher ke andar `this.price` **already updated (20)** hona chahiye tha.

### Option D (total: 40, lastUpdatedBy: "price")
`total` sahi hai, lekin **`lastUpdatedBy` galat** hai — `quantity` watcher **sabse aakhri** me chala, isliye `lastUpdatedBy` **"quantity"** hona chahiye.

---

## Important Exam Notes
- ✅ Separate watchers **independently aur sequentially** fire hote hain.
- ✅ Ek watcher ke andar dusri property ki **current/latest value** use hoti hai.
- ⚠️ Common Mistake: Sochna ki dono watchers "same time" pe purani values use karenge.
- 💡 Trick: Har change ke baad **poori state ka snapshot** likho (table banao).

| Step | Action | price | quantity | total | lastUpdatedBy |
|---|---|---|---|---|---|
| Initial | - | 10 | 1 | 10 | 'initial' |
| price→20 | price watcher fires | 20 | 1 | 20 (=20×1) | 'price' |
| quantity→2 | quantity watcher fires | 20 | 2 | 40 (=20×2) | 'quantity' |

---

## Similar Question Pattern
Multiple separate watchers ke sequential trigger order, aur ek watcher ke andar dusri property ki "current state" dependency ke trace-output questions high-mark me common hain.

---

## Revision Box
Price change hota hai pehle → price watcher fires with `quantity` **still old (1)** → `total=20`. Quantity change hota hai baad me → quantity watcher fires with `price` **already updated (20)** → `total=40`. Final: **total: 40, lastUpdatedBy: "quantity"**.

---
---

# Question 130

## Original Question
```js
class Person {
    constructor(name) {
        this.name = name;
    }

    greet() {
        console.log(`Hello, my name is ${this.name}`);
    }

    static identify() {
        console.log("I am a Person class");
    }
}

const john = new Person("John");
```
> Which of the following expression(s) will cause an error?
>
> Options:
> A. `john.greet();`
> B. `Person.greet();`
> C. `john.identify();`
> D. `Person.identify();`
> E. `console.log(john.name);`

(Multiple Select Question, Correct Marks: 3)

---

## Correct Answer
**Correct Options:** B (`Person.greet();`) and C (`john.identify();`)

---

## Concept Used
- 📘 **Instance Methods (`greet()`):** Regular methods jo class body me define hote hain (bina `static` keyword ke) — ye **sirf instances** (objects created via `new`) pe hi call ki ja sakti hain, **class pe directly nahi**.
- 📘 **Static Methods (`static identify()`):** `static` keyword se define kiye gaye methods **sirf class pe directly** call kiye ja sakte hain, **kisi instance pe nahi**.
- 📘 **Key Rule:**
  - Instance method → sirf **instance** (`obj.method()`) pe kaam karega.
  - Static method → sirf **class** (`ClassName.method()`) pe kaam karega.
  - **Cross-use** → **TypeError** deta hai (`method is not a function`).

**Example:**
```js
class Demo {
  regularMethod() { console.log("instance method"); }
  static staticMethod() { console.log("static method"); }
}
const d = new Demo();
d.regularMethod();       // ✅ Works
Demo.staticMethod();     // ✅ Works
Demo.regularMethod();    // ❌ TypeError
d.staticMethod();        // ❌ TypeError
```

---

## Step-by-Step Solution
1. **Option A: `john.greet();`**
   - `greet()` ek **instance method** hai, `john` ek **valid Person instance** hai.
   - **No Error** ✅ — Output: `"Hello, my name is John"`.
2. **Option B: `Person.greet();`**
   - `greet()` ek **instance method** hai — ye sirf **instances** pe available hota hai, **class pe directly nahi**.
   - `Person.greet` **exist hi nahi karta**.
   - **Error** ❌ — `TypeError: Person.greet is not a function`.
3. **Option C: `john.identify();`**
   - `identify()` ek **static method** hai — ye sirf **class pe directly** available hota hai, **instances pe nahi**.
   - `john.identify` **exist hi nahi karta**.
   - **Error** ❌ — `TypeError: john.identify is not a function`.
4. **Option D: `Person.identify();`**
   - `identify()` static method hai, `Person` khud class hai — valid.
   - **No Error** ✅ — Output: `"I am a Person class"`.
5. **Option E: `console.log(john.name);`**
   - `name` ek valid instance property hai.
   - **No Error** ✅ — Output: `"John"`.

**Shortcut:** Yaad rakho — "**Instance method = sirf object pe. Static method = sirf class pe. Cross karo to error!**"

---

## Final Answer
**Options B (`Person.greet()`) and C (`john.identify()`) will cause an error.**

---

## Why Other Options are Wrong?
### Option A
`greet()` instance method hai, `john` valid instance hai — ye **correctly kaam karega**, error nahi.

### Option D
`identify()` static method hai, `Person` class hai — ye **correctly kaam karega**, error nahi.

### Option E
`john.name` ek valid instance property hai — property access karna simple aur error-free hai.

---

## Important Exam Notes
- ✅ Instance methods → sirf `objectInstance.method()` se call ho sakte hain.
- ✅ Static methods → sirf `ClassName.method()` se call ho sakte hain.
- ✅ Cross-usage → **TypeError**.
- ⚠️ Common Mistake: Static aur instance methods ko interchangeably call karne ki koshish karna.
- 💡 Trick: "**Static = Class ka apna, Instance method = Object ka apna — kabhi mix mat karo!**"

---

## Similar Question Pattern
JavaScript classes ke static vs instance methods ke differences, aur inhe galat context me call karne se hone wale errors — ye common MSQ topic hai.

---

## Revision Box
`greet()` = instance method (sirf `john.greet()` valid hai, `Person.greet()` **error**). `identify()` = static method (sirf `Person.identify()` valid hai, `john.identify()` **error**). Cross-usage hamesha `TypeError` deta hai.

---
---

# 🎯 Overall Quick Revision Summary

| Q.No | Topic | Key Concept |
|---|---|---|
| 114 | Exam Instructions | Subject confirmation (0 marks) |
| 115 | v-if vs v-show | DOM removal vs CSS display toggle |
| 116 | TDZ (Temporal Dead Zone) | `let` shadowing outer var causes ReferenceError |
| 117 | Hoisting | `var` (undefined) vs function declaration (full hoist) |
| 118 | let/const/var Rules | const mandatory initialization |
| 119 | Closures + Pre-increment | `++count` vs `count++`, independent closures |
| 120 | Error-Prone Code Snippets | const init, destructure undefined, const reassignment |
| 121 | Vue Methods | Multiplicative state growth (`*=`) |
| 122 | `this` + Array `.length` | Method-call `this`, array element count trap |
| 123 | `.bind()` Edge Cases | Empty bind() vs bind(obj), extra call arguments |
| 124 | `.call()` + Naming Trap | Parameter vs `this.property` same-name confusion |
| 125 | Vue `:class` Object Syntax | Conditional class binding |
| 126 | Vue Computed + Arrow Function | Arrow function breaks `this` binding in Vue |
| 127 | Vue Named + Default Slots | Fallback content vs parent-provided content |
| 128 | Multiple Vue Instances | Independent instances, post-creation reactivity |
| 129 | Vue Separate Watchers | Sequential watcher execution order matters |
| 130 | Static vs Instance Methods | Cross-usage causes TypeError |

---
**📌 Note:** Ye notes exam revision ke liye complete hain — har question ek mini-chapter ki tarah explain kiya gaya hai taaki original book/solution dekhne ki zaroorat na pade. 🎓
