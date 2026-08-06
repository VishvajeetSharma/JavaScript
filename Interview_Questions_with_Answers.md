# 🚀 Core JavaScript – Professional Interview Guide

> **Level:** Intermediate → Advanced  
> **Purpose:** Interview Preparation, Concept Clarity, Real‑World Understanding  
> **Use Case:** Frontend / Backend / Full‑Stack JavaScript Roles

## 1️⃣ Difference between `var`, `let`, and `const`

### `var`
* Function‑scoped
* Hoisted and initialized with `undefined`
* Can be redeclared and reassigned
  
### Syntax
* var a;          // declaration
* a = 10;         // assignment

Redeclaration allowed:
* var x = 5;
* var x = 10;     //  allowed

### `let`(Modern JavaScript – ES6)
* Block‑scoped
* Hoisted but exists in **Temporal Dead Zone (TDZ)**
* Can be reassigned but **not redeclared**
  
### Syntax
* let a;          // declaration
* a = 10;

Redeclaration : //Not allowed
* let x = 5;
*  let x = 10;  //  Error

### `const`

* Block‑scoped
* Must be initialized at declaration
* Cannot be reassigned (mutation allowed for objects)

 ### Syntax
* const a = 10;   //  must assign immediately

 Not allowed:
* const b;        //  Error

**Best Practice:**

* Use `const` by default
* Use `let` when reassignment is required
* Avoid `var`

 ## 2️⃣ How JavaScript Handles Asynchronous Programming
  JavaScript is single-threaded, meaning it can do only one task at a time.
  If a long task (API call, file read, timer) blocks the main thread, the UI freezes 
  To avoid this, JavaScript uses asynchronous programming.

### Evolution
1. Callbacks → callback hell
2. Promises (ES6) → chaining & error handling
3. Async/Await (ES2017) → synchronous‑like syntax

### Core Components
* Call Stack
* Web APIs
* Microtask Queue (Promises)
* Macrotask Queue (setTimeout, setInterval)
* Event Loop

### 1️⃣ Call Stack
* Executes synchronous code
* Works in LIFO order (Last In, First Out)

```
function first() {
  second();
}
function second() {
  console.log("Hello");
}
first();

```

* All functions run inside the call stack.

 ### 2️⃣ Web APIs
* Provided by the browser (not JS itself):
* setTimeout
* fetch
* DOM events

setTimeout(() => {
  console.log("Async Task");
}, 2000);

* The timer is handled by Web API, not the call stack.


### 3️⃣ Macrotask Queue
* Macrotasks are normal async tasks that execute after microtasks.
 Examples of Macrotasks
* setTimeout
* setInterval
* setImmediate (Node.js)

### 4️⃣ Microtask Queue
* Microtasks are high-priority async tasks that execute immediately after the current synchronous code.
Examples of Microtasks
* Promise.then()
* Promise.catch()
* Promise.finally()
  
Promise.resolve().then(() => console.log("Microtask"));


### 5️⃣ Event Loop 
* The Event Loop constantly checks:
* Is call stack empty?
* If yes → execute microtasks first
* Then execute callback (macrotask)
* Microtasks always execute before macrotasks


## 3️⃣ Closures and How They Work
* A closure is a function that **remembers its lexical scope** even after execution

### How Closure Works Internally
1. JS creates Lexical Environment
2. Inner function gets reference to outer variables
3. Garbage Collector does NOT remove those variables
4. Variables live as long as closure exists

### Benefits
* Data encapsulation
* Private variables
* Stateful functions


### Use Cases
* Module pattern
* Currying
* Event handlers
* Memoization


  # Closures in JavaScript

## Example Code

```js
function outer() {
  let count = 0;

  function inner() {
    count++;
    console.log(count);
  }

  return inner;
}

const counter = outer();
counter(); // 1
counter(); // 2
counter(); // 3
```
 **Note:** Improper usage may cause memory leaks

## 4️⃣ Event Loop and Its Role
JavaScript is single-threaded, but still manages multiple tasks efficiently because of the Event Loop.
### Why Event Loop is Needed?
JavaScript can execute only one task at a time (single thread).
But in real applications we use:
* setTimeout
* fetch / API calls
* Promises
   Without the Event Loop, JavaScript would freeze while waiting for these tasks.

### Execution Flow

1. Call Stack executes synchronous code
2. Web APIs handle async operations
3. Microtasks executed first
4. Macrotasks executed next

### Execution Priority

```
Call Stack
⬇
Microtask Queue (Promises)
⬇
Callback Queue (setTimeout, events)

```

## 5️⃣ Prototypal Inheritance

Prototypal inheritance is the core inheritance model in JavaScript.
Instead of copying properties, objects inherit directly from other objects through a prototype chain.

###  What It Means
* Objects can reuse properties and methods of other objects
* JavaScript looks up properties through the prototype chain
* Inheritance is dynamic and memory-efficient


### Key Points Explained
✔ Objects inherit from other objects

An object can use properties and methods defined on its prototype.
```
const parent = {
  sayHello() {
    console.log("Hello");
  }
};

const child = Object.create(parent);
child.sayHello(); // Hello

```

✔ Prototype chain enables property lookup

If a property is not found on the object, JavaScript looks up the chain.

child → parent → Object.prototype → null

-----

✔ Every object has __proto__

__proto__ points to the object’s prototype.

child.__proto__ === parent; // true

-----

✔ Object.create() creates custom prototype chains

The recommended way to set prototypes.

const admin = Object.create(User.prototype);

----
✔ Constructor functions use .prototype

Functions used as constructors define shared properties on .prototype.
```
function User(name) {
  this.name = name;
}

User.prototype.greet = function () {
  console.log("Hi " + this.name);
};

const u1 = new User("Amit");
u1.greet(); // Hi Amit
```
-----
✔ ES6 classes are syntactic sugar

Classes do not replace prototypes; they wrap them.
```
class Person {
  talk() {
    console.log("Talking");
  }
}
```

## 6️⃣ Difference between `==` and `===`

### Equality (==)
The equality (==) operator checks whether its two operands are equal, returning a Boolean result.
### Syntax
x == y
```
console.log(1 == 1);
// Expected output: true

console.log("hello" == "hello");
// Expected output: true

console.log("1" == 1);
// Expected output: true

```
### Strict equality (===)
The strict equality (===) operator checks whether its two operands are equal, returning a Boolean result. Unlike the equality operator, the strict equality operator always considers operands of different types to be different.

### Syntax
x === y

### Example

```
console.log(1 === 1);
// Expected output: true

console.log("1" === 1);
// Expected output: false

```

**Best Practice:** Always use `===`


## 7️⃣ Scope and Hoisting
Scope and Hoisting are fundamental JavaScript mechanisms that determine how variables and functions are accessed and initialized during execution. 

### 1. Scope (Accessibility)
Scope defines the area of the code where a variable or function is accessible. 
* Global Scope: Variables defined outside any function or block are accessible anywhere in the application.
* Function (Local) Scope: Variables defined inside a function are only accessible within that function.
* Block Scope (ES6): Variables declared with let or const inside {} blocks (e.g., if, for, while) are only accessible within that block. 

### 2. Hoisting (Declaration Movement)
Hoisting is a JavaScript mechanism where variable and function declarations are moved to the top of their containing scope during the compilation phase, before code execution. This allows them to be used before they are formally declared. 

* Function Declarations: The entire function is hoisted, meaning it can be called before it is defined.
* var Variables: Declarations are hoisted and initialized with undefined. Using them before assignment results in undefined, not a ReferenceError
* let and const Variables: These are hoisted but not initialized. They exist in a "Temporal Dead Zone" (TDZ) from the start of the block until the declaration is processed, resulting in a ReferenceError if accessed early.
* Function Expressions: Variables holding functions (e.g., var sum = function() {}) are hoisted like var, so they are undefined before the assignment, not usable as functions.

