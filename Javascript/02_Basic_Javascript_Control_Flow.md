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
```js
""  0  0n  NaN  undefined  null  false
```

- Use `&&`, `||`, `!` to combine or negate conditions

| Operator | Name | Description |
|----------|------|-------------|
| `&&` | AND | true only if **both** sides are true |
| `\|\|` | OR | true if **at least one** side is true |
| `!` | NOT | **flips** the boolean value |

```js
// Falsy check
if (!value) console.log("falsy!");

// Combining conditions
if (age >= 18 && hasID) console.log("entry allowed");
if (isAdmin || isMod) console.log("access granted");
```

#### JS For Loops

- **`for`** → classic loop, uses index + condition + increment; flexible but verbose
- **`for...of`** → cleaner iteration over **array values**
- **`for...in`** → iterates over **object keys**
- **`forEach`** → array method, runs a function on each element

```js
const fruits = ["apple", "banana", "cherry"];
const user = { name: "Sam", age: 25 };

// Traditional for
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]); // "apple", "banana", "cherry"
}

// for...of (array values)
for (const fruit of fruits) {
  console.log(fruit); // "apple", "banana", "cherry"
}

// for...in (object keys)
for (const key in user) {
  console.log(key, user[key]); // "name Sam", "age 25"
}

// forEach
fruits.forEach(fruit => {
  console.log(fruit); // "apple", "banana", "cherry"
});
```


#### JS While & Do While Loops

- **`while`** → checks condition **before** running; may never execute if condition starts false
- **`do...while`** → runs body **first**, then checks; always executes **at least once**
- Useful when the number of iterations is **unknown** in advance
- ⚠️ Always ensure the condition eventually becomes `false` → avoid infinite loops

```js
let count = 0;

// while - checks first
while (count < 3) {
  console.log(count); // 0, 1, 2
  count++;
}

// do...while - runs first, then checks
let tries = 0;
do {
  console.log("attempt:", tries); // runs at least once
  tries++;
} while (tries < 3);

// Condition starts false - while vs do...while
let x = 10;
while (x < 5) { console.log("never runs"); }
do { console.log("runs once"); } while (x < 5);
```

#### JS Error Handling

- **`try`** → wraps code that might fail
- **`catch`** → handles the error gracefully; receives the **error object**
- **`finally`** → always runs regardless of error; good for **cleanup**
- **`throw`** → manually trigger a custom error

```js
function divide(a, b) {
  if (b === 0) throw new Error("Cannot divide by zero");
  return a / b;
}

try {
  console.log(divide(10, 0));
} catch (err) {
  console.log("Error:", err.message); // "Cannot divide by zero"
} finally {
  console.log("always runs"); // cleanup goes here
}

example

try{
    throw("this is an error\n")
}catch (err){
    console.log(err, "this is the catch block error")
}finally{
    console.log("Hey i am still here")
}

output:
//this is an error
// this is the catch block error
//Hey i am still here
```

#### JS Switch Statement

- Cleaner alternative to multiple `if...else` blocks when checking **one variable** against many values
- **`default`** → fallback case, like `else`
- **`break`** → stops execution from falling into the next case ⚠️

```js
const day = "Monday";

switch (day) {
  case "Monday":
    console.log("Start of the week");
    break;
  case "Friday":
    console.log("End of the week");
    break;
  default:
    console.log("Midweek day");
}

// Without break → "fall-through" (usually a bug)
switch (day) {
  case "Monday":
    console.log("Monday");  // runs
  case "Friday":
    console.log("Friday");  // also runs unintentionally!
}
```