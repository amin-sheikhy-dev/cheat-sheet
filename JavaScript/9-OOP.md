```js
// توضیح داده شده Object Enhanced خط 960 درباره
// استفاده کرد constructor fn ها نمیشه به عنوان arrow fn از

// استفاده می‌کردند Constructor Fn ها برای ساخت ابجکت های مشابه از class در جاوااسکریپت قبل از معرفی
const Person = function (firstName, birthYear) {
  this; // Person {}

  this.firstName = firstName;
  this.birthYear = birthYear;

  // فرض کن 1000 تا یوزر داری
  // جداگانه و جدید در حافظه ساخته می‌شه calcAge برای هر شخص یک تابع
  // یعنی 1000 تابع کاملاً یکسان اما هر کدام در جای متفاوتی از حافظه قرار دارن و اصلا بهینه نیست
  // this.calcAge = function () {
  //   return 1404 - this.birthYear
  // }
};

// را به آن اشاره می‌دهد this استفاده می‌کنیم جاوااسکریپت خودش یک ابچکت خالی می‌سازد و new وقتی
const amin = new Person('amin', 1384);
const soheil = new Person('soheil', 1370);

// Prototypes_________________________________________
Person.prototype.calcAge = function () {
  return 1404 - this.birthYear;
};

Person.prototype.city = 'Qom';

amin.__proto__; // {city: 'Qom', calcAge: ƒ}
amin.__proto__ === Person.prototype; // true

// ساخته شده است یا نه؟ Person از روی تابع سازنده amin آیا ابچکت
amin instanceof Person; // true

// ES6 Classes intro_________________________________________
// نمیشن یعنی نمیتونی قبل از تعریف کردنشون ازشون استفاده کنی hoist ها class

// class expression
const PersonCl2 = class {};

// class declaration
class PersonCl {
  constructor(fullName, birthYear) {
    this.fullName = fullName;
    this.birthYear = birthYear;
  }

  // instance Method
  // اضافه میشن prototype این متد هایی که خارج تابع مینویسیم به
  calcAge() {
    return 1404 - this.birthYear;
  }

  greet() {
    return `Hey ${this.fullName}`;
  }

  // Set & Get
  get age() {
    return 1404 - this.birthYear;
  }

  set fullName(name) {
    if (name.includes(' ')) this._fullName = name;
    else console.log('Please enter full name ❌');
  }

  // Static Method
  static hey() {
    return `Hi ${this.fullName}`;
  }
}

const hossein = new PersonCl('Hossein Mohammadi', 1378);

hossein.__proto__ === PersonCl.prototype; // true

// Setters , Getters_________________________________________
// استفاده میشن نه مثل تابع property مثل Setter و Getter

// به شکل متد property برای گرفتن مقدار یک Getter
// property برای تنظیم یا اعتبارسنجی مقدار یک Setter

const account = {
  owner: 'amin',

  movements: [-130, 70, 1300, 200, 10],

  get latestMovement() {
    return this.movements.at(-1);
  },

  set latestMovement(mov) {
    this.movements.push(mov);
  },
};

account.latestMovement; // 10

account.latestMovement = 0;
account.movements; // [-130, 70, 1300, 200, 10, 0]

// Coding Challenge_________________________________________
class CarCl {
  constructor(car, speed) {
    this.car = car;
    this.speed = speed;
  }

  speedAdd() {
    this.speed += 10;
    return `${this.car} is going at ${this.speed} km/h`;
  }

  speedLoss() {
    this.speed -= 5;
    return `${this.car} is going at ${this.speed} km/h`;
  }

  get speedNow() {
    return this.speed;
  }

  set speedUp(speed) {
    return (this.speed *= speed);
  }
}

const BMW = new CarCl('BMW', 30);
BMW.speedAdd(); // BMW is going at 40 km/h
BMW.speedAdd(); // BMW is going at 50 km/h

BMW.speedLoss(); // BMW is going at 45 km/h
BMW.speedLoss(); // BMW is going at 40 km/h
BMW.speedLoss(); // BMW is going at 35 km/h

BMW.speedNow; // 35

BMW.speedUp = 1.5;
BMW.speedNow; // 52.5

// Object.create_________________________________________
// مشخص prototype یه متد برای ساختن آبجکت جدید با
const PersonProto = {
  calcAge() {
    return 1404 - this.birthYear;
  },

  init(fullName, birthYear) {
    this.fullName = fullName;
    this.birthYear = birthYear;
  },
};

const abolfazl = Object.create(PersonProto);
abolfazl.name = 'abolfazl';
abolfazl.birthYear = 1353;

abolfazl; // {name: 'abolfazl', birthYear: 1353}

abolfazl.__proto__ === PersonProto; // true

const hamed = Object.create(PersonProto);
hamed.init('hamed', 1373);

hamed; // {fullName: 'hamed', birthYear: 1373}

// Inheritance Constructor Function_________________________________________

const Student = function (firstName, birthYear, course) {
  // this.firstName = firstName
  // this.birthYear = birthYear
  // تنظیم شه Student اش روی this استفاده میکنیم تا call و متد Constructor Function Person بجای کدای بالا از
  Person.call(this, firstName, birthYear);

  this.course = course;
};

Student.prototype = Object.create(Person.prototype);

Student.prototype.information = function () {
  return `name: '${this.firstName}' |  birthYear: '${this.birthYear}' | course: '${this.course}'`;
};

const steve = new Student('Steve', 2001, 'Computer Science');
steve; // Student {firstName: 'Steve', birthYear: 2001, course: 'Computer Science'}

steve.information(); // name: 'Steve' |  birthYear: '2001' | course: 'Computer Science'

steve.__proto__; // Person {information: ƒ}
Student.prototype; // Person {information: ƒ}
Person.prototype; // {city: 'Qom', calcAge: ƒ}

// Inheritance ES6 Classes_________________________________________

class Person2 {
  constructor(fullName, birthYear) {
    this.fullName = fullName;
    this.birthYear = birthYear;
  }

  calcAge() {
    return 1404 - this.birthYear;
  }

  greet() {
    return `Hey ${this.fullName}`;
  }

  get age() {
    return 1404 - this.birthYear;
  }
}

// به ارث ببره Person class از Student class کاری میکنیم که
// کلاس چایلد متدهای پرنت رو داره
class Student2 extends Person2 {
  constructor(firstName, birthYear, course) {
    // همیشه باید اول باشه
    // constructor یعنی فراخانی در کلاس پرنت super کال کردن در
    super(firstName, birthYear);

    // استفاده میکردیم this و call نبود باید از متد super و extends اگر
    // Person.call(this, firstName, birthYear)

    this.course = course;
  }
}

const rick = new Student2('rick', 2001, 'Biology'); // Student2 {fullName: 'rick', birthYear: 2001, course: 'Biology'}

const daniel = new Student2('daniel', 2008, 'Electrical Engineering'); // Student2 {fullName: 'daniel', birthYear: 2008, course: 'Electrical Engineering'}

// Inheritance Classes with Object.create_________________________________________

const PersonProto2 = {
  calcAge() {
    return 1404 - this.birthYear;
  },

  init(fullName, birthYear) {
    this.fullName = fullName;
    this.birthYear = birthYear;
  },
};

const StudentProto = Object.create(PersonProto2);

StudentProto.init = function (fullName, birthYear, course) {
  PersonProto2.init.call(this, fullName, birthYear);
  this.course = course;
};

StudentProto.introduce = function () {
  return `my name is ${this.fullName} , i study ${this.course} and ${this.calcAge()} years old `;
};

// PersonProto2 فقط خود کلاس
const steven = Object.create(PersonProto2);
steven.init('Steven Jobs', 1370);

steven; // {fullName: 'Steven Jobs', birthYear: 1370}

// Person فقط متدهای
steven.calcAge();

// PersonProto2 و ارث بری شده از StudentProto با استفاده از
const jay = Object.create(StudentProto);
jay.init('Jay Z', 1375, 'Computer Science');

jay; // {fullName: 'Jay Z', birthYear: 1375, course: 'Computer Science'}

// PersonProto2 متد ارث‌ بری‌ شده از
jay.calcAge();

// Student متد خود
jay.introduce(); // my name is Jay Z , i study Computer Science and 29 years old

// Encapsulation with public, protected, private fields/methods_________________________________________
// به معنای مخفی کردن جزئیات داخلی یک کلس و فراهم کردن یک رابط عمومی برای تعامل با آن ها است Encapsulation
// ها نگر داریم class در private هارو به صورت method و property یعنی یه سری Encapsulation
// قابل دسترسی نیستن class که خارج از

class Account {
  // گفته میشود field قرار گرفتن constructor به اینایی که بیرون تابع

  // Public fields
  region = 'asia';
  country = 'iran';
  currency = 'Rl';

  // Private fields
  // با # تعریف می‌شوند و فقط داخل کلاس قابل دسترسی هستند
  #movements = [];
  #pin;

  constructor(owner, pin) {
    this.owner = owner;
    this.#pin = pin;

    // Protected Properties
    // برنامه‌نویس‌ها از _ استفاده می‌کنن تا بگن این فیلد یا متد برای استفاده داخل کلس است و این فقط یه قرارداد است و به طور رسمی در جاوااسکریپت وجود ندارد
    // this._movements = []
  }

  // Public methods
  // ذخیره میشوند چون خارج از کلس هستند prototype تمام متد ها در
  getPin() {
    return this.#pin;
  }

  getMovements() {
    return this.#movements;
  }

  // Method Chaining
  _________________________________________; // برگردانند this شوند ولی متدهایی که عمل انجام می‌دهند باید chain متدهایی که اطلاعات برمی‌گردانند نباید
  deposit(val) {
    this.#movements.push(val);
    return this;
  }

  withdraw(val) {
    this.deposit(-val);
    return this;
  }

  // استفاده میکنیم get برای اینکه مجبور به کال کردن تابع نباشیم از
  get accountSummary() {
    const movements = [...this.#movements]; // کپی از آرایه برای امن‌تر بودن
    return movements.reduce((acc, val) => acc + val, 0);
  }
}

const acc1 = new Account('amin', 1111);
acc1.deposit(840);
acc1.withdraw(190);

acc1.deposit(2500).withdraw(1200).withdraw(30).deposit(120);

acc1.__proto__; // {getPin: ƒ, getMovements: ƒ, deposit: ƒ, withdraw: ƒ}

// static method_________________________________________
// متد استاتیک به خود کلاس تعلق دارد نه به اشیایی که از کلاس ساخته می‌شوند
class Product {
  constructor(title) {
    this.title = title;
  }

  showTitle() {
    return this.title;
  }

  static hello() {
    return 'Hello';
  }
}

const p1 = new Product('Laptop');
p1.showTitle(); // Laptop

// اما روی نمونه قابل دسترسی نیست
Product.hello(); // Hello
// p1.hello() // Error

// همان‌طور که متد می‌تواند استاتیک باشد ویژگی هم می‌تواند استاتیک باشد
class Product2 {
  static count = 0;

  constructor(title) {
    this.title = title;
    Product2.count++;
  }
}

new Product2('A');
new Product2('B');
new Product2('C');

Product2.count; // 3
```