### Best Practices
* Use let and const: Avoid var to prevent unexpected issues with hoisting and scoping.
* Declare at the top: Always declare variables at the beginning of their respective scope.
* Functions before calls: Although functions are hoisted, placing them before calls improves code readability.


## 8️⃣ IIFE (Immediately Invoked Function Expressions)
Immediately Invoked Function Expressions (IIFE) are JavaScript functions that are executed immediately after they are defined. They are typically used to create a local scope for variables to prevent them from polluting the global scope.

### Syntax

```
(function (){ 
// Function Logic Here. 
})();
```

### Example
```
(function() {
    // IIFE code block
    var localVar = 'This is a local variable';
    console.log(localVar); // Output: This is a local variable
})();

```

### Use Cases Of IIFE

* Avoid global scope pollution
* Create private scope
* Used before ES6 modules


## 9️⃣ Higher‑Order Functions
A higher-order function is a function that does one of the following:
* Takes another function as an argument.
* Returns another function as its result.
Higher-order functions help make your code more reusable and modular by allowing you to work with functions like any other value.

### Example 
```
function fun() {
    console.log("Hello, World!");
}
function fun2(action) {
    action();
    action();
}

fun2(fun);
```
* fun2 is a higher-order function because it takes another function (action) as an argument.
* It calls the action function twice.

### Popular Higher Order Functions in JavaScript
* map
* filter
* filter
* forEach



### Benefits

* Increased Code Reusability
* Enhanced Readability
* Improved Code Maintainability
* Abstraction of Operations
* Flexibility and Customization

## 🔟 Promises

JavaScript Promises make handling asynchronous operations like API calls, file loading, or time delays easier.A Promise is an object that represents the eventual completion or failure of an asynchronous operation and its resulting value. 
It can be in one of three states

1.  Pending: The task is in the initial state.
2. Fulfilled: The task was completed successfully, and the result is available.
3. Rejected: The task failed, and an error is provided.


### Methods

* `.then()`
* `.catch()`
* `.finally()`

### Static Methods

* `Promise.all()`
* `Promise.race()`
* `Promise.allSettled()`

## 1️⃣1️⃣ `null` vs `undefined`

### undefined
undefined means a variable is declared but not assigned a value.

```
let a;
console.log(a); // undefined
```
* Variable is declared but no value assigned, so JavaScript gives undefined.


### null
null means the developer has intentionally set the variable to no value.
```
let b = null;
console.log(b); // null
```
* Value is explicitly set by the developer to represent “no value”.


### Extra Points 

```
typeof undefined; // "undefined"
typeof null;      // "object" (JavaScript bug)
```
```
null == undefined   // true
null === undefined  // false
```

## 1️⃣2️⃣ Temporal Dead Zone (TDZ)
Temporal Dead Zone (TDZ) is the time between variable hoisting and its initialization where let and const cannot be accessed.
* Applies only to let and const
* Variable is hoisted but not initialized
* Accessing it before initialization → ReferenceError

**Example:**
```
console.log(a); //  ReferenceError (TDZ)
let a = 10;
a exists in memory but is in TDZ until the line let a = 10 executes.
```

```
console.log(b); //  ReferenceError
const b = 20;
```

### Why var has no TDZ
```
console.log(y); // undefined
var y = 10;
 var is initialized with undefined, so no TDZ
```



## 1️⃣3️⃣ `this` Keyword
this refers to the object that is currently calling the function, and its value depends on how the function is invoked.
* this is a runtime binding
* Its value is decided when the function is called, not where it is written
* Behavior changes based on execution context

### Context‑Based Behavior
1. Global context → this refers to window (browser) or global (Node.js)
    ```
     console.log(this);
    ```
2. Function context → this depends on how the function is called
```
function show() {
  console.log(this);
}
show();
```
3. Method context → this refers to the object before the dot
```
const user = {
  name: "Amit",
  greet() {
    console.log(this.name);
  }
};

user.greet(); // Amit
```
4. Constructor context → this refers to the newly created instance
```
function Person(name) {
  this.name = name;
}

const p1 = new Person("Neha");
console.log(p1.name); // Neha
```
5. Arrow function → this is lexically inherited from its surrounding scope
```
const obj = {
  name: "Rahul",
  greet: () => {
    console.log(this.name);
  }
};
obj.greet(); // undefined
```

## 1️⃣4️⃣ `call`, `apply`, and `bind`
These methods are used to control the value of this explicitly for a function.

### Why do we need them?
In JavaScript, the value of this depends on how a function is called, not where it is written.
call, apply, and bind allow us to manually set this.

### Comparison Table
```
Method  	Behavior                        ExecutesImmediately
call()	  Arguments passed individually 	 Yes
apply()  	Arguments passed as array     	 Yes
bind()	  Returns new bound function     	  No
```
### call() Method
Calls a function immediately with a specified this value and individual arguments.
### Syntax
```
functionName.call(thisArg, arg1, arg2, ...);
```
**Example:**
```
const user = {
  name: "Sumit"
};
function greet(city, country) {
  console.log(`Hello ${this.name} from ${city}, ${country}`);
}
greet.call(user, "Delhi", "India");


//Hello Sumit from Delhi, India

```

### apply() Method
 Same as call(), but arguments are passed as an array.

### Syntax
functionName.apply(thisArg, [arg1, arg2]);

**Example:**
```
const user = {
  name: "Umakant"
};
function greet(city, country) {
  console.log(`Hello ${this.name} from ${city}, ${country}`);
}
greet.apply(user, ["Mumbai", "India"]);

//Hello Sumit from Mumbai, India
```
### bind() Method
 Does not execute immediately.
It returns a new function with this permanently bound.
### Syntax
const newFunction = functionName.bind(thisArg, arg1, arg2);

**Example:**
```
let nameObj = {
    name: "Tony"
}

let PrintName = {
    name: "steve",
    sayHi: function () {

        // Here "this" points to nameObj
        console.log(this.name); 
    }
}

let HiFun = PrintName.sayHi.bind(nameObj);
HiFun();

//Tony

```
## 1️⃣5️⃣ Currying in JavaScript
Currying is a technique where a function with multiple arguments is transformed into a sequence of functions, each taking one argument at a time.

**Example:**
```
// Normal Function
function add(a, b) {
     return a + b;
 }
 console.log(add(2, 3)); 

-----

// Function Currying
function add(a) {
    return function(b) {
        return a + b;
    }
}

const addTwo = add(5);  // First function call with 5
console.log(addTwo(4));

Output
9
```
* Normal Function: Directly takes two arguments (a and b) and returns their sum.
* Function Currying: Breaks the add function into two steps. First, it takes a, and then, when calling addTwo(4), it takes b and returns the sum.


### Advantages
1. Function reusability
2. Partial application
3. Cleaner and readable code
4. Useful in functional programming
5. Helps avoid repeated arguments

## 1️⃣6️⃣ Debouncing and Throttling

### 🔹 Debouncing
Debouncing ensures that a function runs only after a certain time has passed since the last event.

### 🔹 Use Cases
* Search input (API call after user stops typing)
* Window resize
* Form validation
* Auto-save drafts

### Syntax
```js
function debounce(fn, delay) {
  let timer;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}
```

### 🔹 Throttling
Throttling is a technique that ensures a function executes at most once within a specified time interval, even if the event is triggered multiple times.

### Use Cases
* Scroll events
* Mouse move / drag
* Button click rate-limiting
* Window resize (performance control)

# Syntax
```
function throttle(fn, limit) {
  let inThrottle = false;

  return function (...args) {
    if (!inThrottle) {
      fn.apply(this, args);
      inThrottle = true;

      setTimeout(() => {
        inThrottle = false;
      }, limit);
    }
  };
}
```


## 1️⃣7️⃣ Pure Functions and Side Effects

### 🔹 Pure Function
A pure function is a function that:
* Always returns the same output for the same input
* Does not modify any external state (no side effects)

### Characteristics of Pure Functions
* No global variable usage
* No mutation of input parameters
* No API calls, DOM updates, or console logs
* Easy to test and debug

