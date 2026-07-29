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

#### JS Spread Operator (`...`)

- **Combine objects** → later keys override earlier ones on conflicts
- **Combine arrays** → no overriding, just merges elements
- **Rest params** → collect remaining function args into an array
- **Spread into function** → pass array elements as individual arguments

```js
const a = { x: 1, z: 99 };
const b = { y: 2, z: 42 };

// Combine objects → b's z overrides a's z
const merged = { ...a, ...b, w: 3 }; // { x:1, z:42, y:2, w:3 }

// Combine arrays
const arr1 = [1, 2];
const arr2 = [3, 4];
const combined = [...arr1, 99, ...arr2]; // [1, 2, 99, 3, 4]

// Rest params → collect extra args as array
const fn = (first, ...rest) => console.log(first, rest);
fn(1, 2, 3, 4); // first=1, rest=[2,3,4]

// Spread into function → pass array as individual args
const add = (a, b, c) => a + b + c;
const nums = [1, 2, 3];
add(...nums); // 6
```

#### JS Destructuring

##### Object Destructuring
- Extract object properties into variables cleanly
- Supports **defaults**, **renaming**, and **nested** properties

```js
const person = { name: "Sam", age: 25, body: { height: 180, weight: 75 } };

// Basic
const { name, age } = person;

// Default value
const { eyeColor = "unknown" } = person; // "unknown" if missing

// Rename variable
const { name: fullName } = person; // fullName = "Sam"

// Rename + default
const { name: n = "stranger" } = person;

// Nested
const { body: { height, weight } } = person; // height=180, weight=75
```

##### Array Destructuring
- Assigns elements by **position**

```js
const nums = [1, 2, 3];
const [x, y, z] = nums; // x=1, y=2, z=3

// Skip elements
const [first, , third] = nums; // first=1, third=3
```


#### JS Template Literals

- Use backticks ` `` ` to define strings with embedded expressions
- Insert any JS expression using `${}`
- Cleaner alternative to `+` concatenation

```js
const name = "Sam";
const age = 25;

// Old way
"Hi, I'm " + name + " and I'm " + age;

// Template literal
`Hi, I'm ${name} and I'm ${age}`;

// Any expression works inside ${}
`2 + 2 = ${2 + 2}`;           // "2 + 2 = 4"
`Adult: ${age >= 18 ? "yes" : "no"}`; // "Adult: yes"
```


#### JS Import & Export (ES6)

##### Exporting
```js
// Default export (one per file)
export default myFunction;

// Named exports (multiple per file)
export const myString = "hello";
export function myFunc() {}
```

##### Importing
```js
// Default import
import myFunc from "./myFile";

// Named imports
import { myString, myFunc } from "./myFile";

// Import everything (namespace)
import * as utils from "./myFile";
utils.myFunc();
utils.myString;

// Import default + named together
import myFunc, { myString } from "./myFile";
```

##### Key Notes
- Named imports must use the **same name** as the export
- Import namespace (`* as`) useful for files with many exports
- Works with **npm packages** too → use package name instead of file path
```js
import _ from "lodash"; // npm package
```