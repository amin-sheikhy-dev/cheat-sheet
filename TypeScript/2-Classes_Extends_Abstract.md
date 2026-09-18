```ts
// Classes & Interfaces ____________________________________________________________________________________
// وقتی از کلمه پابلیک استفاده میکنیم به این معنا هست که میتونی از پراپرتی در ابجکت استفاده کنی
// اگه کلمه پابلیک یا پرایوت حذف شود به طور پیشفرض پابلیک در نظر گرفته میشود ولی توی تابع کانستراکتور باید همیشه مشخص شود
class Account {
  constructor(
    public firstName: string,
    public lastName: string,
    public age: number,
    private pin: number,
    public gender?: 'male' | 'female' // این پراپرتی اختیاریه
  ) {}

  // readonly
  // به این معنیه که فقط میتونی اونو بخونی یا بگیری ولی نمیتونی هیچ تغییری توش انجام بدی readonly کلمه
  // ولی دقت کن که اگه ارایه باشه یا ابچکت میتونی بهش مقداری اضافه یا کم کنی که این برمیگرده به مباحث فنی جاوااسکریپت
  readonly region: string = 'asia';
  private readonly country: string = 'iran';

  // private
  private movements: number[] = [];
  // کلاس هایی که خصوصی باشن فقط از داخل کلاس در دسترس اند و خارج از کلاس حتی در کلاس های فرزند هم نمیشه به آنها دسترسی داشت
  getMovement() {
    return this.movements;
  }

  // Getters & Setters
  get myPin() {
    return this.pin;
  }
  // تابع ست نمیتونه چیزی رو ریترن کنه پس از ریترن استفاده نکن
  set setNewPin(pin: number) {
    this.pin = pin;
  }

  // static
  // اگه کلمه استاتیک نوشته شود میتونی قبل اینکه کلاس بسازی از اون پراپرتی استفاده کنی - بیشتر برای ساخت متد ها کاربرد داره
  static greet() {
    return 'Hello user!';
  }
}

Account.greet();

const user = new Account('amin', 'sheikhy', 21, 3232);

user.movements; // این چون پرایوته نمیشه بهش دسترسی داشت
user.getMovement(); // ولی با تابع میشه

user.region = 'american'; // نمیتونی تغییرش بدی چون از رداونلی استفاده میکنه

// تابع های ست و گت نیاز به کال کردنشون نیس و مثل یک پراپرتی عمل میکنن
user.myPin;
user.setNewPin = 1111;

// Inheritance Classes ____________________________________________________________________________________
// باشن رو نمیشه از اونا استفاده کرد و فقط میشه از آن ها ارث بری کرد abstract کلاس هایی که
abstract class User {
  constructor(
    name: string,
    age: number,
    private pin: number
  ) {}

  // protected
  // اینه که در کلاس های فرزندان در دسترس است - هیچکدوم خارج از کلاس در دسترس نیستن private با protected تنها فرق
  protected get userPin() {
    return this.pin;
  }
}

// ارث‌ بری برای پارامتر های تابع سازنده نیست! ارث‌ بری برای به اشتراک گذاشتن متدها و پراپرتی‌ها است
// الان کلاس جدیدی که ایجاد کردیم تمام ویژگی های کلاس پرنتشو به ارث برده
class UserAdmin extends User {
  constructor(name: string, age: number, pin: number, adminRole: 'Editor' | 'Designer' | 'superAdmin') {
    super(name, age, pin); // بعدش باید از کلمه سوپر استفاده کنیم تا متد های کلاس پرنت هم فراخوانی شود
  }
}

const admin1 = new UserAdmin('ali', 23, 1234, 'Designer');

abstract class Animal {
  abstract speak(): void;
}

class Dog extends Animal {} // را پیاده‌سازی کند speak() باید متد Dog چون

class Dog extends Animal {
  speak() {
    console.log('Woof!');
  }
}

// interface
// تقریبا شبیه کاستوم تایپ هست
interface Authentication {
  email: string;
  password: string;

  login(): void;
  logout(): void;
}

// توی اینترفیس میتونی دوباره بنویسیش و نقش اضافه کنی بهش
interface Authentication {
  phoneNumber: number;
}

// الان ما یه اینترفیس جدید ایجاد کردیم که اینترفیس های پرنتشو داره
interface AuthenticationAdmin extends Authentication {
  role: 'Guest' | 'Admin' | 'Editor';
}

const user: Authentication = {
  email: 'example@gmail.com',
  password: '1234abc',
  phoneNumber: 902_729_2543,

  login() {
    // code...
  },
  logout() {
    // code...
  },
};

// ویژگی دیگش اینه که میتونی در کلاس ها با کلمه کلیدی ایمپلمنت ازش استفاده کنی و کلاس رو مجبور میکنی از اون قواعد پیروی کنه
class AuthUser implements Authentication {
  constructor(
    public email: string,
    public password: string,
    public phoneNumber: number
  ) {}

  login() {}
  logout() {}
}
```