**Example:**
```
function add(a, b) {
  return a + b;
}
add(2, 3); // always returns 5
```

### Impure Function (Side Effect)
An impure function is a function that:
• Modifies external state
• Depends on variables outside its scope
• Produces different results for the same input
```
let total = 0;
function addToTotal(x) {
  total += x;
}
```

### Why is this impure?
* It modifies the external variable total
* Output depends on previous executions

### 🌟 Benefits of Pure Functions

*  Easy to test
*  Predictable behaviour
*  Safe for concurrency
*  emoization-friendly
*  idely used in React, Redux, functional programming

 ## 1️⃣8️⃣ Generators and Iterators
 
### Iterators
An iterator is an object that provides a way to access elements sequentially using the next() method.

### Key Points
• Uses a next() method
• next() returns { value, done }
• Used internally by for...of

**Example:**
```
const arr = [10, 20, 30];

const iterator = arr[Symbol.iterator]();

console.log(iterator.next()); // { value: 10, done: false }
console.log(iterator.next()); // { value: 20, done: false }
console.log(iterator.next()); // { value: 30, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

### Generators
A generator is a special type of function that can pause and resume execution.

### Key Points
• Defined using function*
• Uses yield keyword
• Automatically creates an iterator
• Execution can be paused and resumed

**Example:**

```
function* generateNumbers() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = generateNumbers();

console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }
```

-----

## 1️⃣9️⃣ Memoization

Memoization is an optimization technique where we cache the result of expensive function calls and reuse the cached result when the same inputs occur again.

**Example:**
```
function memoize(fn) {
  const cache = {};

  return function (n) {
    if (cache[n]) {
      return cache[n]; // return cached result
    }

    cache[n] = fn(n); // store result
    return cache[n];
  };
}
```


### Why Memoization?
*  Improves performance
*  Reduces repeated calculations
*  Useful for pure functions
*  recursion, React

## 2️⃣0️⃣ Event Delegation

Event Delegation is a JavaScript design pattern where a single event listener is attached to a parent element to manage events triggered by its child elements using event bubbling.

### Problem Without Event Delegation
```html
<button>One</button>
<button>Two</button>
<button>Three</button>
```

```js
buttons.forEach(button => {
  button.addEventListener("click", () => {
    console.log("Button clicked");
  });
});
```
* Too many event listeners
* More memory usage
* New buttons will not work automatically

### Solution Using Event Delegation

```html
<div id="container">
  <button>One</button>
  <button>Two</button>
  <button>Three</button>
</div>
```
```js
document.getElementById("container").addEventListener("click", function (event) {
  if (event.target.tagName === "BUTTON") {
    console.log(event.target.innerText);
  }
});
```
### Benefits of Event Delegation
1. Better Performance
2. Less Memory Usage
3. Cleaner and Simple Code
4. Easy to Manage
5. Scalable for Large Applications

## 2️⃣1️⃣ How `setTimeout` Works

### What is setTimeout?
setTimeout schedules a function to run after a minimum delay, not exactly at that time.

# Syntax

```
setTimeout(callback, delay);
```
### How setTimeout Works
JavaScript uses Event Loop + Web APIs

### 1️⃣ Call Stack
All synchronous code runs here first.

### 2️⃣ Web APIs (Browser / Node)
setTimeout is handled by Web APIs, not the JS engine.

### 3️⃣ Callback Queue
After the timer expires, the callback waits here.

### 4️⃣ Event Loop
Moves callback to Call Stack only when stack is empty.

**Example:**
```
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 2000);

console.log("End");

// Output
Start
End
Timeout
```

## 2️⃣2️⃣ Type Coercion
Type Coercion is JavaScript’s automatic conversion of one data type into another during operations.
JavaScript is a loosely typed language, so it converts types when needed.

### Types of Type Coercion
1️⃣ Implicit Type Coercion (Automatic)
2️⃣ Explicit Type Coercion (Manual)

### Implicit Type Coercion (Automatic)
JavaScript converts types by itself.

**Example:**
```
"5" + 2     // "52"
"5" - 2     // 3
true + 1    // 2
false + 1   // 1
```
### Explicit Type Coercion (Manual)
Developer forces conversion.

**Example:**
```
Number("5")     // 5
String(100)     // "100"
Boolean(1)      // true
```


## 2️⃣3️⃣ Truthy and Falsy Values

### What Are Truthy Values?
Truthy values are values that are evaluated to be true when used in a Boolean context. Simply put, any value that is not explicitly falsy is considered truthy.

### These are some truthy values
* Non-zero numbers: 42, -1, 3.14
* Non-empty strings: "hello", "0", " "
* Objects and arrays: {}, []
* Functions: function() {}
* Dates: new Date()
* Symbols: Symbol()
* BigInt values other than 0n: 10n

### What Are Falsy Values?
Falsy values are values that evaluate to false when used in a Boolean. JavaScript has a fixed list of falsy values

* false
* 0
* ""
* null
* undefined
* NaN

**Example:**
```
if ("0") {
  console.log("Truthy");
}
// Output: Truthy
```

## 2️⃣4️⃣ Map and Set

### What is Map?
Map is a collection of key–value pairs where keys can be of any type.

### Why Map?
* Keys can be objects, functions, primitives
* Maintains insertion order
* Better performance than object for frequent add/remove

**Example:**
```
const map = new Map();

map.set("name", "Amit");
map.set(1, "one");
map.set(true, "yes");

console.log(map.get("name")); // Amit
console.log(map.size);        // 3
```

### What is Set?
Set is a collection of unique values (no duplicates).

### Why Set?
* Automatically removes duplicates
* Fast lookup
* Maintains insertion order

**Example:**
```
const set = new Set();

set.add(1);
set.add(2);
set.add(2); // ignored

console.log(set); // Set {1, 2}
```
## 2️⃣5️⃣ WeakMap and WeakSet


### What is WeakMap?
A collection of key–value pairs where:

* Keys must be objects
* Keys are weakly referenced
*  Not iterable

**Example:**
```
const wm = new WeakMap();
let user = { name: "Rahul" };
wm.set(user, "logged-in");
console.log(wm.get(user)); // logged-in
```

### WeakMap Methods
```
Method	             Use
set(key, value)      Add
get(key)	           Read
has(key)	           Check
delete(key)	         Remove
```

### What is WeakSet?
A collection of objects only, stored weakly.

**Example:**
```
const ws = new WeakSet();
let obj = { id: 1 };
ws.add(obj);
console.log(ws.has(obj)); // true
```

### WeakSet Methods
```
Method	        Use
add(value)	    Add object
has(value)	    Check
delete(value) 	Remove
```

## 2️⃣6️⃣ Module Pattern


### What is Module Pattern?
The Module Pattern is a way to:
* Encapsulate data
* Hide private variables
* Expose only public methods
 It is based on closures.

**Example:**
```
const myModule = (function () {
  // private variable
  let count = 0;

  // private function
  function logCount() {
    console.log(count);
  }

  // public API
  return {
    increment() {
      count++;
      logCount();
    },
    reset() {
      count = 0;
    },
  };
})();


///Usage

myModule.increment(); // 1
myModule.increment(); // 2
myModule.reset();

Key Point : count is NOT accessible directly

```
## 2️⃣7️⃣ map, filter, reduce

### 1️⃣ map()
map is an array method that creates a new array by applying a callback function to each element of the original array.
Used when you want to transform elements without changing the original array.

### Syntax
```
array.map((element, index, array) => {
  // return transformed element
});
```

**Example:**
```
const numbers = [1, 2, 3, 4];

const squares = numbers.map(num => num * num);

console.log(squares); // [1, 4, 9, 16]
console.log(numbers); // [1, 2, 3, 4] → original array unchanged
```

### 2️⃣ filter()
filter is an array method that creates a new array containing only elements that pass a given condition.
Used when you want to select or filter items from an array.

### Syntax
```
array.filter((element, index, array) => {
  // return true to keep element
});
```

**Example:**
```
const numbers = [1, 2, 3, 4, 5];

