#### JS Arrow Functions

- Shorthand for defining functions using `=>` instead of `function` keyword
- Must use `let`, `const`, or `var` → **not hoisted** (define before use)
- Avoid inside classes/objects → `this` won't work as expected there

##### Syntax shortcuts
- **1 argument** → drop parentheses
- **1 statement body** → drop `{}` and `return` (auto-returns)
- **Returning an object** → wrap in `()` to avoid syntax error

```js
// Regular vs arrow
const add = function(a, b) { return a + b; }
const add = (a, b) => a + b; // shorter

// No args → keep ()
const greet = () => "Hello!";

// One arg → drop ()
const double = x => x * 2;

// Multi-line → keep {}
const multiply = (a, b) => {
  const result = a * b;
  return result;
};

// Returning object → wrap in ()
const makePerson = (name) => ({ name: name });

// ✅ Great for array functions
const nums = [1, 2, 3];
nums.map(x => x * 2);        // [2, 4, 6]
nums.filter(x => x > 1);     // [2, 3]

// ⚠️ Avoid in objects/classes → this breaks
const obj = {
  name: "Sam",
  greet: () => console.log(this.name), // undefined
  greetCorrect() { console.log(this.name); } // "Sam" ✅
};
```