#### JS Arrow Functions

- Shorthand for defining functions using `=>` instead of `function` keyword
- Must use `let`, `const`, or `var` → **not hoisted** (define before use)
- Avoid inside classes/objects → `this` won't work as expected there

#### Syntax shortcuts
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

#### JS Default Function Arguments

- Set defaults with `=` in the argument list
- Default only triggers when argument is **`undefined`** → not for `null`, `0`, `false`

```js
const greet = (name = "stranger", age = 0, active = true) => ({ name, age, active });

greet();                        // { name: "stranger", age: 0, active: true }
greet("Sam");                   // { name: "Sam", age: 0, active: true }
greet("Sam", 25, false);        // { name: "Sam", age: 25, active: false }

// ⚠️ null/false/0 do NOT trigger defaults
greet(null, 0, false);          // { name: null, age: 0, active: false }
```