const evenNumbers = numbers.filter(num => num % 2 === 0);

console.log(evenNumbers); // [2, 4]
console.log(numbers);     // [1, 2, 3, 4, 5] → original array unchanged
```
### 3️⃣ reduce()
reduce is an array method that reduces all array elements into a single value by executing a callback function on each element, optionally with an initial value.
Used for sum, count, flattening, or building objects from arrays.

### Syntax

```
array.reduce((accumulator, currentValue, index, array) => {
  // return updated accumulator
}, initialValue);
```

**Example:**
```
const numbers = [1, 2, 3, 4];
const sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 10
```


## 2️⃣8️⃣ Deep Copy vs Shallow Copy

### Shallow Copy
A shallow copy copies only the first level of an object.
If the object contains nested objects, they still share the same reference.

**Example:**
```
const user = {
  name: "Amit",
  address: {
    city: "Delhi"
  }
};

const copyUser = { ...user };

copyUser.name = "Rahul";
copyUser.address.city = "Mumbai";

console.log(user.name);           // Amit ✅
console.log(user.address.city);   // Mumbai ❌ (changed)

```

### Deep Copy
A deep copy copies all levels of an object.
Changes in the copied object do NOT affect the original.

**Example:**
```
const user = {
  name: "Amit",
  address: {
    city: "Delhi"
  }
};

// DEEP COPY using JSON
const copyUser = JSON.parse(JSON.stringify(user));

copyUser.name = "Rahul";
copyUser.address.city = "Mumbai";

console.log(user.name);           // Amit ✅
console.log(user.address.city);   // Delhi ✅ (NOT changed)

console.log(copyUser.name);        // Rahul
console.log(copyUser.address.city);// Mumbai

```
## 2️⃣9️⃣ Optional Chaining & Nullish Coalescing

### 1️⃣ Optional Chaining (?.)
Optional Chaining allows safe access to nested object properties or methods without throwing an error if a property is null or undefined.
Instead of error → returns undefined

### Syntax
```js
object?.property
object?.[expression]
object?.method?.()
```
**Example:(Without Optional Chaining )**
```
const user = {};
console.log(user.address.city);
// TypeError: Cannot read property 'city'
```
**Example: (With Optional Chaining )**
```
const user = {};
console.log(user.address?.city);
// undefined (NO error)
```
### 2️⃣ Nullish Coalescing (??)
Nullish Coalescing returns the right-hand value only when the left-hand value is null or undefined.
NOT for 0, false, or ""

### Syntax
```
value ?? defaultValue
```

**Example:**

```
const score = 0;
console.log(score || 100); // 100  (wrong)
console.log(score ?? 100); // 0  (correct)

* || treats 0, false, "" as falsy
* ?? checks only null or undefined
```


## 3️⃣0️⃣ Garbage Collection
Garbage Collection (GC) is an automatic memory management process in JavaScript that frees memory by removing objects that are no longer reachable.
JavaScript automatically handles memory allocation and de-allocation.

### Why Garbage Collection is Needed?
* Prevents memory leaks
* Optimises memory usage
* Developer doesn’t need to manually free memory

 ###  Algorithm: Mark & Sweep
* Mark all reachable objects
* Sweep (delete) unmarked objects

  **Example:**
```
let user = { name: "Amit" };
let admin = user;

user = null;
admin = null;
```

# 🌟 ES6+ Modern JavaScript

## 3️⃣1️⃣ Arrow vs Regular Functions

### 1️⃣ Regular Function

### Definition
A regular function is defined using the function keyword and has its own this, arguments, and prototype.


### Syntax
```
function add(a, b) {
  return a + b;
}
```

**Example:**

```
const user = {
  name: "Amit",
  greet: function () {
    console.log(this.name);
  }
};

user.greet(); // Amit
```

### 2️⃣ Arrow Function

 ### Definition
An arrow function is a shorter syntax for writing functions in JavaScript.

### Syntax
```
const functionName = () => {
  // statements(s);
};

```

**Example:**

```
const add = (a, b) => a + b;
console.log(add(2, 3)); // 5
```

## 3️⃣2️⃣ Destructuring Assignment

### What is Destructuring Assignment?
Destructuring assignment is a JavaScript feature that allows you to extract values from arrays or objects and store them into variables in a clean and readable way.

### Object Destructuring
###  Syntax
```
const { key1, key2 } = object;
```
**Example:**
```
const user = {
  name: "Amit",
  age: 22,
};
const { name, age } = user;
console.log(name); // Amit
console.log(age);  // 22
```

### Array Destructuring
###  Syntax
```
const [a, b] = array;
```
**Example:**
```
const colors = ["red", "green", "blue"];
const [first, second] = colors;
console.log(first);  // red
console.log(second); // green

```

## 3️⃣3️⃣ Template Literals
Template literals are a JavaScript feature (ES6) that allow you to create strings with variables, expressions, and multi-line text easily using backticks ( ) instead of quotes

### Syntax
```
`string text`
```

**Example:**

### Old Way
```
const name = "Amit";
console.log("Hello " + name);
```

### Template Literal Way
```
const name = "Amit";
console.log(`Hello ${name}`);
```


## 3️⃣4️⃣ Default Parameters
Default parameters allow you to set a default value for function parameters.
If no argument (or undefined) is passed, the default value is used automatically.

### Syntax
```
function functionName(a = defaultValue, b=defaultValue) {
  // code
}
```

**Example:**
```
function greet(name = "Abhi") {
  console.log(`Hello ${name}`);
}

greet("Amit");   // Hello Amit
greet();         // Hello Abhi
```

## 3️⃣5️⃣ Rest & Spread

###  Rest Operator
The rest operator collects multiple values into a single array.
Used in function parameters and destructuring.

### Syntax
```
function func(...args) {
  // args is an array
}
```

**Example:**

```
function sum(...numbers) {
  console.log(numbers);
}
sum(1, 2, 3, 4);

//Output:
[1, 2, 3, 4]
```

### Spread Operator
The spread operator expands array or object elements into individual values.
Used for copying, merging, passing arguments.

### Syntax
```
const newArr = [...oldArr];
```

**Example:**

```
const nums = [1, 2, 3];
const copy = [...nums];
console.log(copy); // [1, 2, 3]
```

## 3️⃣6️⃣ Classes vs Function Constructors

### Function Constructor
A function constructor is a normal function used with the new keyword to create objects.
Properties are assigned using this, and methods are usually added to the prototype.

**Example:**
```
function Car(make, model) {
  this.make = make;
  this.model = model;
}
const myCar = new Car('Toyota', 'Camry'); // Creates a new Car object

```

### Class Constructors
The class syntax provides a cleaner, more structured approach to object-oriented programming, with a dedicated constructor method inside the class definition.

**Example:**
```
class Car {
  constructor(make, model) {
    this.make = make;
    this.model = model;
  }
}
const myCar = new Car('Toyota', 'Camry'); // Creates a new Car object

```

## 3️⃣7️⃣ Symbol
Symbol is a primitive data type used to create unique, non-enumerable object keys to avoid naming conflicts.

**Example:**
```
const id1 = Symbol("id");
const id2 = Symbol("id");

console.log(id1 === id2); // false
```

### Why Do We Need Symbol?
Symbols are mainly used to:
*  Avoid property name conflicts
*  Create hidden / private object properties
*  Define well-known behaviors in JavaScript


## 3️⃣8️⃣ Map & Set (ES6)

### What is Map?
Map is a collection of key–value pairs where keys can be of any type.

### Why Map?
* Keys can be objects, functions, primitives
* Maintains insertion order
* Better performance than object for frequent add/remove

**Example:**
```
const map = new Map();

map.set("name", "Amit");
map.set(1, "one");
map.set(true, "yes");

console.log(map.get("name")); // Amit
console.log(map.size);        // 3
```

### What is Set?
Set is a collection of unique values (no duplicates).

### Why Set?
* Automatically removes duplicates
* Fast lookup
* Maintains insertion order

**Example:**
```
const set = new Set();

