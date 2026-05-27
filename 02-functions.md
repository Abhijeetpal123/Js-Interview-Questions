# JavaScript Functions — Everything You Need to Know

Functions are the heart of JavaScript. If you understand functions deeply, you will write better code and perform well in interviews.

In this post, we will cover everything about functions — from the basics to the tricky concepts that interviewers love to ask about.

---

## Table of Contents

1. Function Declaration
2. Function Expression
3. First Class Functions
4. IIFE — Immediately Invoked Function Expression
5. Function Scope and Hoisting
6. Parameters and Arguments
7. The return Keyword
8. Anonymous Functions
9. Higher-Order Functions
10. Callback Functions
11. Spread and Rest Operators
12. Output-Based Interview Questions
13. Practice Questions

---

## 1. Function Declaration

A **function declaration** is a named function created using the `function` keyword.

```js
function greet() {
  console.log("Hello!");
}

greet(); // Hello!
```

**Key points:**
- Fully hoisted — you can call it before it is defined in the code
- Must have a name

```js
// This works because of hoisting
sayHello();

function sayHello() {
  console.log("Hello");
}
```

---

## 2. Function Expression

A **function expression** is when you store a function inside a variable.

```js
const greet = function() {
  console.log("Hello!");
};

greet(); // Hello!
```

**Key points:**
- Not fully hoisted — you cannot call it before the line where it is defined
- The variable is hoisted, but its value (the function) is not

```js
// This will throw an error
greet(); // TypeError: greet is not a function

var greet = function() {
  console.log("Hello");
};
```

### Function Declaration vs Function Expression

| | Function Declaration | Function Expression |
|---|---|---|
| **Syntax** | `function abc() {}` | `const abc = function() {}` |
| **Hoisted?** | Yes — fully hoisted | No — cannot call before definition |
| **Has a name?** | Always | Optional |

---

## 3. First Class Functions

In JavaScript, functions are treated as **first class citizens**. This means:

- You can store a function in a variable
- You can pass a function as an argument to another function
- You can return a function from another function

```js
// Store in a variable
const add = function(a, b) {
  return a + b;
};

// Pass as an argument
function runFunction(fn) {
  fn();
}

// Return from another function
function outer() {
  return function() {
    console.log("Inner function");
  };
}
```

This concept is the foundation of callback functions and higher-order functions.

---

## 4. IIFE — Immediately Invoked Function Expression

An **IIFE** (pronounced "iffy") is a function that runs immediately after it is defined. You do not need to call it separately.

```js
(function(num) {
  console.log(num * num);
})(5); // 25
```

### Why do we wrap IIFE inside parentheses?

Without the parentheses, JavaScript treats it as a function declaration and throws an error. The parentheses tell JavaScript — *"this is an expression, not a declaration"* — so it runs immediately.

### Why use IIFE?

- Creates a **private scope** — variables inside cannot be accessed outside
- Executes code immediately without polluting the global scope
- Avoids naming conflicts in large codebases

```js
(function() {
  var secret = "I am private";
  console.log(secret); // Works fine
})();

console.log(secret); // ReferenceError — not accessible outside
```

---

## 5. Function Scope and Hoisting

### Function Scope

Variables declared inside a function are **local** to that function. They cannot be accessed from outside.

```js
function greet() {
  var message = "Hello";
  console.log(message); // Hello
}

greet();
console.log(message); // ReferenceError — message is not defined
```

### Function Hoisting

JavaScript moves **function declarations** to the top before executing the code. This is called hoisting.

```js
// You can call this before it is defined
sayHi();

function sayHi() {
  console.log("Hi!");
}
```

> Only **function declarations** are fully hoisted. Function expressions are NOT.

---

## 6. Parameters and Arguments

**Parameters** are the names listed in the function definition.
**Arguments** are the actual values passed when calling the function.

```js
// 'a' and 'b' are parameters
function add(a, b) {
  return a + b;
}

// 5 and 3 are arguments
add(5, 3); // 8
```

### Rest Operator ( ...rest )

The **rest operator** collects multiple arguments into a single array.

```js
function sum(...numbers) {
  return numbers.reduce(function(total, num) {
    return total + num;
  }, 0);
}

console.log(sum(1, 2, 3, 4)); // 10
```

### Spread Operator ( ...spread )

The **spread operator** expands an array into individual elements.

```js
const nums = [1, 2, 3];
console.log(Math.max(...nums)); // 3
```

### Rest vs Spread — Quick Difference

| | Rest | Spread |
|---|---|---|
| **Where used** | In function parameters | When calling a function or in arrays |
| **What it does** | Collects values into an array | Expands an array into individual values |

---

## 7. The return Keyword

The `return` keyword does two things:
- Sends a value back to wherever the function was called
- Stops the function immediately — nothing after `return` runs

```js
function add(a, b) {
  return a + b;
  console.log("This never runs"); // unreachable code
}

const result = add(2, 3);
console.log(result); // 5
```

### What if a function has no return statement?

JavaScript automatically returns `undefined`.

```js
function greet() {
  console.log("Hello");
}

const result = greet(); // prints Hello
console.log(result);    // undefined
```

---

## 8. Anonymous Functions

An **anonymous function** is a function without a name. It is usually stored in a variable or passed directly as an argument.

