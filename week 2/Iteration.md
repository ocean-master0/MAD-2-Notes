### 1. Introduction to Iteration in JavaScript

**Explanation:**  
Iteration ka matlab hai **ek collection (array, object, string, Map, Set etc.) ke elements par ek-ek karke kaam karna**. JS mein iteration bahut powerful hai kyunki yeh **synchronous** aur **asynchronous** dono tarah se ho sakta hai. Yeh control flow ka important part hai — bina iteration ke hum arrays, DOM elements, API data handle nahi kar sakte.

**Kyun important hai?**  
- Data processing (filter, transform, aggregate)  
- DOM manipulation (sabhi list items pe action)  
- Performance (modern methods optimized hote hain)  
- Modern JS (ES6+) mein **iterables + iterators** ka concept aaya jo bahut clean aur powerful hai.

**Key Terms:**
- **Iterable**: Wo object jisme `Symbol.iterator` method ho (arrays, strings, Maps, Sets, generators).
- **Iterator**: Object jo `next()` method deta hai → `{value, done}` return karta hai.
- **Loop**: Control structure jo repeatedly execute hota hai jab tak condition true ho.

**Example:**
```javascript
const fruits = ["Apple", "Mango", "Banana"];

// Traditional way
for(let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}
```

**Quick Recap:**
- Iteration = systematically visiting each item.
- JS mein multiple ways: loops + higher-order methods + iterators.
- ES6 ne iteration ko bahut modern bana diya.

---

### 2. Traditional Looping Constructs

#### 2.1 for Loop
**Explanation:**  
Sabse classic aur **most flexible** loop. Initialization, condition aur increment/decrement ek hi line mein. Yeh **index-based** iteration karta hai. Bahut control deta hai (break, continue, manual index change).

**Exam Important ⭐:** Yeh sabse zyada poochha jaata hai — nested loops, pattern printing, performance comparison.

**Example:**
```javascript
// Multiplication table
for(let i = 1; i <= 5; i++) {
    let row = '';
    for(let j = 1; j <= 5; j++) {
        row += (i*j) + "\t";
    }
    console.log(row);
}
```

#### 2.2 while Loop
**Explanation:**  
**Condition-first** loop. Jab tak condition true hai, execute hota rahega. Tab use karo jab **exact number of iterations** pehle se pata na ho (jaise user input ya API polling).

**Example:**
```javascript
let count = 0;
while(count < 5) {
    console.log("Count:", count);
    count++;
}
```

#### 2.3 do-while Loop
**Explanation:**  
**Condition-last** loop. **Kam se kam ek baar** zaroor execute hota hai. Forms mein validation, menu-driven programs ke liye best.

**Example:**
```javascript
let input;
do {
    input = prompt("Enter number > 10");
} while(input <= 10);
```

**Comparison Table: for vs while vs do-while**

| Feature              | for Loop              | while Loop            | do-while Loop         |
|----------------------|-----------------------|-----------------------|-----------------------|
| Initialization       | In loop header        | Outside              | Outside              |
| Condition Check      | Before each iteration | Before               | After                |
| Guaranteed Execution | No                    | No                   | **Yes (at least once)** |
| Use Case             | Known iterations      | Unknown count        | Menu/Validation      |
| Exam Frequency       | Very High             | High                 | Medium               |

**Quick Recap (Traditional Loops):**
- `for` → index control ke liye best.
- `while` → condition pe depend.
- `do-while` → minimum one execution.
- `break` aur `continue` sabme kaam karte hain.

---

### 3. for...in and for...of (ES6)

#### 3.1 for...in Loop
**Explanation:**  
**Enumerable properties** (keys) par iterate karta hai. Objects ke liye bana hai. **Prototype chain** ke inherited properties bhi aa sakti hain (isliye careful use karo).

**Important Note:** Arrays pe mat use karo — order guarantee nahi + extra properties aa sakti hain.

**Example:**
```javascript
const person = { name: "Rahul", age: 25, city: "Delhi" };

for(let key in person) {
    console.log(key + ": " + person[key]);
}
```

#### 3.2 for...of Loop
**Explanation:**  
**Values** par iterate karta hai. **Iterable protocol** follow karta hai. Arrays, strings, Maps, Sets, generators — sab pe kaam karta hai. **Cleanest** way for values.

**Key Difference:**
- `for...in` → **Keys/Indices**
- `for...of` → **Values**

