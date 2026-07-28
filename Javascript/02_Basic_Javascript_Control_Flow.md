#### Equality in Javascript

- `===` checks **value + type** → preferred
- `==` ignores type → can cause unexpected bugs
- Objects/arrays compare by **reference**, not content → use deep equality to compare contents

```js
// == vs ===
1 == "1"   // true  (type coercion)
1 === "1"  // false (different types)

// Objects by reference
const a = { x: 1 };
const b = { x: 1 };
a === b  // false (different instances)
a === a  // true
```

#### JS If Statements & Truthy/Falsy

- `if` requires condition in `()`; chain with `else if` / `else`
- JS has **7 falsy values** → everything else is truthy
  - `false`, `0`, `""`, `null`, `undefined`, `NaN`, `0n`
- Use `&&`, `||`, `!` to combine or negate conditions

```js
// Falsy check
if (!value) console.log("falsy!");

// Combining conditions
if (age >= 18 && hasID) console.log("entry allowed");
if (isAdmin || isMod) console.log("access granted");
```