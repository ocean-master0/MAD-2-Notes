# Week 1 (MAD 2) Exam Notes

## Topic: `this`, Arrow Functions, Objects, `const`, Event Loop, `setTimeout`, `setInterval`, DOM Manipulation

> **Exam Target:** IIT Madras BS - Modern Application Development 2 (MAD 2)
>
> **Prepared from Previous Exam Questions**

---

# Table of Contents

1. JavaScript Objects
2. `this` Keyword
3. Normal Function vs Arrow Function
4. `const` with Objects
5. Event Loop
6. `setTimeout()`
7. `setInterval()`
8. DOM Manipulation
9. Common Interview/Exam Questions
10. Frequently Asked MCQs
11. Exam Tricks (Revision Sheet)

---

# 1. JavaScript Objects

## Object

An object stores data in **key-value pairs**.

```javascript
const obj = {
    name: "Rohit",
    age: 20
}
```

Memory

```
obj
│
├── name = "Rohit"
└── age = 20
```

Access Property

```javascript
console.log(obj.name)
```

Output

```
Rohit
```

Update Property

```javascript
obj.name = "Mohit"
```

Now

```
obj
│
└── name = "Mohit"
```

---

# 2. `this` Keyword

## Definition

`this` refers to the object that is calling the function.

### Example

```javascript
const obj = {
    name: "Rohit",

    changeName: function(name){
        this.name = name;
    }
}

obj.changeName("Mohit")

console.log(obj.name)
```

Output

```
Mohit
```

Reason

```
this = obj
```

So

```
this.name = name

↓

obj.name = name
```

---

## Memory Diagram

Before

```
obj

name = Rohit
```

↓

```
obj.changeName("Mohit")
```

↓

```
obj

name = Mohit
```

---

# Exam Rule

```
obj.method()

↓

this = obj
```

---

# 3. Arrow Function

Arrow Functions **do not create their own `this`**.

They use **Lexical this**.

---

## Wrong

```javascript
const obj = {

    name: "Rohit",

    changeName: (name)=>{

        this.name = name

    }

}

obj.changeName("Mohit")

console.log(obj.name)
```

Output

```
Rohit
```

Reason

```
Arrow Function

↓

No own this

↓

Uses outer this

↓

Object not updated
```

---

## Correct

```javascript
const obj = {

    name: "Rohit",

    changeName: function(name){

        this.name = name

    }

}
```

Output

```
Mohit
```

---

# Arrow Function Inside Normal Function

```javascript
const obj = {

    name:"Rohit",

    arrowFunction:null,

    normalFunction:function(){

        this.arrowFunction=()=>{

            console.log(this.name)

        }

    }

}

obj.normalFunction()

obj.arrowFunction()
```

Output

```
Rohit
```

Reason

```
Normal Function

↓

this = obj

↓

Arrow Function remembers this

↓

this = obj forever
```

---

# Normal Function vs Arrow Function

| Normal Function         | Arrow Function       |
| ----------------------- | -------------------- |
| Own `this`              | No own `this`        |
| `this` = Calling Object | `this` = Outer Scope |
| Used for Object Methods | Used for Callbacks   |

---

# 4. `const` with Objects

Many students think `const` objects cannot change.

Wrong.

## Allowed

```javascript
const obj={

    name:"Rohit"

}

obj.name="Mohit"

console.log(obj.name)
```

Output

```
Mohit
```

Reason

```
Reference same

Property changed
```

---

## Not Allowed

```javascript
const obj={

name:"Rohit"

}

obj={

name:"Mohit"

}
```

Output

```
TypeError
```

Reason

```
Reference changed
```

---

# Remember

```
const

↓

Reference Fixed

NOT Object Fixed
```

---

# 5. Synchronous vs Asynchronous

## Synchronous

Runs line by line.

Examples

```javascript
console.log()

let

const

if

for

while

function()

Math.sqrt()

Array Methods
```

Example

```javascript
console.log("A")

console.log("B")

console.log("C")
```

Output

```
A

B

C
```

---

## Asynchronous

Runs later.

Examples

```
setTimeout()

setInterval()

fetch()

Promise

async/await

Event Listeners
```

---

# 6. Event Loop

JavaScript is Single Threaded.

There are three important things.

```
Call Stack

↓

Web API

↓

Callback Queue

↓

Event Loop
```

---

## Flow

```
Synchronous Code

↓

Call Stack

↓

Web API

↓

Callback Queue

↓

Event Loop

↓

Call Stack
```

---

# setTimeout()

```javascript
setTimeout(()=>{

console.log("Hello")

},1000)
```

Runs only once.

---

## Example

```javascript
setTimeout(()=>console.log("A"),0)

console.log("B")

setTimeout(()=>console.log("C"),0)

console.log("D")
```

Output

```
B

D

A

C
```

Reason

```
Main Thread

↓

Console Logs

↓

Timeouts
```

---

# Important

```
setTimeout(fn,0)

≠

Immediate Execution
```

It waits until

```
Call Stack Empty
```

---

# Event Loop Diagram