set.add(1);
set.add(2);
set.add(2); // ignored

console.log(set); // Set {1, 2}
```

## 3️⃣9️⃣ for...of Loop
for...of loop is used to iterate over iterable objects and gives values directly, not keys.

### Basic Syntax
```
for (const value of iterable) {
  // use value
}
```
**Example:**

```
const nums = [10, 20, 30];
for (const num of nums) {
  console.log(num);
}

//Output: 10 20 30
```

## 4️⃣0️⃣ Tagged Templates

Tagged template is a function that processes a template literal and gives you full control over how the string and expressions are combined.

### Normal Template Literal

```
const name = "Aman";
const age = 25;
console.log(`My name is ${name} and I am ${age}`);

```

### Tagged Template Syntax

```
tagFunction`string text ${expression} more text`;

```
**Example:**

```
function tag(strings, values) {
  console.log(strings);
  console.log(values);
}
tag`Hello ${"Aman"}!`;


//Output:
["Hello ", "!"]
"Aman"

```

---

## ✅ Final Notes

* Covers **core JavaScript interview concepts**
* Suitable for **mid → senior‑level roles**
* Can be extended with examples & diagrams

---

### ⭐ If this repo helped you, don’t forget to star it

**Author:** Amit Chauhan  
**Topic:** Core JavaScript Interview Preparation

---

## 41. Async / Await

async / await is syntactic sugar over Promises that allows us to write asynchronous code that looks synchronous, making it easier to read and maintain.


### Key Points
* async functions always return a Promise
* await waits for a Promise to resolve or reject
* Code execution pauses only inside the async function
* Errors are handled using try / catch

**Example:**
```
async function fetchUser() {
  try {
    const res = await fetch('/api/user');
    const data = await res.json();
    return data;
  } catch (err) {
    console.error(err);
  }
}
```

## 42. Async Iterators

Async Iterators allow us to iterate over asynchronous data sources where values arrive over time, not immediately.

**Use Cases:**

* APIs
* Streams
* Databases
* WebSockets


**Example:**

```js
async function* streamData() {
  yield await Promise.resolve(1);
  yield await Promise.resolve(2);
}

for await (const val of streamData()) {
  console.log(val);
}
```

## 43. Private Class Fields
Private class fields are class properties or methods that are accessible only inside the class.
They are declared using the # (hash) symbol and provide true encapsulation in JavaScript.

**Key Points**
* Declared using # (hash symbol)
* Accessible only inside the class
* Not accessible using this.fieldName without #
* Improves security and encapsulation
* Supported in modern browsers and Node.js

# Synatx 
```
class ClassName {
  #privateField;

  constructor(value) {
    this.#privateField = value;
  }
}

```
**Benefits**
* True encapsulation
* Prevents accidental or unauthorized access
* Improves security and maintainability

**Example:**
```
class User {
  // Private field (sirf class ke andar accessible)
  #password;

  // Constructor runs when object is created
  constructor(pwd) {
    this.#password = pwd;
  }

  // Public method to check password
  checkPassword(input) {
    return this.#password === input;
  }
}

// 🔹 Usage
const user = new User("12345");

// Correct way (allowed)
console.log(user.checkPassword("12345")); // true
console.log(user.checkPassword("00000")); // false

// Wrong way (not allowed)
// console.log(user.#password); // Syntax Error


```

## 44. Static Class Members

Static members are properties or methods that belong to the class itself, not to individual objects (instances).

**Use Case**
* Utility or helper methods
* Constants shared across all objects
* Counters or shared data


#  Syntax
```
class ClassName {
  static methodName() {
    // logic
  }

  static propertyName = value;
}
```

**Example**
```
class MathUtil {
  // Static method
  static add(a, b) {
    return a + b;
  }

  // Static property
  static PI = 3.1416;
}

// Access using class name
console.log(MathUtil.add(2, 3)); // 5
console.log(MathUtil.PI);        // 3.1416

// Not accessible from instances
const util = new MathUtil();
console.log(util.add); // undefined

```

## 45. Promise Combinators

Promise combinators are methods that allow working with multiple Promises at the same time.
They help to run, wait, or manage multiple async operations efficiently.

**Key Combinators**

* Promise.all() – Waits for all promises; fails if any promise rejects (fast fail)
* Promise.race() – Returns the first settled promise (resolve or reject)
* Promise.any() – Returns the first fulfilled promise; ignores rejected promises
* Promise.allSettled() – Waits for all promises to settle; gives status of each

**Example:**
```
const p1 = Promise.resolve("A");
const p2 = Promise.resolve("B");
const p3 = Promise.reject("C");

// 1️⃣ Promise.all – fails if any rejects
Promise.all([p1, p2])
  .then(res => console.log("All:", res)); // ["A", "B"]

// 2️⃣ Promise.race – first settled
Promise.race([p1, p2, p3])
  .then(res => console.log("Race:", res))
  .catch(err => console.log("Race Error:", err)); // "A"

// 3️⃣ Promise.any – first fulfilled, ignores rejects
Promise.any([p3, p2])
  .then(res => console.log("Any:", res)); // "B"

// 4️⃣ Promise.allSettled – get all results
Promise.allSettled([p1, p2, p3])
  .then(res => console.log("All Settled:", res));
/* Output:
[
  { status: "fulfilled", value: "A" },
  { status: "fulfilled", value: "B" },
  { status: "rejected", reason: "C" }
]
*/
```



## 46. Reflect API

Reflect API is a built-in object that provides methods to intercept and operate on JavaScript objects.
It’s similar to Object methods but designed to work with proxies and meta-programming.


### Key Points
* Introduced in ES6
* Contains static methods only
* Works on objects, properties, and functions
* Often used with Proxy objects
* Makes code clearer and consistent

# Syntax
```
Reflect.methodName(target, property[, value, receiver]);
```

### Common Methods
```
Method	                                  Description
Reflect.get(target, property)           	Get property value
Reflect.set(target, property, value)	    Set property value
Reflect.has(target, property)            	Checks if property exists
Reflect.deleteProperty(target, property)	Deletes property
Reflect.apply(target, thisArg, args)     	Calls a function with arguments

```

**Example**
```
const user = { name: "Aman", age: 25 };

// Get property
console.log(Reflect.get(user, "name")); // Aman

// Set property
Reflect.set(user, "age", 26);
console.log(user.age); // 26
```


## 47. Proxies
Proxy is a special object in JavaScript that wraps another object and allows you to intercept and customize operations like:
* Property read/write
* Function calls
* Property deletion
* Object iteration

**Use Cases:**
* Validation – e.g., check property values before setting
* Logging – monitor property access or function calls
* Default values – return default for missing properties
* Security – restrict access or modification
* Meta-programming – advanced JS programming patterns


### Syntax
```
const proxy = new Proxy(target, handler);

target → original object to wrap
handler → object containing traps (methods to intercept operations)

```

### Common Traps (Handler Methods)
```
Trap	                          Description
get(target, prop)	              Intercepts property access
set(target, prop, value)	      Intercepts property assignment
has(target, prop)	              Intercepts in operator
deleteProperty(target, prop)   	Intercepts delete
apply(target, thisArg, args)	  Intercepts function calls
```

**Example**

```
const user = { name: "Aman", age: 25 };

const proxyUser = new Proxy(user, {
  get(target, prop) {
    console.log(`Accessing ${prop}`);
    return target[prop];
  }
});

