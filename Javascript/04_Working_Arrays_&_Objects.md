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

#### JS Built-in Array Functions

| Function                  | Mutates? | Description                                              |
| ------------------------- | -------- | -------------------------------------------------------- |
| `push(el)`                | ✅        | adds element to **end**                                  |
| `pop()`                   | ✅        | removes element from **end**                             |
| `splice(st, del, ...rem)` | ✅        | remove/insert at any index                               |
| `indexOf(el)`             | ❌        | returns index of element, `-1` if not found              |
| `find(fn)`                | ❌        | returns **first** element where `fn` returns true        |
| `filter(fn)`              | ❌        | returns **all** elements where `fn` returns true         |
| `map(fn)`                 | ❌        | transforms each element, returns new array               |
| `sort(fn)`                | ✅        | sorts array in place; pass compare `fn` for custom order |
| `reduce(fn, init)`        | ❌        | reduces array to a **single value** (e.g. sum)           |

```js
const nums = [1, 2, 3, 4, 5, 6];

nums.push(7);                         // [1,2,3,4,5,6,7]
nums.pop();                           // removes 7
nums.splice(1, 1, 99);                // replace index 1 → [1,99,3,4,5,6]  
nums.indexOf(4);                      // 3
nums.find(x => x > 4);               // 5 (first match)
nums.filter(x => x % 2 === 0);       // [2,4,6]
nums.map(x => x * 2);                // [2,4,6,8,10,12]
nums.sort((a, b) => b - a);          // [6,5,4,3,2,1] (descending)
nums.reduce((sum, x) => sum + x, 0); // 21
```

