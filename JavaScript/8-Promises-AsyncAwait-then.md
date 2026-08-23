```js
// https://restcountries.com/
// برای گرفتن اطلاعات کشور ها api

// Promises , Fetch API_________________________________________
// شده اش کار کنیم fullFilled میتونیم با حالت then میده که با متد promise در واقع یه
Promise.resolve({ json: () => Promise.resolve([{ name: { common: 'Portugal' } }]) })
  // هندل میکنیم then رو به کمک متود promise شده fullFilled بعد مقدار
  .then((res) => res.json()) // رو کال کنیم تا خروجی جیسون بده json برای اینکه بتونیم دیتا رو بخونیم باید متد res از
  .then((data) => data);

// Chaining , Rejected Promises , Throwing Errors_________________________________________
// ساخت تابع که هم ارور رو بده هم جیسون
async function getJSON(URL) {
  const res = await fetch(URL);

  // پرامیس فقط زمانی ریجکت میشه که نت کاربر قطع بشه پس اگر جای دیگه مشکل داشته باشیم اصلا ریجکت نمیشه
  if (!res.ok) throw new Error(`Error:${res.status}`);
  const data = await res.json();

  return data;
}

const country = 'iceland'; // همسایه نداره
// const country = 'iran';

document.querySelector('.el5').addEventListener('click', () => {
  //
  getJSON(`https://restcountries.com/v3.1/name/${country}`)
    .then((result) => {
      const [data] = result;

      // console.log(data);

      if (!data.borders) throw new Error('❌ همسایه نداریم');

      return getJSON(`https://restcountries.com/v3.1/alpha/${data.borders[0]}`);
    })
    .then((result) => {
      const [data] = result;

      // console.log(data);
    })
    // هر اروری هر جای زنجیره بیاد هندل میشه
    .catch((error) => 'console.log(error.message)') // یه ابجکت هست که پراپرتی مسیجو برداشتیم error در واقع

    .finally((fin) => {}); // این متد چه پرامیس ریجکت شه چه نشه اجرا میشه
});

// Event Loop => CallBack Queue , MicroTasks_________________________________________
// جوابو حدس بزن کدوم اول و کدوم اخر لاگ گرفته میشه

// اول این دوتا لاگ گرفته میشن چون تاپ لول کد هستن
// console.log('Test Start');

// قرار میگیره پس اخر اجرا میشه CallBack Queue چون داخل
// setTimeout(() => console.log('0 sec timer'), 0);

// اولویت داره CallBack Queue هستش پس به MicroTasks چون داخل
// Promise.resolve('resolved promise').then((response) => console.log(response));

// اول این دوتا لاگ گرفته میشن چون تاپ لول کد هستن
// console.log('Test End');

// Building a Promise_________________________________________
const prm = randomInt(0, 10); // عدد رندوم
// این پرامیس جدید ریترن میکنه - تابع دوتا ورودی میگیره یکی قبول شدن یکی رد شدن پرامیس
const creatPromise = new Promise((resolve, reject) => {
  if (15 > prm) resolve('promise accept ✅');
  else reject(new Error('promise reject ❌'));
});

// console.log(prm);
// creatPromise.then((res) => console.log(res)).catch((err) => console.log(err));

// Async Await => Consuming Promise , Error Handling_________________________________________
// استفاده کنیم await برمی‌گردونه و داخلش باید از Promise رو قبل تابع می‌نویسیم اون تابع به صورت خودکار یک async وقتی
const whereAmI = async function (country) {
  try {
    const res = await fetch(`https://restcountries.com/v3.1/name/${country}`);

    if (!res.ok) throw new Error(`error ${res.status} 😫`);

    const [data] = await res.json();

    // console.log(res);
    // console.log(data);
  } catch (err) {
    // console.log(err.message);
    // console.log(err);
  } finally {
    // console.log('Request completed (successful or unsuccessful)');
  }
};

// whereAmI('portugal');
// Promise Type => all , race , Allsettled , Any_________________________________________
// یک ارایه از پرامیس ها میدن و باعث میشه تمام پرامیس ها با هم استارت دانلود شدنشون بخوره و در زمان صرفه جویی شه

// Promise.all ( [………] )
const promiseAll = async function () {
  // کاربرد دقیق اینه که وایمیسه همه پرامیس ها فول فیل شن بعد ارایه رو میده - اگه یه پرامیس ریجکت شه کلا ریجکت میکنه
  const resolve = await Promise.all([
    getJSON(`https://restcountries.com/v3.1/name/germany`),
    getJSON(`https://restcountries.com/v3.1/name/russia`),
    getJSON(`https://restcountries.com/v3.1/name/usa`),
  ]);

  console.log(resolve.flat());
};
// promiseAll();