**Example:**
```javascript
const arr = ["a", "b", "c"];

for(let value of arr) {
    console.log(value);     // a, b, c
}

for(let char of "Hello") {
    console.log(char);      // H e l l o
}
```

**Comparison Table: for...in vs for...of**

| Aspect             | for...in               | for...of                     |
|--------------------|------------------------|------------------------------|
| Iterates           | Keys/Properties        | Values                       |
| Works on           | Objects mainly         | Any Iterable                 |
| Array Order        | Not guaranteed         | Guaranteed                   |
| Prototype props    | Yes                    | No                           |
| Recommended for Arrays | **Never**           | **Yes**                      |

**Quick Recap:**
- `for...in` = object keys.
- `for...of` = modern, clean value iteration.
- ⭐ Exam: Difference likhna compulsory.

---

### 4. Array Higher-Order Iteration Methods (Most Important 🔥)

Ye sab **callback functions** lete hain aur **functional programming** style follow karte hain.

#### 4.1 forEach()
**Explanation:**  
Har element pe callback execute karta hai. **Return value nahi deta**. Side effects (console, DOM change) ke liye best. Break nahi kar sakte.

**Example:**
```javascript
const numbers = [1, 2, 3, 4];
numbers.forEach((num, index) => {
    console.log(`Index ${index}: ${num}`);
});
```

#### 4.2 map()
**Explanation:**  
**New array** return karta hai jisme har element ko transform kiya gaya ho. Original array immutable rehta hai. **Transformation** ke liye king.

**Example:**
```javascript
const nums = [1, 2, 3, 4];
const squares = nums.map(num => num * num); // [1, 4, 9, 16]
```

#### 4.3 filter()
**Explanation:**  
**Condition** satisfy karne wale elements ka **new array** return karta hai.

**Example:**
```javascript
const ages = [12, 18, 25, 16, 30];
const adults = ages.filter(age => age >= 18);
```

#### 4.4 reduce()
**Explanation:**  
**Single value** mein reduce karta hai (sum, product, max, object building etc.). Accumulator concept samajhna bahut zaroori.

**Example:**
```javascript
const nums = [1, 2, 3, 4, 5];
const sum = nums.reduce((acc, curr) => acc + curr, 0); // 15
```

**Other Important Methods:**
- `some()` → koi ek bhi true?
- `every()` → sab true?
- `find()` → first matching element
- `findIndex()`
- `flatMap()`

**Quick Recap (Array Methods):**
- `forEach` → side effects, no return.
- `map` → transform → new array.
- `filter` → condition based selection.
- `reduce` → accumulator based single value.
- ⭐ Sab immutable (original array safe).

---

### 5. Advanced Iteration: Iterators & Generators

#### 5.1 Iterable Protocol & Iterators
**Explanation:**  
Koi bhi object **iterable** ban sakta hai agar usme `[Symbol.iterator]()` method ho jo iterator return kare. Iterator ka `next()` method `{value, done}` return karta hai.

**Text Diagram:**
```
Iterable → [Symbol.iterator]() → Iterator
Iterator → .next() → {value: any, done: boolean}
```

**Example:**
```javascript
const myIterable = {
    [Symbol.iterator]: function* () {
        yield 1; yield 2; yield 3;
    }
};

for(let num of myIterable) {
    console.log(num);
}
```

#### 5.2 Generators (`function*`)
**Explanation:**  
Special functions jo `yield` keyword se pause ho sakte hain aur baad mein resume. **Lazy evaluation** — values tabhi generate hote hain jab chahiye. Memory efficient for large/infinite sequences.

**Example:**
```javascript
function* fibonacci() {
    let a = 0, b = 1;
    while(true) {
        yield a;
        [a, b] = [b, a+b];
    }
}

const fib = fibonacci();
console.log(fib.next().value); // 0
console.log(fib.next().value); // 1
```

**Exam Angle ⭐:** Generators + async generators (for await...of) advanced questions mein aate hain.

---

### 6. Async Iteration (`for await...of`)

**Explanation:**  
Async iterables (jaise streams, async generators) ke liye. Promises ko handle karta hai cleanly.

**Example:**
```javascript
async function* asyncGenerator() {
    yield Promise.resolve(1);
    yield Promise.resolve(2);
}

(async () => {
    for await (let num of asyncGenerator()) {
        console.log(num);
    }
})();
```

---