console.log(proxyUser.name); // Logs: Accessing name, Output: Aman
console.log(proxyUser.age);  // Logs: Accessing age, Output: 25
```

## 48. Web Components
Web Components are reusable custom elements with their own encapsulated HTML, CSS, and JavaScript.
They allow developers to create modular, maintainable, and framework-independent UI components.

### Why Use Web Components?
* Encapsulation → Styles & markup don’t leak
* Reusability → Build once, use anywhere
* Framework-independent → Works with React, Angular, Vue, or vanilla JS
* Maintainability → Components isolated, easier to update

### Parts of Web Components
* Custom Elements – Define your own HTML tags
* Shadow DOM – Encapsulate markup & styles
* Templates – Reusable HTML structures

### Syntax – Custom Element
```
customElements.define('my-card', class extends HTMLElement {
  constructor() {
    super();
    // Optional: attach shadow DOM
    this.attachShadow({ mode: "open" });
    this.shadowRoot.innerHTML = `<p>Hello from Web Component!</p>`;
  }
});
```

### Usage in HTML
```
<my-card></my-card>
```


## 49. Intl Object
“Intl object provides built-in internationalization support for formatting numbers, dates, strings, and relative time based on locale.”

### Intl Constructor-Based Objects
* Intl.NumberFormat → Format numbers and currencies
* Intl.DateTimeFormat → Format dates & times
* Intl.Collator → Compare strings
* Intl.RelativeTimeFormat → Format relative time


**Example**

```
const number = 1234567.89;

// Format as US currency
console.log(new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(number));
// Output: $1,234,567.89

```



## 50. Polyfills

A Polyfill is a piece of JavaScript code that adds missing features to older browsers, so that modern JavaScript works everywhere.

### Why Polyfills Are Needed

* Old browsers don’t support new JS features
* To ensure cross-browser compatibility
* Used for ES6+ features like:
   a. Array.map
   b. Promise
   c. fetch
   d. Object.assign


**Example**
```
const nums = [1, 2, 3];
const doubled = nums.map(n => n * 2);

console.log(doubled); // [2, 4, 6]
```


## 51. JavaScript Performance Optimization

JavaScript Performance Optimization means writing JS code in a way that executes faster, uses less memory, and gives a smooth user experience.

### Why Performance Matters
* Faster page load 
* Smooth UI (no lag, no freeze)
* Better SEO & Core Web Vitals

### Common Performance Techniques


 ### 1️⃣ Debouncing
 ### Definition:
   Debouncing delays the execution of a function until the user stops triggering it for a specified time.

### Use case:
* Search input
* Window resize
* Typing events

**Example:**
```
function debounce(fn, delay) {
  let timer;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn(...args), delay);
  };
}

```

### 2️⃣ Throttling

### Definition:
Throttling ensures a function executes at most once in a given time interval, no matter how many times the event fires.

### Use case:
* Scroll
* Button clicks
* Mouse move

**Example:**
```
function throttle(fn, limit) {
  let flag = true;
  return function (...args) {
    if (!flag) return;
    flag = false;
    fn(...args);
    setTimeout(() => flag = true, limit);
  };
}
```

### 3️⃣ Memoization

### Definition:
Memoization is an optimization technique where function results are cached so repeated calls with the same input return faster.

### Use case:
* Expensive calculations
* Recursive functions
* Repeated API results

**Example:**

```
const memo = fn => {
  const cache = {};
  return x => cache[x] ?? (cache[x] = fn(x));
};
```

### 4️⃣ Lazy Loading

### Definition:
Lazy loading means loading resources only when they are needed, instead of loading everything at once.

### Use case:
* Large JS files
* Images
* Modules

**Example:**
```
import("./heavyModule.js").then(module => {
  module.run();
});

```


## 52. Event Delegation

Event Delegation is a technique where a single event listener is attached to a parent element to handle events for its child elements using event bubbling, improving performance and memory usage.

### Key Concept: Event Bubbling
When an event occurs on a child element, it bubbles up to its parent.
```
Child → Parent → Document
```
Event Delegation uses this behavior.


###Advantages

* Better performance
* Less memory usage
* Handles dynamic elements
* Cleaner code

**Example**
```
document.getElementById('list').addEventListener('click', e => {
  if (e.target.tagName === 'LI') {
    console.log(e.target.textContent);
  }
});

```


## 53. Lazy Loading

Lazy Loading is a technique where resources are loaded only when they are needed, instead of loading everything at once, to improve performance.


### Why Use Lazy Loading?
* Faster initial page load
* Reduced memory usage
* Better user experience
* Improved performance on slow networks

### Common Use Cases
* Images
* JavaScript modules
* Components in large applications

# Image Lazy Loading
<img src="image.jpg" loading="lazy" alt="image" />

# JavaScript Lazy Loading (Dynamic Import)
import('./heavyModule.js').then(module => {
  module.run();
});

## 54. Optimize Large Applications

Optimizing large applications means keeping the app fast, responsive, and scalable even when the codebase, users, and data grow.

### Code Splitting
Break a large JavaScript bundle into smaller chunks and load them on demand.

**Benefit:** Faster initial load.
```
import('./feature.js').then(module => {
  module.init();
});
```

### Tree Shaking
Remove unused code from the final bundle during build time.
**Benefit:** Smaller bundle size, faster download.
**Works with:** ES Modules (import / export)

### CDN Caching
Serve static assets (JS, CSS, images) from a Content Delivery Network close to the user.

**Benefit:**
* Faster asset delivery
* Reduced server load
* Better global performance


  
## 55. Service Workers

A Service Worker is a background JavaScript script that runs independently of the web page and enables advanced features such as offline support, request interception, caching, push notifications, and background synchronization.


### Key Characteristics
* Runs in the background
* No direct access to DOM
* Works only on HTTPS
* Event-driven (install, activate, fetch)
* Enables Progressive Web Apps (PWA)


### Why Service Workers Are Used
* Offline functionality
* Faster loading using cache
* Reduced network usage
* Background tasks
* Push notifications

### Life Cycle of Service Worker

* Install → Cache static assets
* Activate → Clean old caches
* Fetch → Intercept network requests

  
**Example:**
```
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js')
    .then(() => console.log('Service Worker Registered'))
    .catch(err => console.error('SW registration failed', err));
}
```

### Real-World Use Case
* Google Maps → Offline maps
* E-commerce → Faster page loads
* News apps → Cached articles
* PWA apps → App-like experience


## 56. Memory Leaks

A memory leak occurs when a program allocates memory that is no longer needed but is not released, causing the application to consume more memory over time and eventually degrade performance or crash.

In JavaScript, memory leaks usually happen when objects remain reachable due to unintended references, preventing the garbage collector from freeing memory.


### Common Causes of Memory Leaks

# 1️⃣ Global Variables

Objects stored in global scope are never garbage-collected.

###  Bad Example
```
userData = { name: "Rahul", age: 25 }; // implicit global
```
###  Fix
```
let userData = { name: "Rahul", age: 25 };
userData = null;

```

# 2️⃣ Timers & Intervals

setInterval and setTimeout keep references alive if not cleared.

### Bad Example

```
const timer = setInterval(() => {
  console.log("Running...");
}, 1000);
```

###  Fix
```
clearInterval(timer);

```

# 3️⃣ Closures Holding References

Closures may retain large objects even after they are no longer required.

### Bad Example
```
function createHandler() {
  const largeData = new Array(1000000).fill("data");
  
  return function () {
    console.log(largeData.length);
  };
}
```

###  Fix
```
function createHandler() {
  let largeData = new Array(1000000).fill("data");
  
  return function () {
    console.log(largeData.length);
    largeData = null;
  };
}
```

# 4️⃣ DOM References Not Removed

Detached DOM elements still referenced in JS cannot be garbage-collected.

###  Bad Example
```
let el = document.getElementById("box");
document.body.removeChild(el);
// el still in memory

