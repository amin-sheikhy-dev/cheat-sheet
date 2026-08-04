```js
// Converting And Checking Numbers_________________________________________
// توی جاوااسکریپت اعداد همه دسیمال یا همون اعشاری هستن حتی اگه ما نزاریم
23 === 23.0; // true

0.1 + 0.2; // 0.30000000000000004

0.1 + 0.2 === 0.3; // false

// تبدیل میشه num به str روش هایی که
Number('23'); // 23

+'23'; // 23

// .parseInt(string, radix)
// استخراج عدد از استرینگ
// ورودی دوم بیس رو میگه که بیس 10 هست - این ورودی اختیاریه
Number.parseInt('30px', 10); // 30

// باید با عدد شروع شه مگرنه کار نمیکنه
Number.parseInt('e23', 10); // NaN

Number.parseInt('  2.5rem  '); // 2

// .parseFloat()
// مثل همون بالایی فقط اعداد اعشاری هم خارج میکنه
Number.parseFloat('  2.5rem  '); // 2.5

// .isNaN();
// میده true میده نباشه false چک میکنه عدد هست یا نه اگه عدد باشه
Number.isNaN(20); // false
Number.isNaN('20'); // false
Number.isNaN(+'20X'); // true
Number.isNaN(23 / 0); // false
// میده دیگه infinity بالایی

// .isFinite()
// پایان پذیر بودن رو بررسی میکنه
// همچنین بهترین تابع برای چک کردن اینکه ببینیم عدد هست یا نه
Number.isFinite(20); // true
Number.isFinite('20'); // false
Number.isFinite(+'20X'); // false
Number.isFinite(23 / 0); // false

// .isInteger()
// صحیح بودن یا نبودن عدد رو بررسی میکنه
Number.isInteger(23); // true
Number.isInteger(23.0); // true
Number.isInteger(23.3); // false
Number.isInteger(23 / 0); // false

// Maths_________________________________________
/* متد       |         توضیح       |               مثال            |           خروجی      |
|==================|==============================|=====================|================|
|  Math.PI         | عدد پی (π)                  |  Math.PI            |  3.14159...    |
|  Math.sqrt(x)    | جذر (ریشه دوم)              |  Math.sqrt(16)      |  4             |
|  Math.pow(a, b)  | توان                        |  Math.pow(2, 3)     |  8             |
|  Math.abs(x)     | قدر مطلق                   |  Math.abs(-5)       |  5             |
|  Math.floor(x)   | گرد به پایین                 |  Math.floor(3.9)    |  3             |
|  Math.ceil(x)    | گرد به بالا                  |  Math.ceil(3.1)     |  4             |
|  Math.round(x)   | گرد به نزدیک‌ترین عدد صحیح   |  Math.round(2.5)    |  3             |
|  Math.max(...)   | بیشترین مقدار              |  Math.max(3, 7, 1)  |  7             |
|  Math.min(...)   | کم‌ترین مقدار              |  Math.min(3, 7, 1)  |  1             |
|  Math.trunc(x)  | حذف قسمت اعشار بدون گرد |   Math.trunc(3.78)  |  3             |
|=================|==========================|=====================|===============*/

// اعداد به توان کسر رسونده بشن رادیکال میگیرن
25 ** (1 / 2); // 5
8 ** (1 / 3); // 2

// شبیه هم نیستن trunc و floor متد های
// توی اعداد منفی فرق دارن
Math.trunc(-23.3); // -23
Math.floor(-23.3); // -24

/*      هدف                   |      کد پیشنهادی                          |
|=======================|=================================================|
| عدد بین 0 و 1          |  Math.random()                                 |
| عدد بین 0 تا n         |  Math.random() * n                             |
| عدد صحیح بین 0 تا n-1 |  Math.floor(Math.random() * n)                 |
| عدد صحیح بین a تا b   |  Math.floor(Math.random() * (b - a + 1)) + a  |
|======================|=============================================*/

// ساخت تابعی ک خودمون مشخص کنیم یه عدد بین اونا بگه
const randomInt = (min, max) => Math.trunc(Math.random() * (max - min + 1)) + min;
randomInt(10, 20);

// Rounding Decimals_________________________________________
// این متد خروجی استرینگ میده
// داده میشه به عنوان نمایش اعداد اعشار نشون میده toFixed ورودی که به متد
(2.7).toFixed(3); // '2.700'
(2.345).toFixed(2); // '2.35'
+(2.345).toFixed(2); // 2.35

// اگر ورودی ندیم یا ورودی 0 بدیم به طور خودکار گرد کردن انجام می‌دهد
(2.7).toFixed(0); // '3'
(2.3).toFixed(); // '2'

// باقیمانده تقسیم رو نشون میده
5 % 2; // 1
5 / 2; // 2.5

8 % 3; // 2
8 / 3; // 2.6666666666666665

// Numeric Separators_________________________________________
// برای اینکه بخاییم اعداد رو به دسته بندی های مختلف قسمت کنیم - توی چاپ کردن یا استفاده کردن اینا حساب نمیشن
287_460_000_000; // 287460000000
15_00; // 1500
1_5_0_0; // 1500

Number('230_000'); // NaN
parseInt('230_000'); // 230

// Bigint_________________________________________
// ها هستن integer ها یه نوع خاصی از bigInt

// میتونه نشون بده JavaScript این بزرگترین ترین عددی هست که
2 ** 53 - 1; // 9007199254740991

// حتی خودش هم نشون میده
Number.MAX_SAFE_INTEGER; // 9007199254740991

// توی این ها دقت رو از دست میدیم
2 ** 53 + 1; // 9007199254740992
2 ** 53 + 2; // 9007199254740994
2 ** 53 + 3; // 9007199254740996
2 ** 53 + 4; // 9007199254740996

// اخر جمله بزاری میتونی عددو کامل دریافت کنی n اگه یدونه

4838430248342043823408394839483204n; // 4838430248342043823408394839483204n
// بهتره فانکشن وقتی استفاده ک اعدادمون کوچیکترن
BigInt(48384302); // 48384302n

// میشه محاسباتم کرد باهاش
10000n + 10000n; // 20000n
36286372637263726376237263726372632n * 10000000n; // 362863726372637263762372637263726320000000n

const bigInt = 20289830237283728378237n;
const num = 23;
// ها محاسبه کنه dataType رو با سایر bigint نمیتونه
bigInt * BigInt(num); // و باید اول تبدیل کنیم به بیگ اینت

typeof 20n; // bigint
20n > 15; // true
20n < 15; // false
20n === 20; // false
20n == 20; // true
20n == '20'; // true

// Date_________________________________________
const now = new Date();
now; // Mon Dec 22 2025 10:48:19 GMT-0800 (Pacific Standard Time)

// 2019-11-01 سال ماه روز
// T جدا کننده تاریخ و زمان
// 13:15:33.035 ساعت دقیقه ثانیه میلی ثانیه
// اگه بزاریش یعنی این ساعت به وقت جهانی تعریف شده اگه نزاریش یعنی تو کشور خودتون و به وقت محلی تعریف شده - یعنی به وقت جهانی Z
new Date('2019-11-01T13:15:33.035Z'); // Fri Nov 01 2019 06:15:33 GMT-0700 (Pacific Daylight Time)

// (year, month, day, hour, minute, second, ms)
new Date(2222, 10, 19, 15, 23, 5); // Tue Nov 19 2222 15:23:05 GMT-0800 (Pacific Standard Time)

const future = new Date(2037, 10, 19, 15, 23);
future; // Thu Nov 19 2037 15:23:00 GMT-0800 (Pacific Standard Time)

future.getFullYear(); // دریافت سال تاریخ
// 2037
future.getMonth(); // دریافت ماه
// 10
future.getDate(); // دریافت روز اون ماه
// 19
future.getDay(); // دریافت روز هفته - یعنی مثلا چهارمین روز هفته یا مثلا چهارشنبه
// 4
future.getHours(); // ساعت
// 15
future.getMinutes(); // دقیقه
// 23
future.getSeconds(); // ثانیه
// 0

// فرمت خاص
future.toISOString(); // 2037-11-19T23:23:00.000Z

// Operation With Date_________________________________________
// TimeStamp خروجی یه عدد میده که بهش میگن Date منهای Date در کل
const future2 = new Date(2020, 10, 19, 15, 23);
Number(future2); // 1605828180000

// هر روز 24 ساعت هر ساعت 60 دقیقه و هر دقیقه 60 ثانیه و هر ثانیه 1000 میلی ثانیه
const calcDaysPassed = (date1, date2) => Math.round(Math.abs(date1 - date2) / (1000 * 60 * 60 * 24));
calcDaysPassed(new Date(), future2); // 1859

// Internationalizing Date (Intl)_________________________________________
// به طور کلی تاریخو بر اساس شهر و منطقه مشخص میکنه
const localDate = new Intl.DateTimeFormat('fa-IR').format(now);
localDate; // ۱۴۰۴/۱۰/۱

// کمی تنظیمات بهش اضافه کنیم
const options = {
  hour: 'numeric',

  minute: 'numeric',

  second: 'numeric',

  day: 'numeric',

  weekday: 'long', // اسم روز رو هم میگه روی لانگ باشه

  month: 'numeric',
  // month: 'long', اسم ماه رو مینویسه
  // month: '2-digit', دو رقمی ماه رو نشون میده مثلا مرداد رو میگه 05 و بقیه ماها

  year: '2-digit', // دود رقم اخر سال رو نشون بده
  // year: 'numeric',
};

const localDate2 = new Intl.DateTimeFormat('fa-IR', options).format(now);
localDate2; // دوشنبه ۰۴/۱۰/۱, ۱۲:۰۱

// Timers: settimeout & setinterval_________________________________________

// settimeout: یک بار بعد از گذشت یک زمان مشخص یه کد اجرا بشه
// setTimeout(() => console.log('im frontend deveoper'), 1000);
// ورودی دوم ثانیه هست و بر حسب میلی ثانیه اس

// بهش ورودی هم میدیم - اسم هم براش تعریف میکنیم
const pizzaTimer = setTimeout((ing1, ing2) => console.log(`your pizza recived with ${ing1} and ${ing2}`), 1000, 'cheese', 'spinach');

// تابع رو پاک میکنیم تا اجرا نشه
clearTimeout(pizzaTimer);

// setinterval: برای اینه که به صورت تکرارشونده در فاصله ها مشخص که ما تایین میکنیم یه کد اجرا بشه
// const clock = setInterval(() => console.log('Hi'), 1000);
// ورودی دوم ثانیه هست و بر حسب میلی ثانیه اس

// مثال ساخت تایمر
let timerValue = 1950;

const clock = setInterval(() => {
  timerValue--;

  let seconds = timerValue % 60;
  let mins = Math.trunc(timerValue / 60);

  console.log(`${mins < 10 ? '0' : ''}${mins}:${seconds < 10 ? '0' : ''}${seconds}`);
}, 1000);

// تابع رو پاک میکنیم تا اجرا نشه
clearInterval(clock);
```