// Promise.race ( [………] )
const promiseRace = async function () {
  // پرامیسی ک سریع تر تکلفش مشخص چه ریجکت چه فول فیل شده باشه رو ریترن میکنه - از ارایه ک بهش دادیم هم کلا یدونه ریترن میکنه
  // و خب قاعدتا پرامیسی ک ریجکت شده تکلیفش سریع تر روشن میشه
  const resolve = await Promise.race([
    getJSON(`https://restcountries.com/v3.1/name/germany`),
    getJSON(`https://restcountries.com/v3.1/name/russia`),
    getJSON(`https://restcountries.com/v3.1/name/usa`),
    Promise.reject('error❗'),
  ]);

  // اگه نگاه کنی میبینی کلا یدونه ریترن میکنه و هر دفعه رفرش کنی یدونه متفاوت میده یعنی همونی ک تکلیفش مشخص شده
  // console.log(resolve.flat());
};
// promiseRace();

// Promise.allSettled ( [………] )
const promiseAllSettled = async function () {
  // تقریبا مثل آل ولی با این تفاوت که در هر صورت ارایه رو میده چه پرامیسی ریجکت شه چه فول فیل یعنی مقدار ریجکت شده پرامیس هم میده
  const resolve = await Promise.allSettled([
    getJSON(`https://restcountries.com/v3.1/name/germany`),
    getJSON(`https://restcountries.com/v3.1/name/russia`),
    getJSON(`https://restcountries.com/v3.1/name/usa`),
    Promise.reject('error❗'),
  ]);

  console.log(resolve.flat());
};
// promiseAllSettled();

// Promise.allSettled ( [………] )
const promiseAny = async function () {
  // اولین پرامیس فول فیل شده رو ریترن میکنه و کلا ریجکت شده رو نادیده میگیره - مثل ریس هست ولی ریجکتارو نادیده میگیره
  const resolve = await Promise.any([
    getJSON(`https://restcountries.com/v3.1/name/germany`),
    getJSON(`https://restcountries.com/v3.1/name/russia`),
    getJSON(`https://restcountries.com/v3.1/name/usa`),
    Promise.reject('error❗'),
  ]);

  console.log(resolve.flat());
};
// promiseAny();

// fetch function argument_________________________________________
/* هست و ورودی دوم ابجکت ویژگی ها URL این تابع 2 تا ورودی میگیره که ورودی اول

fetch( URL , { method , body , headers , signal , credentials , mode , cache } ) با تمام ورودی ها
fetch( URL , { method , body , headers } ) رایج ترین ورودی ها

? method = نوع درخواستی که میزنیم
method: 'GET'     
method: 'POST'    
method: 'DELETE'  
method: 'PUT'     
method: 'PATCH'   

? body = دیتایی که می‌فرستی به بک‌اند
body: JSON.stringify({ title: 'React Course' , price: 100 })
body: JSON.stringify({ title: 'React Course' , date: '2026-02-04' })
مجاز نیست GET برای

? headers = اطلاعات اضافی درباره درخواست
به بک‌ اند میگه این دیتا چیه؟ از کی اومده؟ چجوری بخونمش؟

ارسال جیسون - بیشتر این استفاده میشه 
headers: { 'Content-Type': 'application/json' }

برای احراز هویت 
headers: { 'Authorization': 'Bearer TOKEN' }
*/

// GET_________________________________________
// (پیش‌فرض) گرفتن دیتا
async function getData(userData) {
  const res = await fetch(`http://URL/data${userData}`);

  if (!res.ok) throw new Error('error occurred !');

  return res.json();
}

// POST_________________________________________
// ساختن دیتا
async function sendData(userData) {
  const res = await fetch(`http://URL/data`, {
    method: 'POST',

    body: JSON.stringify(userData),

    headers: { 'Content-Type': 'application/json' },
  });

  if (!res.ok) throw new Error('error occurred !');

  return res.json();
}

// DELETE_________________________________________
// حذف
async function deleteData(id) {
  const res = await fetch(`http://URL/data/${id}`, {
    method: 'DELETE',
  });

  if (!res.ok) throw new Error('error occurred !');

  return res.json();
}

// PUT_________________________________________
// جایگزینی کامل
async function updateData(id, userData) {
  const res = await fetch(`http://URL/data/${id}`, {
    method: 'PUT',

    body: JSON.stringify(userData),

    headers: { 'Content-Type': 'application/json' },
  });

  if (!res.ok) throw new Error('error occurred !');

  return res.json();
}

// PATCH_________________________________________
// ویرایش جزئی
async function updateData(id, userData) {
  const res = await fetch(`http://URL/data/${id}`, {
    method: 'PATCH',

    body: JSON.stringify(userData),

    headers: { 'Content-Type': 'application/json' },
  });

  if (!res.ok) throw new Error('error occurred !');

  return res.json();
}

// signal = برای کنسل کردن درخواست
// const controller = new AbortController()
async function fetchAPI() {
  const res = await fetch(URL, { signal: controller.signal });

  if (!res.ok) throw new Error('error occurred !');

  const data = await res.json();

  return data;
}

/*
? credentials = برای کوکی و احراز هویت
credentials: 'include' ارسال کوکی
credentials: 'same-origin'
credentials: 'omit'

fetch('/profile', { credentials: 'include' })
*/
```
