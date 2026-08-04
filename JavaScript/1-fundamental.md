```js
'use strict'; // این باعث میشه تا خطاها و باگ هایی که توی برناممون داشتیم رو بیاد بهمون بگه و بهتره که اول هر پروژه ای بزاریمش

// استفاده میکنیم const و let برای تعریف متغیر از
// هیچوقت تغییر نمیکند const تعریف کردیم رو دوباره مقدار دهی کنیم ولی مقدار متغیر های let اگر بخاهیم میتوانیم متغیری که با
let myName = 'amin';

const myBirthYear = '1384';

// تبدیل رشته به عدد
Number(myBirthYear); // 1384

// تبدیل عدد به رشته
String(23); // '23'

let x = 0;
x += 10; // x = x + 10

x *= 4; // x = x * 4

x++; // x = x + 1

x--; // x = x - 1

// دوتا ستاره نشون دهنده توان رساندنه - 2 به توان 7
2 ** 7; // 128

// falsy values: 0 , '' , undefined , null , NaN , false

// سه خط مساوی یعنی اینکه دو طرف تساوی رو با هم مقایسه میکنه که یکی هستن یا خیر
// سه مساوی نوع و مقدار رو مقایسه میکنه و مقدار رو تغییر نمیده

5 === '5'; // false  چون نوع‌ها متفاوت‌اند (عدد و رشته)
'19' === 19; // false
true === 1; // false  چون نوع فرق داره (boolean و number)
null === undefined; // false   چون نوع‌ها فرق دارن

// دو مساوی نوع رو مقایسه میکنه و تبدیل نوع انجام میده
5 == '5'; // true   چون '5' به عدد تبدیل می‌شود
true == 1; // true   چون true به 1 تبدیل میشه
null == undefined; // true    چون جاوااسکریپت این دو رو "برابر" در نظر می‌گیره

// هست string نوشته بشه خروجی رشته یا همان prompt هر چیزی توی
// const ageUser = prompt('inter your age:')

// استفاده از ==! در مواقع که بخای بگی مخالف باشه
12 !== 9; // true
9 !== 9; // false
!''; // true
!'amin'; // false

const value1 = true;
const value2 = true;

if (value1 && value2) ''; // زمانی ک هر دو شرط درست باشن اجرا میشه
if (value1 || value2) ''; // زمانی ک حداقل یکی از دو شرط درست باشن اجرا میشه

// switch case
const rooz = '3shanbe';

switch (rooz) {
  case 'shanbe':
  case '2shanbe':
    'kelas nadari';
    break;

  case '1shanbe':
    'madar manteghi 7:30 | ons qoran 10:30 | barname sazi 14:45';
    break;

  case '3shanbe':
    'mabani 7:30 | riazi 14:45';
    break;

  case '4shanbe':
    'fizik2 16:30';
    break;

  case '5shanbe':
    'zaban 11:30';
    break;

  case 'jome':
    'Gosaste 7:30';
    break;

  default:
    'rooz namoshakhas!!!';
}

const myAge = 20;
// مقدار دادن به یک متغیر بر اساس شرایط
// اگه شرط داخل درست باشه اولی رو میده - اگه هم شرط برقرار نشه دومی میده
const drink = myAge >= 18 ? 'i like wine 🍷' : 'i like water 💧';

// function & return
// اسم تابع ما لاگر هستش
function logger() {
  'My Name Is Amin :D';
}

// برای استفاده از تابع باید اسم آن را همراه پرانتز های جلوش بزاریم
logger();

// داخل پرانتز اطلاعات ورودی تابع رو میزاریم
// این مقادیری یا هرچیزی که داخل تابع میزاریم ارگومان های تابع حساب میشوند
function information(name, city) {
  return `my name is ${name} and I live in ${city}`;
}

information('sara', 'tehran'); // my name is sara and I live in tehran

information('amin', 'qom'); // my name is amin and I live in qom

// function Expression
const adder = function (a, b) {
  return a + b;
};
adder(2, 5); // 7

// function Declaration
function adder2(a, b) {
  return a + b;
}
adder2(7, 2); // 9

// Arrow function
// یک نوع خاص از اکسپرشن ها هستن arrow function
// در توابع یه خطی که کوتاه هستن خیلی مفیدن و کاربردی اند و لازم نیس چیزی ریترن بشه در اون ها
const adder3 = (a) => a + 2;
// دو متغیری
const adder4 = (a, b) => a + b;

// استفاده کنیم باید گیومه باز کنیم و عین تابع معمولی ریترن کنی Arrow function اگر در شرایطی بیشتر از یه خط کد داشتیم و میخاستیم از
const adder5 = (a, b) => {
  c = a + b;
  return c;
};

// Array fundamental
// ارایه ها برای جمع اوری اطلاعات و ذخیره آن ها در یک جای مشخص استفاده میشه
const friends = ['ahmad', 'mmd', 'keyvan', 'hasan'];

// برای استفاده از ارایه ها باید جلوی ارایه آکولاد باز کنیم و ایندکس اون مقداری که در ارایه داریم رو بزاریم
// دقت شود که شمارش ارایه ها از 0 شروع میشود یعنی اگر بخاییم به اولین مقدار ارایه دسترسی داشته باشیم باید عدد 0 رو به آن ارایه بدهیم
friends[0]; // 'ahmad'
friends[3]; // 'hasan'

// استفاده کنیم length برای به دست اوردن تعداد اعضای ارایه میتونیم از متد
friends.length; // 4

// عوض کردن یکی از مقادیر ارایه
friends[1] = 'reza';
friends[2] = 'mohsen';

// ارایه ها میتونن هر مقداری رو در خودشون ذخیره کنن برای مثال در ارایه زیر انواعی از مقدار های مختلف یعنی استرینگ - متغیر و عدد در ارایه زیر دیده میشود
const firstName = 'amin';

const amin2 = [firstName, 'sheikhy', 1404 - 1384, 'programmer', friends]; // حتی میشود در یک ارایه ارایه دیگری را ذخیره کرد

// Objects fundamental
// در ابجکت ها هم مثل ارایه ها هر چیزی میتونی قرار بدی مثل تابع یا بولین ها

const aminObjects = {
  firstName: 'amin', // فرست نیم کلید هست و امین ولیو هست و به کل این خط میگن متد

  lastName: 'sheikhy',

  age: 1404 - 1384,

  job: 'programmer',

  friends: ['matin', 'fatemeh', 'mohammad'],

  driverLicense: true,

  calcAge: function (birthYear) {
    return 1404 - birthYear;
  },
};
// برای دسترسی به پراپرتی های موجود اول اسم ابجکت و بعد نقطه بعد کلیدش
aminObjects.job; // 'programmer'

// میشه از براکتم استفاده کرد اینجوری که باید رشته ای از پراپرتی که میخای داخل براکت بزاری
aminObjects['programmer'];

// قرار دادن اکسپرشن داخل براکت
const nameKey = 'Name';
aminObjects['first' + nameKey];
aminObjects[`last${nameKey}`];

// اضافه کردن پراپرتی به یه ابجکت
aminObjects.city = 'Qom';

// اضافه کردن پراپرتی به یه ابجکت با استفاده از براکت
aminObjects['location'] = 'iran';

// Loop fundamental
// حلقه ها تا زمانی که شرط ما ترو باشد اجرا میشود
// اولی یعنی از چند شروع شه و دومی یعنی شرط چی باشد که تا کجا ادامه یابد و سومی یعنی چنتا چنتا اضافه شه
for (let i = 1; i < 4; i++) {
  `Lifting weights repetition ${i} 🏋️‍♀️`;
}

// Continue
const UX = [1, 'a', true];
// حتی اگه شرط داخل پرانتز برقرار بود از اون یدونه چشم پوشی کن و بقیه رو ادامه بده
for (let i = 0; i < UX.length; i++) {
  if (typeof UX[i] !== 'number') continue; // اگه به عضوی رسیدی که نوعش نامبر نبود از اون چشم پوشی کن و حلقه رو ادامه بده
}

// Break
for (let i = 0; i < UX.length; i++) {
  if (typeof UX[i] === 'number') break; // به اولین عدد رسید حلقه رو متوقف کنه
}

// while loop
let i = 0;
// تا زمانی که شرط داخل پرانتز درست باشد کد داخل حلقه اجرا میشود
while (i < 5) {
  // your code...
  i++;
}

// do while loop
let j = 10;
// ابتدا بدنه حلقه یک بار اجرا می‌شود سپس شرط بررسی می‌شود
do {
  j;
} while (j < 5);
```
