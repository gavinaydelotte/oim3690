# JavaScript Study Guide

---

## 1. Variables

Variables store data so you can use it later. There are three ways to declare them.

| Keyword | Reassignable | Scope | When to use |
|---|---|---|---|
| `var` | yes | function | avoid — legacy, has quirks |
| `let` | yes | block `{}` | most variables |
| `const` | no | block `{}` | values that won't change |

```js
let score = 0;         // can be reassigned later
const name = "Gavin";  // cannot be reassigned
score = 10;            // OK
name = "Other";        // ERROR — const cannot be reassigned
```

> **Rule of thumb:** Default to `const`. Use `let` only when you know the value will change. Avoid `var`.

---

## 2. Data Types

| Type | Example | Notes |
|---|---|---|
| `number` | `42`, `3.14` | integers and decimals |
| `string` | `"hello"`, `'world'` | text, wrapped in quotes |
| `boolean` | `true`, `false` | only two possible values |
| `undefined` | `let x;` | declared but not assigned |
| `null` | `let x = null;` | intentionally empty |
| `array` | `[1, 2, 3]` | ordered list of values |
| `object` | `{ name: "Gavin" }` | key-value pairs |

```js
typeof 42          // "number"
typeof "hello"     // "string"
typeof true        // "boolean"
typeof undefined   // "undefined"
typeof null        // "object" — known JS quirk, not actually an object
typeof [1, 2, 3]   // "object" — arrays are objects in JS
```

---

## 3. Operators

### Arithmetic Operators

```js
5 + 3   // 8   addition
5 - 3   // 2   subtraction
5 * 3   // 15  multiplication
5 / 2   // 2.5 division
5 % 2   // 1   modulo (remainder)
5 ** 2  // 25  exponentiation
```

### Comparison Operators

These return `true` or `false`.

| Operator | Meaning | Example | Result |
|---|---|---|---|
| `===` | strict equal (value AND type) | `5 === "5"` | `false` |
| `==` | loose equal (value only, type coerced) | `5 == "5"` | `true` |
| `!==` | strict not equal | `5 !== "5"` | `true` |
| `>` | greater than | `5 > 3` | `true` |
| `<` | less than | `5 < 3` | `false` |
| `>=` | greater than or equal | `5 >= 5` | `true` |
| `<=` | less than or equal | `4 <= 5` | `true` |

> **Always prefer `===` over `==`** — loose equality causes unexpected bugs.

### Logical Operators

| Operator | Meaning | Example | Result |
|---|---|---|---|
| `&&` | AND — both must be true | `true && false` | `false` |
| `\|\|` | OR — at least one must be true | `true \|\| false` | `true` |
| `!` | NOT — flips the boolean | `!true` | `false` |

```js
const age = 20;
const hasID = true;

if (age >= 21 && hasID) {
  console.log("Welcome");
}

if (age < 18 || !hasID) {
  console.log("Cannot enter");
}
```

---

## 4. Conditionals

### `if` / `else if` / `else`

```js
if (condition) {
  // runs if condition is true
} else if (otherCondition) {
  // runs if first is false but this is true
} else {
  // runs if all conditions are false
}
```

**Grade calculator example:**
```js
function getGrade(score) {
  if (score >= 90) {
    return "A";
  } else if (score >= 80) {
    return "B";
  } else if (score >= 70) {
    return "C";
  } else if (score >= 60) {
    return "D";
  } else {
    return "F";
  }
}

getGrade(85); // "B"
getGrade(72); // "C"
```

### Truthy and Falsy

JavaScript evaluates non-boolean values in conditionals automatically.

**Falsy values** (treated as `false`):
```js
false, 0, "", null, undefined, NaN
```

**Truthy values** (everything else, including):
```js
true, 1, "hello", [], {}, -1
```

```js
let username = "";
if (username) {
  console.log("Hello, " + username); // skipped — empty string is falsy
} else {
  console.log("No username provided"); // this runs
}
```

---

## 5. Functions

Functions are reusable blocks of code.

```js
// Declaration
function greet(name) {
  return "Hello, " + name;
}

// Call
greet("Gavin"); // "Hello, Gavin"
```

**Functions with conditionals:**
```js
function isEven(num) {
  if (num % 2 === 0) {
    return true;
  } else {
    return false;
  }
}

// Shorter version:
function isEven(num) {
  return num % 2 === 0;
}
```

