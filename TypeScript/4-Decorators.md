```ts
// Decorator ____________________________________________________________________________________

function Something() {
  console.log('Hello');
}
// اجرا می‌کند Decorator آن را در جایگاه خاصی به عنوان framework/runtime است ولی Function خودش معمولاً یک Decorator در واقع

// Class Decorator
@Something
class User {}

// Method Decorator
class User {
  @Something
  login() {}
}

// Property Decorator
class User {
  @Something
  name = '';
}

// Accessor Decorator
class User {
  private _name = '';

  @Something
  get name() {
    return this._name;
  }
}

// Parameter Decorator
class User {
  login(@Something username: string) {}
}

//

function Logger(target: Function) {
  console.log(target.name);
  // Product
  // User
}

@Logger
class Product {}

@Logger
class User {
  name = 'Amin';
}

//

class User {
  name = 'Amin';
}

const user = new User();

user.greet(); // در حالی که داخل خود کلاس این متد را ننوشته‌ایم

function AddGreeting(target: Function) {
  target.prototype.greet = function () {
    console.log(`Hello ${this.name}`);
  };
}

@AddGreeting
class User {
  name = 'Amin';
}

// Method Decorator _________________________________________________________________________

function Logger(target: any, propertyKey: string, descriptor: PropertyDescriptor) {}
// target => User.prototype معمولا پروتوتایپ کلاس است

// propertyKey => "login" اسم پراپرتی یا متدی که دکوریتور روی ان قرار گرفته

// descriptor => login اطلاعات مربوط به
// descriptor.value = خود تابع لاگین

class User {
  @Logger
  login() {
    console.log('User logged in');
  }
}

//

function Log(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const originalMethod = descriptor.value;

  descriptor.value = function () {
    console.log('Calling login...');

    originalMethod();
  };
}

class User {
  @Log
  login() {
    console.log('User logged in');
  }
}

const user = new User();
user.login();
// Calling login...
// User logged in

// Decorator Factory ________________________________________________________________________

function Log(message: string) {
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    console.log(message);
  };
}

//

function Log(message: string) {
  return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    const originalMethod = descriptor.value;

    descriptor.value = function (...args: any[]) {
      console.log(message);

      return originalMethod.apply(this, args);
    };
  };
}

class User {
  @Log('User is logging in')
  login() {
    console.log('Login');
  }

  @Log('User is logging out')
  logout() {
    console.log('Logout');
  }
}

const user = new User();

user.login();
// User is logging in
// Login

user.logout();
// User is logging out
// Logout

// Decorator execution order _____________________________________________________________________
// دکوریتورهایی که به کلاس نزدیک‌تر هستند زودتر اجرا می‌شوند
@First
@Second
@Third
class User {}

// 1-@Third 2-@Second 3-@First

//

function First() {
  console.log('1');

  return function (target: Function) {
    console.log('2');
  };
}

function Second() {
  console.log('3');

  return function (target: Function) {
    console.log('4');
  };
}

// توابع از بالا اجرا میشن بعدش دکوریتور ها از پایین اجرا میشن

@First()
@Second()
class User {}
// 1
// 3
// 4
// 2

// Property Decorator _________________________________________________________

function Log(target: any, propertyKey: string) {
  console.log(target);
  console.log(propertyKey);
}
// target => User.prototype
// propertyKey => "name"

class User {
  @Log
  name = 'Amin';
}

//

class User {
  @Required
  name: string;

  @Required
  email: string;
}

// Parameter Decorator ________________________________________________

function Log(target: any, propertyKey: string, parameterIndex: number) {}
// target => UserController.prototype
// propertyKey => "login"
// parameterIndex => 0

class UserController {
  login(@Log username: string) {
    console.log(username);
  }
}

//

function Log(target: any, propertyKey: string, parameterIndex: number) {
  console.log('Method:', propertyKey);
  console.log('Parameter index:', parameterIndex);
}

class UserController {
  login(username: string, @Log password: string) {
    console.log(username, password);
  }
}
// Method: login
// Parameter index: 1

// Decorator Return Values _______________________________________________

// اضافه کردن یه متد با ساخت کلاس جدیدی که ارث بری شده از کلاس قبلی
function AddGreeting(target: Function) {
  return class extends (target as any) {
    greet() {
      console.log(`Hello ${this.name}`);
    }
  };
}

@AddGreeting
class User {
  name = 'Amin';
}

// کلا یه کلاس جدید ساختیم
function ReplaceClass(target: Function) {
  return class {
    name = 'New User';

    greet() {
      console.log('Hello');
    }
  };
}

@ReplaceClass
class User {
  name = 'Amin';
}

// Reflect Metadata ____________________________________________________

class User {
  name = 'amin';

  // role = 'admin' میخامم این پراپرتی رو بهش اضافه کنیم
}

// ست کردنش
// ذخیره کن admin با مقدار role به اسم metadata یک User روی
Reflect.defineMetadata('role', 'admin', User);

// خوندنش
// رو بده role به اسم User , metadata از
Reflect.getMetadata('role', User);

//

import 'reflect-metadata';

function Role(role: string) {
  return function (target: Function) {
    Reflect.defineMetadata('role', role, target);
  };
}

@Role('admin')
class User {
  name = 'amin';
}

const role = Reflect.getMetadata('role', User);

console.log(role); // admin

// فقط برای ادمین‌هاست Controller می‌خوایم مشخص کنیم یک
function Roles(...roles: string[]) {
  return function (target: Function) {
    Reflect.defineMetadata('roles', roles, target);
  };
}

@Roles('admin', 'manager')
class UsersController {}

const roles = Reflect.getMetadata('roles', UsersController);

console.log(roles); // ["admin", "manager"]
```
