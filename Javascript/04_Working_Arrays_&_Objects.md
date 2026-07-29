#### JS Built-in Object Functions

- **`Object.keys(obj)`** → returns array of **keys**
- **`Object.values(obj)`** → returns array of **values**
- **`Object.entries(obj)`** → returns array of **[key, value]** pairs
- **`Object.assign(target, ...sources)`** → copies properties onto target ⚠️ mutates target
  - Later sources **override** earlier ones on conflicts
  - Pass `{}` as target to **copy** an object without reference

```js
const person = { name: "Sam", age: 25 };

Object.keys(person);    // ["name", "age"]
Object.values(person);  // ["Sam", 25]
Object.entries(person); // [["name", "Sam"], ["age", 25]]

// Merge objects
const job = { role: "Dev", age: 30 };
Object.assign(person, job);
console.log(person); // { name: "Sam", age: 30, role: "Dev" } → age overridden

// Copy object (no reference)
const copy = Object.assign({}, person);
copy.name = "Jane";
console.log(person.name); // "Sam" → original unchanged
```