> **`return` exits the function immediately.** Any code after a `return` in the same block will not run.

---

## 6. Arrays

Arrays store an ordered list of values. Index starts at **0**.

```js
const fruits = ["apple", "banana", "cherry"];
//               index 0    index 1    index 2

fruits[0]  // "apple"
fruits[2]  // "cherry"
fruits[3]  // undefined — out of bounds
```

### Common Array Properties & Methods

| Method / Property | What it does | Example |
|---|---|---|
| `.length` | number of items | `fruits.length` → `3` |
| `.push(val)` | add to the **end** | `fruits.push("mango")` |
| `.pop()` | remove from the **end** | `fruits.pop()` → `"cherry"` |
| `.unshift(val)` | add to the **beginning** | `fruits.unshift("kiwi")` |
| `.shift()` | remove from the **beginning** | `fruits.shift()` → `"apple"` |
| `.indexOf(val)` | find index of value (-1 if not found) | `fruits.indexOf("banana")` → `1` |
| `.includes(val)` | check if value exists | `fruits.includes("apple")` → `true` |
| `.join(sep)` | combine into a string | `fruits.join(", ")` → `"apple, banana, cherry"` |

```js
const scores = [88, 92, 75, 100];

scores.push(85);         // [88, 92, 75, 100, 85]
scores.pop();            // removes 85 → [88, 92, 75, 100]
scores.length;           // 4
scores.indexOf(75);      // 2
scores.includes(99);     // false
```

### Editing Array Values

You can directly reassign any index:

```js
const nums = [10, 20, 30];
nums[1] = 99;
// nums is now [10, 99, 30]
```

---

## 7. Loops

Loops repeat a block of code multiple times.

### `for` Loop

Best when you know how many times to repeat.

```js
for (let i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
}
```

**Anatomy:**
```
for ( initialization ; condition ; update ) { ... }
      let i = 0       i < 5       i++
```

- **Initialization** — runs once at the start (`let i = 0`)
- **Condition** — checked before every iteration; loop stops when false (`i < 5`)
- **Update** — runs after each iteration (`i++`)

### Looping Through an Array

```js
const fruits = ["apple", "banana", "cherry"];

for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}
// "apple"
// "banana"
// "cherry"
```

### `while` Loop

Best when you don't know in advance how many times to repeat.

```js
let count = 0;
while (count < 5) {
  console.log(count); // 0, 1, 2, 3, 4
  count++;
}
```

> **Infinite loop warning:** If the condition never becomes false, the loop runs forever. Always make sure the variable inside the condition changes inside the loop.

### `for...of` Loop

A cleaner way to loop through arrays when you don't need the index.

```js
const fruits = ["apple", "banana", "cherry"];

for (const fruit of fruits) {
  console.log(fruit);
}
// "apple"
// "banana"
// "cherry"
```

---

## 8. Loops + Arrays Together

This is where things get powerful — loops let you process every item in an array.

**Sum all values:**
```js
const scores = [80, 90, 75, 95];
let total = 0;

for (let i = 0; i < scores.length; i++) {
  total += scores[i];
}

console.log(total); // 340
```

**Find the highest value:**
```js
const scores = [80, 90, 75, 95];
let max = scores[0];

for (let i = 1; i < scores.length; i++) {
  if (scores[i] > max) {
    max = scores[i];
  }
}

console.log(max); // 95
```

**Build a new array from an existing one:**
```js
const prices = [10, 20, 30];
const discounted = [];

for (let i = 0; i < prices.length; i++) {
  discounted.push(prices[i] * 0.9);
}

console.log(discounted); // [9, 18, 27]
```

---

## Quick Reference Cheat Sheet

```js
// Variables
const x = 10;
let y = 5;

// Conditional
if (x > y) {
  console.log("x is bigger");
} else {
  console.log("y is bigger");
}

// Array
const items = ["a", "b", "c"];
items.push("d");         // add to end
items.pop();             // remove from end
items.length;            // 3

// for loop through array
for (let i = 0; i < items.length; i++) {
  console.log(items[i]);
}

// while loop
let i = 0;
while (i < 3) {
  console.log(i);
  i++;
}

// Function with conditional
function classify(n) {
  if (n > 0) return "positive";
  else if (n < 0) return "negative";
  else return "zero";
}
```
