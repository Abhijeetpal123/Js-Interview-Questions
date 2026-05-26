# Js-Interview-Questions
# Mastering map(), filter(), and reduce() in JavaScript

If you've been writing JavaScript for a while, you've probably heard of `map()`, `filter()`, and `reduce()`. These three array methods are the backbone of functional programming in JavaScript — and once you understand them, you'll write cleaner, more readable, and more expressive code.

Let's break them down one by one.

---

## What is map()?

`map()` is used to **transform each element** of an array and return a **new array** with the transformed values. The resulting array always has the **same length** as the original.

```js
const nums = [1, 2, 3];
const doubled = nums.map(num => num * 2);
console.log(doubled); // [2, 4, 6]
```

---

## What is filter()?

`filter()` is used to **select elements based on a condition**. It returns a new array containing only the elements that satisfy that condition. The resulting array may be **shorter** than the original.

```js
const nums = [1, 2, 3, 4];
const evens = nums.filter(num => num % 2 === 0);
console.log(evens); // [2, 4]
```

---

## What is reduce()?

`reduce()` iterates through an array and **reduces it to a single value** — such as a sum, a count, an object, or even another array.

```js
const nums = [1, 2, 3, 4];
const sum = nums.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 10
```

**Two key terms:**
- `acc` (accumulator) — stores the accumulated result from previous iterations
- `curr` (current value) — represents the element currently being processed

---

## map() vs filter() — What's the Difference?

| | `map()` | `filter()` |
|---|---|---|
| **Purpose** | Transform each element | Select elements based on a condition |
| **Return length** | Same as original | Equal to or shorter than original |
| **Use when** | You want to modify values | You want to exclude certain values |

---

## map() vs forEach() — When Should You Use Which?

Use **`map()`** when you want to transform data and return a new array. It's the functional programming way.

Use **`forEach()`** when you just want to iterate and perform side effects — like logging, updating a variable, or calling an API — and you don't need a return value.

```js
// Use map() — you need the result
const doubled = [1, 2, 3].map(n => n * 2); // [2, 4, 6]

// Use forEach() — you just want to log
[1, 2, 3].forEach(n => console.log(n));
```

---

## filter() vs find() — What's the Difference?

- `filter()` returns **all elements** that satisfy the condition → returns an **array**
- `find()` returns **only the first element** that satisfies the condition → returns a **single value** (or `undefined` if none found)

```js
const arr = [1, 2, 3, 4];
arr.filter(n => n > 2); // [3, 4]
arr.find(n => n > 2);   // 3
```

---

## Chaining map() and filter()

You can chain these methods together for powerful one-liners:

```js
const arr = [1, 2, 3, 4];
const result = arr
  .filter(num => num % 2 === 0)
  .map(num => num * 2);

console.log(result); // [4, 8]
```

First, `filter()` keeps only even numbers `[2, 4]`, then `map()` doubles each one.

---

## Can reduce() Replace map() and filter()?

Yes! Because `reduce()` is flexible enough to accumulate results into any shape — including a new array. You can replicate `map()` behavior like this:

```js
const nums = [1, 2, 3];
const result = nums.reduce((acc, curr) => {
  acc.push(curr * 2);
  return acc;
}, []);

console.log(result); // [2, 4, 6]
```

---

## What Happens Without an Initial Value in reduce()?

If you don't provide an initial value, `reduce()` uses the **first element** as the accumulator and starts iterating from the **second element**.

```js
const nums = [1, 2, 3, 4];
const sum = nums.reduce((acc, curr) => acc + curr);
console.log(sum); // 10
```

This works fine for simple sums, but always provide an initial value when working with objects or arrays to avoid unexpected behavior.

---

## What is filter(Boolean)?

Passing `Boolean` directly into `filter()` removes all **falsy values** from an array (like `0`, `""`, `null`, `undefined`, `false`, `NaN`).

```js
const arr = [0, 1, "", "hello", null];
const result = arr.filter(Boolean);
console.log(result); // [1, "hello"]
```

Only truthy values survive. It's a clean and idiomatic trick.

---

## Tricky Output Questions

### Q1 — What is the output?
```js
const arr = [1, 2, 3];
const result = arr.map(num => {
  if (num > 1) return num * 2;
});
console.log(result);
```

**Output:** `[undefined, 4, 6]`

When `num` is `1`, the `if` condition is false and there's no explicit `return`, so `map()` inserts `undefined` for that element.

---

### Q2 — What is the output?
```js
const arr = [1, 2, 3];
const result = arr.filter(Boolean);
console.log(result);
```

**Output:** `[1, 2, 3]`

All three values (`1`, `2`, `3`) are truthy, so none are removed.

---

## map() vs reduce() — Which is Better for Performance?

`reduce()` **can be more performant** because it iterates through the array only once. Chaining `map().filter()` iterates twice.

However, `map()` and `filter()` are generally **more readable and easier to understand**. For most use cases, prefer readability — only optimize if you're working with very large datasets.

---

## Does map() Modify the Original Array?

**No.** `map()` always returns a **new array** and leaves the original untouched. This is a core principle of functional programming — avoid mutating data.

```js
const original = [1, 2, 3];
const doubled = original.map(n => n * 2);

console.log(original); // [1, 2, 3] — unchanged
console.log(doubled);  // [2, 4, 6]
```

---

## Summary

| Method | Returns | Use when |
|---|---|---|
| `map()` | New array (same length) | Transforming values |
| `filter()` | New array (shorter or same) | Selecting values by condition |
| `reduce()` | Single value (any type) | Accumulating into one result |
| `find()` | Single element or `undefined` | Finding the first match |
| `forEach()` | `undefined` | Side effects, no return needed |

---

These three methods are tools you'll use every single day as a JavaScript developer. Master them and your code will be cleaner, shorter, and much more expressive.

Happy coding! 🚀
