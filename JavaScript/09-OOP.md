```js
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  sayHello() {
    console.log(`Hello ${this.name}`);
  }
}
const u1 = new User('Ali', 24);
const u2 = new User('Sara', 33);
const u3 = new User('Amin', 12);

u1.sayHello();
u2.sayHello();
u3.sayHello();

// Inheritance ___________________________________________________

class Animal {
  constructor(name) {
    this.name = name;
  }

  eat() {
    console.log(`${this.name} is eating.`);
  }
}

class Dog extends Animal {}
class Cat extends Animal {}

const dog = new Dog('Rex');
dog.eat(); // Rex is eating.

// استفاده کند super() داشته باشد باید از constructor اگر کلاس فرزند هم

class Dog extends Animal {
  constructor(name, color) {
    super(name);

    this.color = color;
  }
}

const dog = new Dog('Rex', 'Black');

// Method Overriding
class Animal {
  speak() {
    console.log('Some sound');
  }
}

class Dog extends Animal {
  speak() {
    console.log('Woof!');
  }
}

const dog = new Dog();
dog.speak(); // Woof!

// اگر بخواهی هم متد والد اجرا شود و هم کد خودت
class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
  }

  showInfo() {
    console.log(`${this.name} - ${this.price}$`);
  }
}

class Laptop extends Product {
  constructor(name, price, ram) {
    super(name, price);

    this.ram = ram;
  }
}

class Phone extends Product {
  constructor(name, price, camera) {
    super(name, price);

    this.camera = camera;
  }
}

// Encapsulation _________________________________________________
class BankAccount {
  #balance;

  constructor(owner, balance) {
    this.owner = owner;
    this.#balance = balance;
  }

  deposit(amount) {
    this.#balance += amount;
  }

  withdraw(amount) {
    this.#balance -= amount;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount('Amin', 100);

console.log(account.#balance); // error فقط داخل همان کلاس قابل دسترسی است
account.deposit(50);
console.log(account.getBalance());

// Getter و Setter

class User {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    if (value.length < 3) {
      console.log('Name is too short');
      return;
    }

    this._name = value;
  }
}

const user = new User('Amin');

user.name = 'Al';
user.name = 'Mohammad';

console.log(user.name);

// Polymorphism _________________________________________________
class Animal {
  speak() {
    console.log('Some sound');
  }
}

class Dog extends Animal {
  speak() {
    console.log('Woof!');
  }
}

class Cat extends Animal {
  speak() {
    console.log('Meow!');
  }
}

class Duck extends Animal {
  speak() {
    console.log('Quack!');
  }
}

const animals = [new Dog(), new Cat(), new Duck()];

for (const animal of animals) {
  animal.speak();
  // Woof!
  // Meow!
  // Quack!
}

// Static ________________________________________________________________
// متعلق به خود کلاس هستند نه اشیایی که از آن ساخته می‌شوند

class MathHelper {
  static add(a, b) {
    return a + b;
  }
}

console.log(MathHelper.add(10, 20)); // 30

const math = new MathHelper();
math.add(10, 20); // error

//

class User {
  static count = 0;

  constructor(name) {
    this.name = name;
    User.count++;
  }
}

new User('Ali');
new User('Sara');
new User('Amin');

console.log(User.count); // 3

//

class User {
  static company = 'OpenAI';

  static showCompany() {
    console.log(this.company);
  }
}

User.showCompany(); // OpenAI

// Prototype _________________________________________________

class User {
  sayHello() {
    console.log('Hello');
  }
}
// می‌سازد؟ sayHello آیا جاوااسکریپت برای هر ابجکت یک کپی جدا از
const user1 = new User();
const user2 = new User();
/*
کلاس ذخیره می‌شوند Prototype متدها داخل

     User.prototype
  -------------------
       sayHello()
            ▲
            │
  ┌─────────┴─────────┐
  │                   │
 user1               user2
*/

class User {}

User.prototype.sayHi = function () {
  console.log('Hi!');
};

const user = new User();
user.sayHi();

// this _________________________________________________________________

class User {
  constructor(name) {
    this.name = name;
  }

  show() {
    console.log(this.name);
  }
}

const user = new User('Amin');
user.show(); // Amin

const fn = user.show;
fn(); // this === undefined

// ثابت برمی‌گرداند this تابع را اجرا نمی‌کند فقط یک نسخه جدید با
const fn = user.show.bind(user);
fn(); // Amin

//

function hello() {
  console.log(this.name);
}

const user = {
  name: 'Amin',
};

hello.call(user); // Amin

//

const user = {
  name: 'Amin',

  show: () => {
    console.log(this.name);
  },
};

user.show(); // undefined

// Object.assign(target, source) _________________________________________

const user = { name: 'Amin', age: 23 };

const data = { name: 'Ali' };

Object.assign(user, data);

console.log(user); // { name: 'Ali', age: 23 }

// جدید باشد property اگر

const user = {
  name: 'Amin',
  age: 23,
};

const attrs = {
  email: 'amin@gmail.com',
};

Object.assign(user, attrs); // { name: 'Amin', age: 23, email: 'amin@gmail.com' }

// چند تا پراپرتی

Object.assign(target, source1, source2, source3);

const user = {
  name: 'Amin',
};

const data1 = {
  name: 'Ali',
};

const data2 = {
  name: 'Reza',
};

Object.assign(user, data1, data2); // { name: 'Reza' }

// برای ساخت ابجکت جدید

const user = { name: 'Amin', age: 23 };

const updatedUser = Object.assign({}, user, { age: 24 });
```
