```js
// SCOPE & SCOPECHAIN_________________________________________
// وقتی بخای از یه متغیر استفاده کنی باید در خط های قبلی در همان اسکوپ اون رو تعریف کرده باشی
// ولی اگه مثلا تو گلوبال اسکوپ یه متغیر تعریف کردی ولی تو فانکشن اسکوپ های قبل آن ازش استفاده کنی ارور نمیده چون گلوبال اسکوپ اسکوپ پرنت آن است
// ولی تو گلوبال اسکوپ یه متغیری تعریف کنی و توی همان اسکوپ و خط های قبلی ازش بخای استفاده کنی نمیشه

// این فانکشن کالک ایج 3 توی گلوبال اسکوپ تعریف شده و تاپ لول هست
// SCOPE 1
function calcAge3(birthYear) {
  // و توی اسکوپ خودش هم مقادیر تعریف شده ای هم دارد
  const age4 = 1404 - birthYear;

  firstName4; // این داخل اسکوپ خود کالک ایج نیست

  // SCOPE 2
  function printAge() {
    let output2 = 'signing'; // قراره تو اسکوپ چایلد عوض شه

    // الان به اسکوپ های پرنتش هم که شامل فانکشن و گلوبال اسکوپ میشه دسترسی داره چون داخل اسکوپ چینش هست
    const output = `${firstName4} have ${age4} years old and born in ${birthYear}`;

    // SCOPE 3
    if (birthYear >= 1380) {
      // الان من یه متغیر تعریف کردم که توی اسکوپ پرنتش هم بود و به نظرت توی کنسول کدومو لاگ میگیره ؟
      const firstName4 = 'amin';
      // توی کنسول متغیر اسکوپ خودش رو لاگ میگیره
      // چون که انجین جاوااسکریپت اول توی اسکوپ خودش دنبال اون متغیر میگرده و اگه پیدا نکرد به ترتیب به اسکوپ های پرنتش سر میزنه و دنبال اون متغیر میگرده
      // یعنی در واقع متغیر لوکال به متغیر گلوبال ارجعیت داره

      var blc = 'dahe hashtadie';
      const str2 = `${firstName4} dahe 80 hastesh va ${birthYear} sal tavalodeshe`; // الان این داخل اسکوپش تعریف شده

      const add = (a, b) => a + b;

      add(4, -9);

      // اینجا اومدم مقدار اوتپوت 2 رو در اسکوپ چایلد عوض کردم و هرجا بخاییم از این متغیر استفاده کنیم همینی که در چایلد ست کردیم اجرا میشه
      output2 = 'exiting';
    }
    output2;

    // add(3, 4) ارور میده چون فانکشن داخل بلاک اسکوپ هست
    // str2 ارور میده جون داخل اسکوپش نیست

    // در واقع وار فانکشن اسکوپ هست نه بلاک اسکوپ و الان متعلق به پرینت ایج هستش
    blc; // ولی الان ارور نمیده چون مقدارش با وار تعریف شده نه با کانست و لت
  }

  return age4;
}
const firstName4 = 'amin';

// HOISTING , TDZ_________________________________________

man; // قبل تعریف کردنش میشه بهش دسترسی داشت ولی چون مقدار اولیه اش روی اندیفایند تعریف شده نمیشه باهاش کاری انجام داد یا ازش استفاده کرد
var man = 'amin'; // undefined چون متغیر هایی که با وار تعریف میشن هویست میشن ولی مقدار اولیه شون رو اندیفایند ست میشه

// قبل تعریف کردنشون نمیشه بهشون دسترسی داشت چون تو محیط تی دی زد هستن
// job3
// year2
let job3 = 'programmer';
const year2 = 1384;

// این فانکشن ها هویست میشن چون مقدار اولیه شون روی خودشون ست میشه
Dec(2, 3);

function Dec(a, b) {
  return a + b;
}

// همانند کانست و لت هویست نمیشن
// Expr(2, 3)
const Expr = function (a, b) {
  return a + b;
};

// همانند کانست و لت هویست نمیشن
// Arrow(2, 3)
const Arrow = (a, b) => a + b;

// این چون با وار تعریف شده میتونی کالش کنی ولی بهت مقدار اندیفایند میده و نمیتونی از تابع استفاده کنی انگار یچیز اندیفایند کال میکنی
ExprVar;
var ExprVar = function (a, b) {
  return a + b;
};

// THIS KEYWORD_________________________________________

const calcAge4 = function (birthYear) {
  1404 - birthYear;
  this;
  // فانکشن های معمولی برخلاف ارو فانکشن ها دیس کی ورد مخصوص خودشون رو دارن که روی اندیفایند ست میشه
};

const calcAgeArrow = (birthYear) => {
  1404 - birthYear;
  // بجاش از لکسیکال دیس کی ورد استفاده میکنن
  // لکسیکال دیس کی ورد یعنی مقدار دیس کی ورد اسکوپ پرنتشون
};

const jonas = {
  name: 'jonas schmedtmann',

  birthYear: 1380,

  loger: function () {
    this; // این دیس کی ورد به ابجکت جوناس اشاره میکنه

    1404 - this.birthYear;

    const that = this; // that یا self راه حلی که قبل نسخه 6 استفاده میشد
    const dahe80 = function () {
      // this اندیفایند
      // that ولی این درسته و به ابجکت جوناس اشاره میکنه

      // this.birthYear >= 1380 && this.birthYear <= 1390 اندیفایند ست میشه و ارور میده
      ('dahe 80? ba expretion', that.birthYear >= 1380 && that.birthYear < 1390);
    };
    dahe80();

    // راه حل دوم اینه از ارو فانکشن استفاده کنیم
    // چون دیس کی وردش روی فانکشن پرنتش سیو میشه و فانکشن پرنتش دیس کی وردش همون جوناسه
    const dahe80_2 = () => {
      ('dahe 80? ba arrow', this.birthYear >= 1380 && this.birthYear < 1390);
    };
    dahe80_2();
  },
};

const matilda = {
  birthYear: 1390,
  name: 'matilda',
};

matilda.loger = jonas.loger; // هر فانکشن فقط یک مقدار یا ولیو هست پس میشه همون مقدارو کپی کرد برای یه متغیر دیگه

matilda.loger(); // الان دیس کی ورد که توی تابع لاگر جوناس بود و به جوناس اشاره میکرد الان رفته توی ماتیلدا و به ماتلیدا اشاره میکنه

const mamadBob = {
  firstName: 'mamad bobi',
  birthYear: 1374,

  information: () => {
    // دیس کی وردش روی گلوبال اسکوپ ست میشه
    this; // این الان باید ممد باب رو لاگ بگیره ولی گلوبال رو لاگ میگیره
  },
  // ارو فانکشن ها دیس کی ورد مخصوص خودشون رو ندارن و دیس کی ورد اسکوپ یا فانکشن پرنتشون استفاده میکنن
  // پس دیس کی ورد این فانکشن روی دیس کی ورد گلوبال اسکوپ ست میشه
  // اینم در نظر بگیر که این ابجکت کد بلاک حساب نمیشه و تمام ابجکت ما داخل گلوبال اسکوپ هست چون در واقع پس متد اینفورمیشن ما پرنتش میشه گلوبال اسکوپ
};

// arguments for functions_________________________________________
const addExpr2 = function (a, b) {
  arguments; // ارگیومنت درواقع یه ارایه هست از ورودی های فانکشن و زمانی کاربرد داره که ورودی های بیشتری به فانکشن میدیم

  return a + b;
};
addExpr2(2, 3, 6, 7, 8); // هیچ مشکلی نداره که موقع کال کردن فانکشن ورودی های بیشتری بهش بدیم ولی فقط ورودی های جدید اسمی ندارن ولی داخل ارگیومنت هستن
// همچنان میتونیم داخل اعضای ارایه لوپ بزنیم و ازشون استفاده کنیم

// ارو فانکشن همونطور که دیس کی ورد نداره پس ارگیومنت هم نداره
const addArrow2 = (a, b) => {
  // arguments براشون ارگیومنت تعریف نشده
  return a + b;
};
addArrow2(3, 4, 5);

// Primitive Type  VS  Reference Type_________________________________________

// مقدار مستقیماً ذخیره و کپی میشود
// Primitive Type
// انواعشون
// String , Number , Boolean , Undefined , Null , BigInt , Symbol

let age5 = 30;
let oldAge = age5;
age5 = 31;

// الان چیزی ک انتظار میره اینه اولد ایج 30 باشه و ایج 31 و درست هم هست
age5; // 31
oldAge; // 30

// داده در حافظه را نگه می‌دارد نه خود داده را (Reference) متغیر آدرس
// Reference Type
// انواعشون
// Object , Array , Function , Date , Map , Set , ....

const me = {
  name: 'amin',
  age: 20,
};

const friend4 = me;
friend4.age = 23; // الان اینجا ایج دوستم رو عوض کردم و گزاشتم 23 سن دوستم رو

// آن را نگه می‌دارد Reference متغیر خود آبجکت را نگه نمی‌دارد بلکه

// الان چیزی ک انتظار داریم اینه که ایج من 20 باشه ولی ایج دوستم 23 باشه
// ولی طبق چیزی ک انتظار داشتیم نشد
me = 23;
friend4 = 23;

// متفاوت هستند Reference چون دو
const objTest = {} === {}; // false

// متفاوت هستند Reference چون دو
const arrTest2 = [] === []; // false

const arrTest3 = [];
const arrTest4 = arrTest3;
arrTest3 === arrTest4; // true

// است Reference Type هم Function آیا
// است Object هم یک Function در JavaScript

// مثال های دیگه
// Primitive Type
let lastName2 = 'jokar';
let oldLastName = lastName2;
lastName2 = 'sheikhy';
// oldLastName = 'jokar'
// lastName2 = 'sheikhy'

// Reference Type
const jessica = {
  firstName: 'jessica',

  lastName: 'Williams',

  age: 27,

  family: ['marry', 'daniel'],
};

const marriedJessica = jessica;
marriedJessica.lastName = 'Davis';
// jessica.lastName = 'Davis'
// marriedJessica.lastName = 'Davis'

// Object.assign(target, sources)
// تارگت یعنی شی مقصدی که مقادیر به آن کپی می‌شوند
// سورس یعنی یک یا چند شی مبدا ک ویژگی هاشون تو تارگت کپی میشه

const jessicaCopy = Object.assign({}, marriedJessica); // الان دیگه یه ابجکت کاملا مجزا ساختیم با ویژگی های همون ابجکت
jessicaCopy.age = 40;
jessicaCopy.firstName = 'elizabeth';
jessicaCopy.lastName = 'miller';

// الان انتظار داریم جسیکا کپی توش 4 تا فمیلی باشه و توی جسیکا اصلی 2 تا فمیلی
jessicaCopy.family.push('john');
jessicaCopy.family.push('miple');

// Shallow copy , Deep copy_________________________________________
// آن‌ها همچنان مشترک است Reference فقط سطح اول آبجکت را کپی می‌کند و اگر داخل آن آبجکت یا آرایه‌ی دیگری وجود داشته باشد، Shallow Copy
// Deep Copy تمام سطوح آبجکت را به صورت کامل کپی می‌کند و هیچ Reference مشترکی باقی نمی‌ماند

// Shallow Copy
const user = {
  name: 'Amin',
  address: {
    city: 'Tehran',
  },
};

const copy = { ...user };

user.name; // Amin

// Deep copy
const copy2 = structuredClone(user);

copy.address.city = 'Berlin';

user.address.city; // Tehran
```