```
console.log()

↓

Immediate

-----------------

setTimeout()

↓

Web API

↓

Callback Queue

↓

Event Loop

↓

Execute
```

---

# 7. setInterval()

Runs repeatedly after fixed interval.

Example

```javascript
let handler=setInterval(()=>{

console.log("Hello")

},1000)
```

---

Stop

```javascript
clearInterval(handler)
```

---

# Example

```javascript
let x="orange".split("").reverse()

console.log(x)
```

Output

```
["e","g","n","a","r","o"]
```

Now

```javascript
x.pop()
```

returns

```
o
```

Then

```
r

↓

a

↓

n

↓

g

↓

e
```

---

Timeline

```
1 sec

o

2 sec

r

3 sec

a

4 sec

n

5 sec

g

6 sec

e
```

---

# split()

Converts string into array.

```javascript
"hello".split("")
```

Output

```javascript
["h","e","l","l","o"]
```

---

# reverse()

```javascript
["a","b","c"].reverse()
```

Output

```javascript
["c","b","a"]
```

---

# pop()

Removes last element.

```javascript
let arr=[1,2,3]

arr.pop()
```

Output

```
3
```

Remaining

```
[1,2]
```

---

# 8. DOM Manipulation

Find Element

```javascript
document.getElementById("div1")
```

Change Style

```javascript
document.getElementById("div1").style.backgroundColor="yellow"
```

---

Example

```html
<div id="div1"
style="background:red">
</div>
```

JavaScript

```javascript
setTimeout(()=>{

document.getElementById("div1").style.backgroundColor="yellow"

},0)
```

Final Color

```
Yellow
```

Reason

```
HTML

↓

DOM Created

↓

setTimeout Executes

↓

Color Changed
```

---

# DOM Diagram

```
HTML

↓

DOM

↓

JavaScript

↓

Style Updated
```

---

# 9. Frequently Asked MCQs

---

### Q1

```javascript
const obj={

name:"Rohit"

}

obj.name="Mohit"

console.log(obj.name)
```

Answer

```
Mohit
```

---

### Q2

```javascript
const obj={

name:"Rohit",

change:function(){

this.name="Mohit"

}

}
```

Output

```
Mohit
```

---

### Q3

```javascript
const obj={

name:"Rohit",

change:()=>{

this.name="Mohit"

}

}
```

Output

```
Rohit
```

---

### Q4

```javascript
console.log("A")

setTimeout(()=>console.log("B"),0)

console.log("C")
```

Output

```
A

C

B
```

---

### Q5

```javascript
let arr="abc".split("").reverse()

console.log(arr.pop())
```

Output

```
a
```

---

### Q6

```javascript
setInterval(()=>{

console.log("Hi")

},1000)
```

Runs

```
Forever
```

Until

```javascript
clearInterval()
```

---

# 10. Most Important Exam Tricks

---

## Trick 1

```
obj.method()

↓

this = obj
```

---

## Trick 2

```
Arrow Function

↓

No own this

↓

Lexical this
```

---

## Trick 3

```
const

↓

Reference Fixed

Property Change Allowed
```

---

## Trick 4

```
console.log()

↓

Immediate
```

---

## Trick 5

```
setTimeout()

↓

Web API

↓

Callback Queue

↓

Event Loop
```

---

## Trick 6

```
setInterval()

↓

Runs Again & Again
```

---

## Trick 7

```
clearInterval()

↓

Stops setInterval
```

---

## Trick 8

```
split()

↓

String → Array
```

---

## Trick 9

```
reverse()

↓

Reverse Array
```

---

## Trick 10

```
pop()

↓

Removes Last Element
```

---

## Trick 11

```
document.getElementById()

↓

Find DOM Element
```

---

## Trick 12

```
style.backgroundColor

↓

Changes Background Color
```

---

# 🚀 One-Page Ultra Fast Revision

```
Object Method
↓

this = obj

--------------------

Arrow Function
↓

No own this

↓

Outer Scope

--------------------

const Object

↓

Properties Change Allowed

↓

Reference Cannot Change

--------------------

console.log()

↓

Synchronous

--------------------

setTimeout()

↓

Web API

↓

Callback Queue

↓

Event Loop

--------------------

setInterval()

↓

Runs Repeatedly

↓

clearInterval()

Stops It

--------------------

split()

↓

String → Array

--------------------

reverse()

↓

Reverse Array

--------------------

pop()

↓

Removes Last Element

--------------------

DOM

↓

getElementById()

↓

style.backgroundColor

↓

Updates HTML
```

# ⭐ Final Exam Tips

1. **`this` in Normal Function = Calling Object.**
2. **Arrow Function ka apna `this` nahi hota; ye lexical `this` use karti hai.**
3. **`const` object ki properties change ho sakti hain, reference nahi.**
4. **`setTimeout(..., 0)` bhi synchronous code ke baad hi execute hota hai.**
5. **`setInterval()` ko hamesha `clearInterval()` se stop kiya ja sakta hai.**
6. **DOM ko modify karne se pehle ensure karo ki element available ho (DOM load ho chuka ho).**


