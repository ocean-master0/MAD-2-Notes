### 1. Introduction to Higher-Order Array Methods

**Kya hai?**  
JavaScript mein arrays ke saath kaam karte waqt **Higher-Order Functions** bahut powerful hote hain. Ye functions dusre functions ko as argument accept karte hain (callback function). Inme se sabse commonly used `map()`, `forEach()`, `filter()`, `find()`, `reduce()` etc. hain.

**Kyun important hai?**  
- Code ko declarative banate hain (kya karna hai, kaise nahi).
- Traditional `for` loop se kam lines mein aur readable code.
- Functional Programming style support karte hain.
- React, Node, modern frameworks mein heavily use hote hain.

**⭐ Exam Important**: Interview aur university exams mein aksar poocha jaata hai – “Difference between `forEach`, `map`, `filter` aur `find`” ya “Kab `filter` use karoge aur kab `find`?”

---

### 2. The `filter()` Method

#### 2.1 Deep Explanation
`filter()` ek **higher-order array method** hai jo ek **new array** return karta hai jisme sirf woh elements hote hain jo **callback function** mein `true` return karte hain.

- **Original array ko modify nahi karta** (immutable operation).
- Callback function har element ke liye call hota hai.
- Agar callback `true` → element new array mein include.
- Agar `false` / `undefined` / `null` → exclude.
- Agar koi element match na kare to **empty array** return hota hai.

**Syntax**:
```javascript
const newArray = array.filter(callback(currentValue, index, arr), thisArg);
```

**Parameters**:
- **callback**: Mandatory. 3 arguments le sakta hai – `element`, `index`, `array`.
- **thisArg** (optional): Callback mein `this` ki value set karne ke liye.

**Kaise kaam karta hai internally?**  
JS engine har element pe callback run karta hai → truthy value check → matching elements ko collect karke naya array banata hai.

**Key Terms & Definitions**:
- **Predicate Function**: Woh callback jo `true`/`false` return kare.
- **Immutable**: Original array change nahi hota.

#### 2.2 Practical Example
```javascript
const students = [
  { name: "Amit", marks: 85, active: true },
  { name: "Rohit", marks: 45, active: false },
  { name: "Priya", marks: 92, active: true },
  { name: "Sumit", marks: 38, active: true }
];

// Pass students with marks > 60 aur active
const passedStudents = students.filter(student => student.marks > 60 && student.active);

console.log(passedStudents);
// Output: Amit aur Priya ke objects
```

**Real-world use**: E-commerce mein price filter, user search results filter, active users list, etc.

#### 2.3 Text-based Flow Diagram
```
Original Array
       ↓ (for each element)
Callback Function (predicate)
       ↓
   True?  ──Yes──► Include in New Array
       ↓
      No
       ↓
    Skip
       ↓
   Return New Filtered Array
```

**Quick Recap – filter()**:
- New array return karta hai.
- Original array safe rahta hai.
- Multiple elements return kar sakta hai (ya zero).
- Predicate function `true` return kare tabhi include.
- Chaining possible (`arr.filter().map()`).

---

### 3. The `find()` Method

#### 3.1 Deep Explanation
`find()` method **pehle element** ko return karta hai jo callback condition satisfy kare. Jaise hi pehla match mil jaaye, wo ruk jaata hai (short-circuiting).

- Agar koi match na mile to **`undefined`** return karta hai.
- Sirf **ek value** return karta hai (object, primitive, ya undefined).
- Performance better hota hai jab sirf ek cheez chahiye kyunki baaki elements check nahi karta.

**Syntax**:
```javascript
const result = array.find(callback(currentValue, index, arr), thisArg);
```

**Important Difference from filter**:  
`filter` saare matches deta hai → new array.  
`find` sirf first match deta hai → single value.

**⭐ Exam Important**: Yeh difference sabse zyada poocha jaata hai. `find()` ko `findIndex()` ke saath bhi compare karte hain.

#### 3.2 Practical Example
```javascript
const users = [
  { id: 1, name: "Rahul", role: "admin" },
  { id: 2, name: "Sneha", role: "user" },
  { id: 3, name: "Aman", role: "admin" }
];

// Pehla admin user dhundo
const firstAdmin = users.find(user => user.role === "admin");

console.log(firstAdmin); // { id: 1, name: "Rahul", role: "admin" }
```

**Real-world use**: Database se unique record nikaalna (jaise product by ID, user by email), routing mein slug se page dhundna, etc.

#### 3.3 Text-based Flow
```
Start from index 0
       ↓
Callback on element
       ↓
   True? ──Yes──► Return that element immediately
       ↓
      No
       ↓
   Next element...
       ↓
No match till end? → return undefined
```

**Quick Recap – find()**:
- Pehla matching element return karta hai.
- Sirf ek value (ya `undefined`).
- Short-circuits (performance friendly for single search).
- Original array modify nahi karta.

---

### 4. Comparison: `filter()` vs `find()`

| Feature                  | `filter()`                          | `find()`                              |
|-------------------------|-------------------------------------|---------------------------------------|
| Return Type             | New Array                           | Single value or `undefined`           |
| No. of elements returned| 0 to all elements                   | 0 or 1                                |
| Stops early?            | No (saare check karta hai)          | Yes (first match pe ruk jaata hai)   |
| Use Case                | Multiple results chahiye            | Ek specific item chahiye              |
| When nothing matches    | Empty array `[]`                    | `undefined`                           |
| Chaining                | Bahut easy                          | Value pe further methods laga sakte ho |
| Performance             | Thoda slow jab array bada ho        | Faster for lookup                     |

**⭐ Exam Important**: Table yaad kar lo – 5-8 marks ka question aa sakta hai ispe.

---

### 5. Advanced Concepts & Edge Cases

#### 5.1 `thisArg` Parameter
```javascript
const obj = { threshold: 50 };
const numbers = [30, 60, 45, 80];

const above = numbers.filter(function(num) {
  return num > this.threshold;
}, obj);   // thisArg use kiya
```

#### 5.2 Common Mistakes
- `filter` ko value assign karna bhool jaana.
- `find` ke baad `.name` karna jab undefined aaye (TypeError).
- Arrow function mein `{}` use karne pe `return` bhoolna.
- Sparse arrays (empty slots) mein behavior alag hota hai.

#### 5.3 Polyfill (Exam ke liye important)
```javascript
// Simple filter polyfill
Array.prototype.myFilter = function(callback) {
  let result = [];
  for(let i = 0; i < this.length; i++) {
    if(callback(this[i], i, this)) {
      result.push(this[i]);
    }
  }
  return result;
};
```

Similar tarike se `find` ka polyfill bhi likh sakte ho.

---

### 6. Related Methods (Quick Comparison)

- **`map()`**: Har element ko transform karta hai → new array same length.
- **`forEach()`**: Side effects ke liye, kuch return nahi karta.
- **`findIndex()`**: Index return karta hai instead of value.
- **`some()`**: Kya koi element condition satisfy karta hai? (`true`/`false`)
- **`every()`**: Kya sab elements condition satisfy karte hain?

---