```
###  Fix
```
el.remove();
el = null;
```

### How to Detect Memory Leaks
* Browser DevTools → Memory tab
* Heap snapshots
* Performance profiling


## 57. Tree Shaking

Tree Shaking is a build-time optimization technique that removes unused ES6 module exports from the final JavaScript bundle, resulting in smaller bundle size and better performance.

### Why Tree Shaking Is Important
* Reduces JavaScript bundle size
* Improves page load time
* Eliminates dead code
* Optimizes production builds

### Key Requirements
* Must use ES6 modules (import / export)
* Bundler support (Webpack, Rollup, Vite)
* Works best in production mode
* No side effects in unused code


**Example**

````utils.js

export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}
````

````main.js
import { add } from './utils.js';

console.log(add(2, 3));


````

## 58️ Code Splitting

### **Definition **  
**Code Splitting** is a **performance optimization technique** that breaks a large JavaScript bundle into **smaller chunks** and loads them **only when required**.  
This helps reduce **initial load time**, improve **performance**, and enhance **user experience**.

Code splitting is commonly implemented using **dynamic `import()`**, which loads modules **on demand**.

---

### **Why Code Splitting Is Important**
- Faster initial page load
- Reduced bundle size
- Better performance on slow networks
- Efficient resource usage

---

### **How Code Splitting Works**
- Application loads the **core code first**
- Additional code is fetched **only when needed**
- Unused code is not loaded upfront

---

## **Basic Example**

### **module.js**
```js
export function run() {
  console.log("Module loaded on demand!");
}
```

## 59️ Performance Profiling

### **Definition**  
**Performance Profiling** is the process of **measuring, analyzing, and identifying performance bottlenecks** in an application using tools and metrics, in order to improve **speed, responsiveness, and resource usage**.

It helps developers understand **where time and memory are being spent** during execution.

---

### **Why Performance Profiling Is Important**
- Identifies slow functions and rendering issues
- Detects memory leaks and excessive re-renders
- Improves load time and runtime performance
- Enhances user experience

---

## **Common Performance Profiling Tools**

### **1️⃣ Chrome DevTools**
Used for **runtime performance analysis**.

**Key Features:**
- Performance tab → CPU, JS execution, rendering
- Memory tab → Heap snapshots, memory leaks
- Network tab → API timing, payload size

**Example Use Case:**
```text
Record page load → find long tasks → optimize heavy functions

```

### **2️⃣ Lighthouse**
Used for automated performance audits.

**Metrics Analyzed:**
* First Contentful Paint (FCP)
* Largest Contentful Paint (LCP)
* Time to Interactive (TTI)
* Cumulative Layout Shift (CLS)

**Example:**
```
Run Lighthouse → get performance score → apply recommendations
```


### **3️⃣ RUM (Real User Monitoring) Tools**
Used for real-world performance tracking from actual users.

### Popular RUM Tools:
* Google Analytics
* New Relic
* Datadog
* Sentry

### What They Measure:
* Page load time
* User interaction delays
* Errors in production

---
### **Example Scenario**
```
Problem: Page loads slowly in production
Action:
- Use Chrome DevTools → identify long JS tasks
- Use Lighthouse → check performance score
- Use RUM tools → analyze real user data
Solution:
- Optimize code and reduce bundle size
```


## 60 requestAnimationFrame

### **Definition**  
`requestAnimationFrame` is a **browser API** used to create **smooth, high-performance animations** by synchronizing JavaScript execution with the browser’s **screen refresh rate**.

It tells the browser to **run the animation callback just before the next repaint**, making animations more efficient and visually smooth.

---

### **Why requestAnimationFrame Is Used**
- Smooth animations (60 FPS when possible)
- Optimized CPU and GPU usage
- Pauses automatically in inactive tabs
- Better performance than `setTimeout` / `setInterval`

---

## **Basic Example**
```js
function animate() {
  console.log("Animating...");
  requestAnimationFrame(animate);
}

animate();
```


# TESTING & DEBUGGING


## 66 Types of Testing

Software testing ensures that an application works **correctly, reliably, and as expected**. Different types of testing focus on different levels of the application.

---

## **1️⃣ Unit Testing**

### **Definition**  
**Unit testing** tests **individual functions or components in isolation** to verify that they work correctly.

- Smallest level of testing
- Fast to execute
- No dependency on external systems

### **Example**
```js
function add(a, b) {
  return a + b;
}

test('adds two numbers', () => {
  expect(add(2, 3)).toBe(5);
});

```

### Tools
* Jest
* Mocha
* Jasmine


# 2️⃣ Integration Testing

## Definition
**Integration Testing** verifies that **multiple modules or components work together correctly** by testing their interactions, data flow, and communication.

Unlike unit testing (which tests components in isolation), integration testing ensures that **combined units behave as expected**.

---

## Why Integration Testing Is Important
- Detects issues in module interaction
- Validates API and database communication
- Ensures correct data flow between components
- Catches bugs missed by unit tests

---

## What Is Tested in Integration Testing
- API ↔ Database
- Frontend ↔ Backend
- Service ↔ Service
- Module ↔ Module

---

## Example (Node.js + Express)

### API Route
```js
app.post('/users', async (req, res) => {
  const user = await User.create(req.body);
  res.status(201).json(user);
});
```

### Tools
* Jest
* Supertest
* Mocha


### 3️⃣ End-to-End (E2E) Testing

### **Definition**

End-to-End testing validates the complete user flow of an application from start to finish, simulating real user behavior.

* Tests UI, backend, and database together
* Slow but highly reliable

## **Example**
```
cy.visit('/login');
cy.get('#email').type('test@mail.com');
cy.get('#password').type('123456');
cy.get('button').click();
```

### Tools
* Cypress
* Playwright
* Selenium

### 4️⃣ Snapshot Testing

## **Definition**
Snapshot testing captures the UI output of a component and compares it with a previously saved snapshot to detect unintended changes.

* Common in UI testing
* Ensures UI consistency

## **Example**
```
const tree = renderer.create(<Button />).toJSON();
expect(tree).toMatchSnapshot();
```

### Tools
* Jest
* React Test Renderer



## 67. Unit Testing

### **Definition**  
**Unit testing** tests **individual functions or components in isolation** to verify that they work correctly.

- Smallest level of testing
- Fast to execute
- No dependency on external systems

### **Example**
```js
function add(a, b) {
  return a + b;
}

test('adds two numbers', () => {
  expect(add(2, 3)).toBe(5);
});

