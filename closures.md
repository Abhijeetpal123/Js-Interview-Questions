# JavaScript Closures — The Concept Every Interviewer Asks About

If there is one JavaScript concept that shows up in almost every interview, it is **closures**.

Most beginners find it confusing at first — but once you understand it, you will realize you have been using closures all along without knowing it.

Let's break it down step by step.

---

## Table of Contents

1. Lexical Environment
2. Scope Chain
3. What is a Closure?
4. Regular Function vs Closure
5. Can Closures Modify Outer Variables?
6. Nested Closures
7. IIFE and Closures
8. double()() Syntax — What Does It Mean?
9. Output-Based Interview Questions
10. Practice Questions

---

## 1. Lexical Environment

Before understanding closures, you need to understand **lexical environment**.

Every time a function runs, JavaScript creates a **lexical environment** for it. This contains:
- **Local memory** — variables defined inside that function
- **Reference to the parent's lexical environment** — where the function was written in the code

> "Lexical" simply means — *where the function is written*, not where it is called.

```js
function outer() {
  let x = 10; // outer's local memory

  function inner() {
    console.log(x); // inner can access outer's memory
  }

  inner();
}

outer(); // 10
```

`inner()` was **written inside** `outer()` — so its lexical parent is `outer()`. That is why it can access `x`.

---

## 2. Scope Chain

When JavaScript looks for a variable, it follows the **scope chain**:

1. First checks its own local scope
2. If not found, goes up to the parent's scope
3. Then the parent's parent — all the way up to the global scope
4. If still not found — `ReferenceError`

```
inner() → middle() → outer() → global scope
```

This chain of scopes is called the **scope chain**.

---

## 3. What is a Closure?

> A **closure** is a function that remembers the variables from its outer scope — even after the outer function has finished executing.

```js
function outer() {
  let count = 0;

  return function inner() {
    count++;
    console.log(count);
  };
}

const counter = outer(); // outer() is done here
counter(); // 1
counter(); // 2
counter(); // 3
// inner still remembers 'count' — that's closure!
```

**What is happening here?**

- `outer()` runs and returns `inner()`
- `outer()` is now finished — normally its variables would be gone
- But `inner()` still holds a **reference** to `count`
- So every time we call `counter()`, it remembers and updates `count`

This is closure — the inner function carries its outer scope with it.

---

## 4. Regular Function vs Closure

| | Regular Function | Closure |
|---|---|---|
| **Accesses** | Own variables + global variables | Own variables + outer function's variables + global variables |
| **After outer function ends** | Cannot access outer variables | Still remembers outer variables |
| **State** | No memory between calls | Maintains state between calls |

```js
// Regular function — no memory
function add(a, b) {
  return a + b;
}

// Closure — remembers outer variable
function makeAdder(a) {
  return function(b) {
    return a + b; // remembers 'a' from outer scope
  };
}

const add5 = makeAdder(5);
console.log(add5(3)); // 8
console.log(add5(10)); // 15
```

---

## 5. Can Closures Modify Outer Variables?

**Yes.** Closures do not just read outer variables — they can also **modify** them.

Also, closures store a **reference** to the variable, not a copy of its value. So if the variable changes, the closure sees the updated value.

```js
function outer() {
  let count = 0;

  return {
    increment: function() { count++; },
    getCount: function() { console.log(count); }
  };
}

const obj = outer();
obj.increment();
obj.increment();
obj.getCount(); // 2
```

Both `increment` and `getCount` share the **same closure** over `count`. When `increment` changes `count`, `getCount` sees the new value.

---

## 6. Nested Closures

Closures work at multiple levels. An inner function can access variables from all its parent scopes.

```js
function outer() {
  let x = 1;

  function middle() {
    let y = 2;

    function inner() {
      let z = 3;
      console.log(x + y + z); // accesses all three
    }

    inner();
  }

  middle();
}

outer(); // 6
```

**How JavaScript finds the variables:**
```
inner()  → has z = 3
  ↑ looks up
middle() → has y = 2
  ↑ looks up
outer()  → has x = 1
  ↑ looks up
global scope
```

---

## 7. IIFE and Closures

An **IIFE** (Immediately Invoked Function Expression) is closely related to closures because it creates its own private scope.

```js
const result = (function() {
  let secret = 42;
  return secret * 2;
})();

console.log(result); // 84
console.log(secret); // ReferenceError — not accessible outside
```

`secret` is trapped inside the IIFE — nothing outside can touch it. This is closure being used to create **data privacy**.

---

## 8. double()() Syntax — What Does It Mean?

You might see this pattern in interviews — `outer()()`. It looks confusing but it is simple.

```js
function outer() {
  return function inner() {
    console.log("Hello!");
  };
}

outer()(); // Hello!
```

**Breaking it down:**
- First `()` → calls `outer()` which returns the `inner` function
- Second `()` → immediately calls that returned function

It is the same as writing:

```js
const fn = outer(); // first ()
fn();               // second ()
```

`outer()()` is just a shortcut — two function calls in one line.

