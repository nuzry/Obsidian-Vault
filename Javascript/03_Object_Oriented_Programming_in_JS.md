[[Javascript]]
#### [[JS Classes]]

- `class` → defines a class; `constructor` → sets up properties; `new` → creates an instance
- No true private variables yet → prefix with `_` by convention (e.g. `_password`)
- `static` methods belong to the **class itself**, not instances → no `this`
- `extends` → subclassing; `super()` → calls parent constructor
- `instanceof` → checks if an object is an instance of a class (includes parent classes)

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this._age = age;        // "_" = treat as private
  }
  greet() {
    console.log(`Hi, I'm ${this.name}`);
  }
  static create(name, age) {  // static method
    return new Person(name, age);
  }
}

class Employee extends Person {
  constructor(name, age, salary) {
    super(name, age);         // calls Person's constructor
    this.salary = salary;
  }
  greet() {
    console.log(`${super.greet()}, I earn ${this.salary}`); // override + extend
  }
}

const emp = new Employee("Sam", 25, 50000);
emp.greet();
console.log(emp instanceof Employee); // true
console.log(emp instanceof Person);   // true
```

#### [[JS Prototype-Based Inheritance

- JS **doesn't have real classes** → `class` syntax is just syntactic sugar for prototypes
- Instead of classes & instances → JS uses **prototypes & children**
- `Object.create(proto)` → creates a child object from a prototype
- Changes to the prototype **reflect on children** (unless overridden)
- **Own properties** → set directly on the object (show in console)
- **Inherited properties** → come from prototype (hidden in console, but accessible)

### Prototype Chain
When accessing a property, JS looks:
1. Object itself → 2. Its prototype → 3. Prototype's prototype → ... → 4. Base `Object` prototype → `null`

```js
const person = {
  name: "John",
  greet() { console.log(`Hi, I'm ${this.name}`); }
};

const employee = Object.create(person); // employee's prototype = person
employee.greet();        // "Hi, I'm John" (inherited)
console.log(employee);  // {} (own properties only)

// Override inherited property
employee.name = "Sam";
employee.greet();        // "Hi, I'm Sam"

// Delete override → falls back to prototype
delete employee.name;
employee.greet();        // "Hi, I'm John" (back to prototype)

// Check prototype
Object.getPrototypeOf(employee) === person; // true
```