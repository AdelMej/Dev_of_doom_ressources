# JavaScript Classes – Cheat Sheet

## 1. Basic Class Syntax
```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
```

## 2. Creating an Instance
```js
const p = new Person("Alice", 30);
```

## 3. Instance Methods
```js
class Person {
  sayHello() {
    return `Hello, I'm ${this.name}`;
  }
}
```

## 4. Class Methods (Static)
```js
class MathUtils {
  static add(a, b) {
    return a + b;
  }
}
```

## 5. Getters & Setters
```js
class Car {
  constructor(speed) {
    this._speed = speed;
  }

  get speed() {
    return this._speed;
  }

  set speed(value) {
    if (value >= 0) this._speed = value;
  }
}
```

## 6. Inheritance
```js
class Animal {
  speak() {
    return "Animal sound!";
  }
}

class Dog extends Animal {
  speak() {
    return "Woof!";
  }
}
```

## 7. Calling Parent Methods (super)
```js
class Parent {
  greet() {
    return "Hello";
  }
}

class Child extends Parent {
  greet() {
    return super.greet() + ", child here!";
  }
}
```

## 8. Private Fields (#)
```js
class BankAccount {
  #balance = 0;
  deposit(amount) {
    this.#balance += amount;
  }
}
```

## 9. Class Fields
```js
class User {
  role = "guest";      // instance field
  static maxUsers = 100; // static field
}
```

## 10. Using Classes as Blueprints
```js
class Product {
  constructor(name, price) {
    Object.assign(this, { name, price });
  }
}
```

## Summary
- constructor() initializes data
- static methods belong to the class itself
- extends enables inheritance
- super calls parent methods
- # makes fields private
- Classes use prototypes under the hood