```js
// Stored in a variable
const greet = function() {
  console.log("Hello!");
};

// Passed directly as an argument
setTimeout(function() {
  console.log("Runs after 1 second");
}, 1000);
```

---

## 9. Higher-Order Functions

A **higher-order function** is a function that either:
- Takes another function as an argument, OR
- Returns another function

`map()`, `filter()`, and `reduce()` are all higher-order functions.

```js
// Takes a function as argument
[1, 2, 3].map(function(num) { return num * 2; }); // [2, 4, 6]

// Returns a function
function multiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = multiplier(2);
console.log(double(5)); // 10
```

---

## 10. Callback Functions

A **callback function** is a function that is passed as an argument to another function and is called later — usually after some task is completed.

```js
function greetUser(name, callback) {
  console.log("Hello " + name);
  callback();
}

function afterGreet() {
  console.log("How are you?");
}

greetUser("Abhijeet", afterGreet);
// Hello Abhijeet
// How are you?
```

### Where are callbacks used?

- `setTimeout` — run code after a delay
- Event listeners — run code when a button is clicked
- API calls — run code after data is fetched

```js
// setTimeout example
setTimeout(function() {
  console.log("This runs after 1 second");
}, 1000);
```

### Why Are Callbacks Important in Async JavaScript?

JavaScript runs one line at a time. When it needs to wait for something — like fetching data from an API or a timer — it cannot just pause. 

Callbacks allow JavaScript to say: *"Go do this task, and when it is done, call this function."*

```js
console.log("Start");

setTimeout(function() {
  console.log("Inside setTimeout");
}, 1000);

console.log("End");

// Output:
// Start
// End
// Inside setTimeout  ← runs after 1 second
```

---

## Summary Table

| Concept | One Line Explanation |
|---|---|
| Function Declaration | Named function, fully hoisted |
| Function Expression | Function stored in a variable, not hoisted |
| First Class Function | Functions can be stored, passed, and returned |
| IIFE | Runs immediately, creates private scope |
| Anonymous Function | A function with no name |
| Higher-Order Function | Takes or returns another function |
| Callback Function | A function passed as argument, called later |
| Rest Operator | Collects arguments into an array |
| Spread Operator | Expands array into individual values |

---

## Output-Based Interview Questions

Try to guess the output before revealing the answer.

---

### Question 1 — Hoisting Trick

```js
sayHello();

function sayHello() {
  console.log("A");
}

var sayHello = function() {
  console.log("B");
};

sayHello();
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
A
B
```

The first call runs the function declaration (hoisted to top) → prints `"A"`.
Then `sayHello` is reassigned to a new function expression → second call prints `"B"`.

</details>

---

### Question 2 — return Stops Execution

```js
function demo() {
  return;
  console.log("Hello");
}

console.log(demo());
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
undefined
```

`return` with no value returns `undefined`. The `console.log("Hello")` never runs because `return` stops the function immediately. This is also an example of **ASI — Automatic Semicolon Insertion**, where JavaScript inserts a semicolon after `return`.

</details>

---

### Question 3 — Unreachable Code After return

```js
function abc() {
  console.log("A");
  return "B";
  console.log("C");
}

console.log(abc());
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
A
B
```

`console.log("A")` runs first. Then `return "B"` sends `"B"` back and stops the function. `console.log("C")` is unreachable and never runs. The outer `console.log` prints `"B"`.

</details>

---

### Question 4 — Function Returns a Function

```js
function x() {
  console.log("A");

  return function() {
    console.log("B");
  };
}

x()();
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
A
B
```

`x()` is called first → prints `"A"` and returns a function. The second `()` immediately calls that returned function → prints `"B"`.

</details>

---

### Question 5 — No Return Statement

```js
function test() {
  console.log("Hello");
}

console.log(test());
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
Hello
undefined
```

`test()` runs and prints `"Hello"`. But it has no `return` statement, so JavaScript automatically returns `undefined`. The outer `console.log` then prints `undefined`.

</details>

---

### Question 6 — IIFE Output

```js
(function(num) {
  console.log(num * num);
})(5);
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
25
```

The IIFE runs immediately with `5` as the argument. `5 * 5 = 25`.

</details>

---

## Practice Questions — Try These Yourself

No answers given — run these in your browser console and figure it out.

**1.** What is the output?
```js
var x = 10;

function test() {
  console.log(x);
  var x = 20;
  console.log(x);
}

test();
```

**2.** What is the output?
```js
function outer() {
  var count = 0;
  return function() {
    count++;
    console.log(count);
  };
}

const increment = outer();
increment();
increment();
increment();
```

**3.** Write a function `multiplyBy` that takes a number and returns a new function that multiplies any number by it.
```js
const triple = multiplyBy(3);
console.log(triple(5)); // 15
console.log(triple(10)); // 30
```

**4.** What is the output?
```js
console.log(typeof greet);

var greet = function() {
  return "Hello";
};

console.log(typeof greet);
```

**5.** Create an IIFE that takes your name as an argument and logs `"Hello, [name]!"` immediately.

---

These concepts come up in almost every JavaScript interview — especially callbacks, hoisting, and return behavior. Understand the *why* behind each one, not just the output.

Happy coding! 🚀

#wemakedevs #javascript #webdev #100DaysOfCode
