#### JS Classes

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
