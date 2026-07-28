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