```

### Tools
* Jest
* Mocha
* Jasmine


## 68 Mocking

### Definition
**Mocking** is a testing technique where **real dependencies are replaced with simulated (mock) objects or functions** to control behavior and isolate the code under test.

Mocks allow developers to **test units independently** without relying on external systems like APIs, databases, or timers.

---

## Why Mocking Is Important
- Isolates the unit under test
- Makes tests fast and predictable
- Avoids dependency on external services
- Helps test edge cases and error scenarios

---

## What Can Be Mocked
- API calls
- Database queries
- Third-party libraries
- Timers (`setTimeout`, `setInterval`)
- Browser APIs (`fetch`, `localStorage`)

---

## Basic Example (Function Mocking)

### Real Function
```js
export function fetchUser() {
  return fetch('/api/user').then(res => res.json());
}
```





## 69 End-to-End (E2E) Testing

### Definition 
**End-to-End (E2E) Testing** verifies the **complete user journey** of an application by testing the system **from the user’s perspective**, including the **frontend, backend, APIs, and database**.

It ensures that the **entire application works together as expected** in real-world scenarios.

---

## Why E2E Testing Is Important
- Validates real user workflows
- Detects integration issues missed by unit tests
- Ensures application stability before release
- Builds confidence in production readiness

---

## What Is Tested in E2E Testing
- UI interactions (clicks, forms, navigation)
- API calls and responses
- Database read/write operations
- Authentication and authorization
- Error handling and edge cases

---

## Example (Login Flow – Cypress)

```js
describe('Login Flow', () => {
  it('should login the user successfully', () => {
    cy.visit('/login');

    cy.get('#email').type('test@mail.com');
    cy.get('#password').type('123456');
    cy.get('button[type="submit"]').click();

    cy.url().should('include', '/dashboard');
  });
});
```



## 70  Debugging JavaScript

### Definition

Debugging is the process of testing, finding, and reducing bugs (errors) in computer programs. It involves:
* Identifying errors (syntax, runtime, or logical errors).
* Using debugging tools to analyze code execution.
* Implementing fixes and verifying correctness.

Effective debugging improves **code quality, performance, and reliability**.

---

## Common Types of JavaScript Errors

### 1️⃣ Syntax Errors
Errors caused by incorrect syntax.

```js
if (x > 5 {
  console.log(x);
}

```
### 2️⃣ Runtime Errors
Errors that occur while the code is running.

```
console.log(user.name); // user is undefined
```

### 3️⃣ Logical Errors
Code runs without errors but produces incorrect results.
```
function isEven(n) {
  return n % 2 === 1; // wrong logic
}
```

### 4. Type Errors
This happens when a value is not of the expected type (e.g., trying to call a method on undefined).
```
let num = 5;
num.toUpperCase(); // TypeError: num.toUpperCase is not a function
```

## Tools & Techniques

### 1️⃣ Browser DevTools
Used to inspect code execution, errors, and performance.

**Common DevTools Tabs**
- **Console** → Errors and logs
- **Sources** → Debugging and breakpoints
- **Network** → API and request issues
- **Performance** → Slow code analysis
- **Memory** → Memory leaks

---

### 2️⃣ Breakpoints
Breakpoints pause execution at a specific line to inspect variables and flow.

```js
function calculate(a, b) {
  return a + b; // breakpoint here
}

```

### 3️⃣ debugger Keyword
Pauses execution programmatically at runtime.
```
function divide(a, b) {
  debugger;
  return a / b;
}

```

### 4️⃣ Source Maps
Source maps map minified or transpiled code back to the original source code, making production debugging easier.

```
{
  "devtool": "source-map"
}

```


## 71️ Code Coverage Tools

### Definition 
**Code Coverage** indicates **which parts of the source code are executed during automated tests**.  
It helps teams **identify untested code paths** and improve overall test reliability.

Code coverage is usually measured as a **percentage**.

---

## Coverage Types

### 1️⃣ Statement Coverage
Measures **how many executable statements (lines of code)** are run during testing.

```js
let x = 10;
console.log(x);
```

### 2️⃣ Branch Coverage

Measures whether all conditional paths (if, else, switch) have been executed.

```
function checkAge(age) {
  if (age >= 18) {
    return "Adult";
  } else {
    return "Minor";
  }
}
```

### 3️⃣ Function Coverage

Measures whether functions are invoked at least once during tests.

```
function greet() {
  return "Hello";
}
```

### Code Coverage Tools

* Istanbul (nyc)
The most widely used JavaScript code coverage tool.
Jest internally uses Istanbul for coverage reporting.

* Jest Coverage
Built-in coverage support in Jest, easy to configure and use.

* Codecov
A reporting and visualization platform for coverage results, commonly used in CI/CD pipelines.

### Jest Coverage Command
```
jest --coverage
```

## 72 Static Code Analysis

### Definition
**Static Code Analysis** is the process of **analyzing source code without executing it** to detect **bugs, code quality issues, and violations of coding standards**.

It helps identify problems **early in the development lifecycle**, before the code reaches runtime or production.

---

## Tools Used for Static Code Analysis

- **ESLint**  
  Identifies JavaScript code issues, enforces best practices, and maintains consistent coding standards.

- **TypeScript**  
  Performs static type checking to catch type-related errors at compile time.

- **SonarQube**  
  A comprehensive platform for detecting bugs, security vulnerabilities, and maintainability issues across multiple languages.

---

## ESLint Example

```js
if (a == b) {
  //  loose equality (not recommended)
}

if (a === b) {
  //  strict equality (recommended)
}
```

### Benefits of Static Code Analysis
✔ Early detection of bugs and issues
✔ Enforces consistent coding standards
✔ Improves code readability and maintainability
✔ Integrates with pre-commit hooks and pull request (PR) checks
✔ Reduces technical debt





## 73 Cross-Browser Testing

### Definition 
**Cross-Browser Testing** is the process of **verifying that a web application behaves consistently across different browsers, devices, and operating systems**.

It ensures a **uniform user experience** regardless of the platform used.

---

## Why Cross-Browser Testing Is Important
- Different browsers interpret code differently
- Prevents UI and functionality issues
- Ensures compatibility across devices
- Improves user experience and accessibility

---

## Tools for Cross-Browser Testing

- **BrowserStack**  
  Cloud-based platform for testing on real browsers and devices.

- **Sauce Labs**  
  Supports automated and manual cross-browser testing at scale.

- **LambdaTest**  
  Provides real-time and automated testing across multiple browsers and OS combinations.

---
 
 



🔐 BEST PRACTICES & SECURITY (76–90)


#76. JavaScript Best Practices

---

## 1️⃣ ESLint + Prettier

- **ESLint:** Tool to **identify and fix code issues**, enforce coding standards.  
- **Prettier:** Tool to **format code consistently** (spacing, semicolons, indentation).  
- **Why:** Maintains **readable, consistent, and error-free code** across teams.  

```bash
# Install ESLint
npm install eslint --save-dev

# Install Prettier
npm install prettier --save-dev

```
## 2️⃣ Meaningful Naming
* Always use descriptive names for variables, functions, classes.
* Use camelCase for variables and functions, PascalCase for classes.

```bash
// Bad
const a = 10;

// Good
const userAge = 10;
function calculateTotalPrice(items) { ... }
```

## 3️⃣ Error Handling
* Use try...catch to handle exceptions gracefully.
* Validate inputs and avoid runtime errors.

```
try {
  const data = JSON.parse(userInput);
} catch (error) {
  console.error("Invalid JSON!", error);
}
```
## 4️⃣ Performance Optimization
* Avoid unnecessary loops or DOM manipulations.
* Debounce/throttle events for scroll, resize, input.
* Use memoization to cache results of expensive calculations.
* Minimize global variables.
```
// Example: Debounce function
function debounce(func, delay) {
  let timer;
  return function(...args) {
    clearTimeout(timer);
    timer = setTimeout(() => func.apply(this, args), delay);
  };
}

```
## 5️⃣ Security Validation
* Validate user inputs to prevent XSS and injection attacks.
* Escape HTML content when inserting dynamically.
* Avoid storing sensitive info in localStorage.
* Use HTTPS and secure cookies when applicable.
```
const userInput = "<script>alert('XSS')</script>";
const safeInput = userInput.replace(/</g, "&lt;").replace(/>/g, "&gt;");
```

## 6️⃣ Unit Tests + Code Reviews
* Unit Tests: Test individual functions/modules to catch bugs early.
* Popular frameworks: Jest, Mocha, Chai.
* Code Reviews: Peer reviews help maintain quality, spot logical errors, and share knowledge.
```
// Example: Jest test
test('adds 2 + 3 to equal 5', () => {
  expect(add(2, 3)).toBe(5);
});

```

# 77. Handle Async Operations Effectively

JavaScript provides multiple ways to handle asynchronous operations efficiently, ensuring non-blocking execution, proper error handling, and optimized performance.

---

## Key Techniques

### 1️⃣ Async/Await

- Modern, readable syntax over Promises.
- Pauses function execution until the promise resolves.

```javascript
async function fetchData() {
  try {
    const data1 = await api1();
    const data2 = await api2();
    console.log(data1, data2);
  } catch (error) {
    console.error("Error:", error);
  }
}
```
### 2️⃣ Parallel Execution with Promise.all

* Run multiple async operations simultaneously instead of sequentially.
* Improves performance significantly.
```
async function fetchAll() {
  try {
    const [data1, data2] = await Promise.all([api1(), api2()]);
    console.log(data1, data2);
  } catch (error) {
    console.error("Error:", error);
  }
}
```

### 3️⃣ Try / Catch for Error Handling

Always wrap await in try/catch to catch errors in async functions.
```
async function fetchData() {
  try {
    const result = await apiCall();
  } catch (error) {
    console.error("API failed", error);
  }
}
```

### 4️⃣ AbortController for Cancellation
Useful to cancel ongoing fetch requests or other async tasks.

```
const controller = new AbortController();
const signal = controller.signal;
fetch("https://api.example.com/data", { signal })
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => {
    if (err.name === "AbortError") {
      console.log("Fetch aborted");
    }
  });

// Cancel request
controller.abort();
```

 