---

## Summary Table

| Concept | One Line Explanation |
|---|---|
| Lexical Environment | Local memory + reference to parent scope |
| Scope Chain | The path JavaScript follows to find a variable |
| Closure | Inner function remembers outer variables even after outer function ends |
| Reference vs Value | Closures store reference — they see the latest value, not a snapshot |
| Data Privacy | Use closures/IIFE to hide variables from outside |

---

## Output-Based Interview Questions

Try to guess the output before revealing the answer.

---

### Question 1 — Basic Closure

```js
function makeCounter() {
  let count = 0;
  return function() {
    count++;
    return count;
  };
}

const counter = makeCounter();
console.log(counter());
console.log(counter());
console.log(counter());
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
1
2
3
```

The inner function forms a closure over `count`. Every call to `counter()` remembers the previous value and increments it. `count` is never reset because the closure holds a reference to the same variable.

</details>

---

### Question 2 — Closure Stores Reference, Not Value

```js
function outer() {
  let x = 10;

  function inner() {
    console.log(x);
  }

  x = 20;
  inner();
}

outer();
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
20
```

Closure stores a **reference** to the variable `x`, not its value at the time the function was created. When `inner()` runs, it looks up the current value of `x`, which is `20` at that point — not `10`.

</details>

---

### Question 3 — Famous var + setTimeout Interview Question

```js
for (var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
3
3
3
```

`var` is **function scoped**, not block scoped. All three `setTimeout` callbacks share the **same reference** to `i`. By the time 1 second passes, the loop has already finished and `i` is `3`. So all three print `3`.

**Fix using `let`:**
```js
for (let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
// Output: 0, 1, 2 — because let creates a new scope for each iteration
```

</details>

---

### Question 4 — Two Separate Closures

```js
function greet() {
  let name = "John";
  return function() {
    console.log("Hello " + name);
  };
}

let sayHello = greet();
let sayHello2 = greet();

sayHello();
sayHello2();
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
Hello John
Hello John
```

Each call to `greet()` creates a **new separate closure** with its own `name` variable. `sayHello` and `sayHello2` are independent — they do not share the same closure.

</details>

---

### Question 5 — Nested Closure Output

```js
function outer() {
  let x = 1;

  function middle() {
    let y = 2;

    function inner() {
      let z = 3;
      console.log(x + y + z);
    }
    inner();
  }
  middle();
}

outer();
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
6
```

`inner()` accesses `z` (its own), `y` (from `middle`), and `x` (from `outer`) through the scope chain. `1 + 2 + 3 = 6`.

</details>

---

### Question 6 — Closure with Object

```js
function outer() {
  let count = 0;

  return {
    increment: function() { count++; },
    getCount: function() { console.log(count); }
  };
}

const obj = outer();
obj.increment();
obj.increment();
obj.getCount();
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
2
```

Both `increment` and `getCount` share the same closure over `count`. Closure can not only read but also **modify** outer variables.

</details>

---

### Question 7 — IIFE with Closure

```js
const result = (function() {
  let secret = 42;
  return secret * 2;
})();

console.log(result);
console.log(secret);
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
84
ReferenceError: secret is not defined
```

The IIFE runs immediately and returns `84`. But `secret` is trapped inside the IIFE's private scope — it cannot be accessed from outside.

</details>

---

### Question 8 — Multiplier Closure

```js
function makeMultiplier(x) {
  return function(y) {
    return x * y;
  };
}

const double = makeMultiplier(2);
const triple = makeMultiplier(3);

console.log(double(5));
console.log(triple(5));
console.log(double(10));
```

<details>
<summary>Click to see the answer</summary>

**Output:**
```
10
15
20
```

`double` closes over `x = 2` and `triple` closes over `x = 3`. They are completely independent closures. Each remembers their own value of `x`.

</details>

---

## Practice Questions — Try These Yourself

**1.** What will be the output?
```js
function outer() {
  var x = 10;
  return function() {
    console.log(x);
    var x = 20;
  };
}
outer()();
```

**2.** Fix this code so it prints `0, 1, 2` instead of `3, 3, 3`:
```js
for (var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
```

**3.** Create a function `makeGreeter` that takes a `greeting` and returns a function that takes a `name` and logs `"[greeting], [name]!"`.
```js
const sayHi = makeGreeter("Hi");
sayHi("Abhijeet"); // Hi, Abhijeet!
sayHi("Priya");    // Hi, Priya!
```

**4.** What is the output?
```js
let fns = [];

for (var i = 0; i < 3; i++) {
  fns.push(function() {
    return i;
  });
}

console.log(fns[0]());
console.log(fns[1]());
console.log(fns[2]());
```

**5.** Create a counter using closure that has three methods — `increment()`, `decrement()`, and `reset()`.

---

Closures are used everywhere in JavaScript — in event listeners, API calls, React hooks, and more. Once you understand this concept, a lot of JavaScript will start making sense.

Happy coding! 🚀

#wemakedevs #javascript #webdev #100DaysOfCode
