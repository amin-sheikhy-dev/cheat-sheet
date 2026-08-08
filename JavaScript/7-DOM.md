```js
document.querySelector('.element');
// برای انتخاب یک کلس نقطه . میزاریم دقیقا مثل انتخاب کردن تو سی سی اس میمونه

// انتخاب عنصر ایدی دار
// تکست کانتنت محتوای متنو میده
document.querySelector('#element').textContent;

// برای انتخاب مقادیر اینپوت ها
document.querySelector('.element').value = 23;

element.addEventListener(type, listener);
// type =   "click"   "keydown"   "submit"   "mouseenter"

document.querySelector('.element').addEventListener('click', () => {
  // اگه روی عنصر اچ تی ام ال کلیک شد و بعدش عملی که خواستی انجام بشه
});

// دستکاری کردن استایل های سی اس اس
// اول المنت سلکت میشه و بعدش استایل بعدشم اون استایلی که خاستی
// هر زمان بخاییم استایل دستکاری کنیم باید استرینگ وارد کنیم
document.querySelector('.element').style.backgroundColor = '#fff9c4';

// فوکوس خودکار روی اون اینپوت یا چیزی ک میخاییم
document.querySelector('.element').focus();

// همیشه اولین عنصری که این کلاسو داره سلکت میکنه اگه بخای همرو انتخاب کنه باید آل بدی بهش
document.querySelector('.element');

// به ما یک نودلیست میدهد که رفتاری شبیه ارایه دارد
document.querySelectorAll('.element');

for (let i = 0; i < element.length; i++) {
  console.log(element[i].textContent); // یعنی میشه توش لوپ کرد
}

// همیشه یادت باشه به کلس لیست ها نقطه اول استرینگ ندی
// اولین عنصری که کلاس مدال داره رو سلکت کن برو داخل لیست کلاس های و کلاس هیدن رو حذف کن
document.querySelector('.modal').classList.remove('hidden'); // حواست باشه یوقت نقطه نزاری توش

// مثال
<div class="box red big square">......</div>;

const box = document.querySelector('.box');

box.classList.add('circle'); // اضافه می‌کنه
box.classList.remove('red'); // حذف می‌کنه
box.classList.toggle('big'); // اگر بود حذف می‌کنه اگر نبود اضافه می‌کنه
box.classList.contains('square'); // true یا false

/*
| Event      | زمانی اجرا می‌شه                                         |
|============|========================================================|
|  keydown   |      وقتی کاربر یه کلید فشار می‌ده(همون لحظه)             |
|  keypress  | تا زمانی که کلید فشرده باشد داعما اجرا میشه و یکمی قدیمیه  |
|  keyup     |                 وقتی کاربر کلید رو رها می‌کنه             |
|============|======================================================|*/

document.addEventListener('keydown', (e) => {
  // هروقت دکمه ای کلیک شه جاوااسکریپت یه ابجکت ارسال میکنه ک اینجوری میتونی ببینش
  console.log(e);

  // حالا از اون ابجکت میاییم مقدار کلید رو برمیداریم تا بفهمیم چی کلیک شده
  console.log(e.key);

  // هروقت اسکیپ کلیک شد این تو کنسول تایپ میشه
  if (e.key === 'Escape') console.log('Escape clicked');
});

// عوض کردن سورس یک المنت
document.querySelector('.element').src = '';

document.querySelector('.Element').addEventListener('click', (e) => {
  e.preventDefault();
  // چون رفتار دیفالت مرورگر اینه وقتی یه فرم سابمیت میشه صفحه ریلود شه پس ما اینو میزاریم اینجوری نشه
});

element.insertAdjacentHTML(position, text);
// کد بنویسی HTML رو نسبت به یه المنت ایجاد کنی بدون اینکه دستی توی HTML باعث میشه خیلی راحت کد

/*
position:
  "beforebegin"  →  قبل از خود المنت
  "afterend"     →  بعد از خود المنت
  "afterbegin"   →  به عنوان اولین فرزند (داخل المنت  ابتدای آن)
  "beforeend"    →  به عنوان آخرین فرزند (داخل المنت  انتهای آن)

text: → که میخای تولید شه رو داخل یک استرینگ مینویسی و میدی بهش HTML تگ های
*/

const divTag = document.querySelector('.divTag');

// .createElement('')_________________________________________

// و اضافه کردن کلس و ست کردن متن براش EL ساخت یک
const message = document.createElement('button');
message.classList.add('el');
message.textContent = 'fake EL';

// .prepend()_________________________________________

// این متد اون المنتی که ساخته بودیم رو به عنوان اولین چایلد اون تگ ست میکنه
divTag.prepend(message);

// .append()_________________________________________

// دقیقا مثل متد بالا فقط به عنوان چایلد اخر میده اینو

// .cloneNode( Boolean )_________________________________________

divTag.append(message.cloneNode(true));
// کپی ساختیم از همون EL حالا دقیقا یه
// وقتی ترو بدیم یعنی تمام چایلد هاش رو هم کپی میکنه

// رو جا به جا کنند HTML میتونن المنت های موجود در یک صفحه append و prepend اینو 2 تا متد

// .after()_________________________________________

// .before()
// قبل و بعد خود تگ میزارن before و after متد
divTag.after(message.cloneNode(true));
divTag.before(message.cloneNode(true));

// اینه که داخل نسبت به خود تگ جا به جا میکنن prepend,append با متد های after,before تفاوت متد
// داخل خود تگ جا به جا میکنن prepend و append ولی متد های

// .remove()_________________________________________

// حذف یک المنت
document.querySelector('.el3').addEventListener('click', () => document.querySelector('.el3').remove());

// Attributes_________________________________________

<meta
  class="meta"
  atr1="contentAtr1"
  id="metaID"
  name="viewport"
  content="width=device-width, initial-scale=1.0"
  data-version-number="3.0"
  data-user-id="123"
  data-role="admin"
/>;

// HTML گرفتن اتریبیوت ها از
const Element = document.querySelector('.meta');
Element.name; // viewport
Element.content; // width=device-width, initial-scale=1.0
Element.className; // Element

// عوض کردن یه اتریبیوت
Element.id = 'ID-mata';
Element.id; // ID-mata

// صحیح نیست یعنی یچیزی از خودت تعریف کردی html گرفتن اتریبیوت اشتباه که توی سند
Element.atr1; // undefined

// ولی با این میشه
Element.getAttribute('atr1'); // contentAtr1

// ساخت اتریبیوت و ست کردن مقدار براش
Element.setAttribute('atr2', 'contentAtr2'); // ورودی اول اتریبیوت و دومی مقدارش

// Data attributes_________________________________________

<button data-version-number="3.0" data-user-id="123" data-role="admin"></button>;
Element.dataset.versionNumber; // 3.0
Element.dataset.userId; // 123
Element.dataset.role; // admin

// Classes_________________________________________

Element.classList.add('c', 'j');
Element.classList.remove('c', 'j');
Element.classList.toggle('c'); // چک میکنه اگه بود حذف کنه نبود اضافه کنه
Element.classList.contains('c'); // ریترن میکنه false یا true در ارایه ها عمل میکنه یعنی includes شبیه

// Position Element_________________________________________
const component10 = document.querySelector('.component10');
const wlcElement = document.querySelector('.wlc');
const El2 = document.querySelector('.el2');
const El6 = document.querySelector('.el6');

// Scroll To Position_________________________________________

// این متد ابجکت مشخصات کلی المنت را میدهد که شامل طول و عرض و موقعیت آن در صفحه است
component10.getBoundingClientRect();

{
  bottom: 3768.78125;
  left: 286.5;
  right: 592.5;
  top: 3705.984375;

  height: 62.796875;
  width: 306;

  x: 286.5;
  y: 3705.984375;
}

// اسکرول به یک کامپوننت خاص
El2.addEventListener('click', () => component10.scrollIntoView({ behavior: 'smooth' }));
component10.addEventListener('click', () => wlcElement.scrollIntoView({ behavior: 'smooth' }));

// الان با کلیک این 1400 پیکسل میریم پایین
El6.addEventListener('click', () => window.scrollTo({ top: 1400, behavior: 'smooth' }));
// left: 0 , right: 0  پراپرتی چپ و راست هم میگیره ولی زیاد استفاده نمیشه

// نشان میدهد کاربر در چه موقعیتی از سایت قرار داره و چقدر اسکرول کرده
window.scrollY;
window.scrollX;

// برای فهمیدن عرض و ارتفاع صفحه کاربر
window.innerHeight;
window.innerWidth;

// برای فهمیدن کل ارتفاع سایت
document.documentElement.scrollHeight;

// requestAnimationFrame( () => {} )
// یک تابع جاوااسکریپتی برای هماهنگ کردن انیمیشن‌ها با فریم ریت صفحه است requestAnimationFrame
// مرورگر خودش زمان‌بندی می‌کنه که فقط یک بار در هر فریم اجرا شود

let ticking = false;

window.addEventListener('scroll', () => {
  if (!ticking) {
    window.requestAnimationFrame(() => {
      // console.log('Scroll position:', window.scrollY);
      ticking = false;
    });

    ticking = true;
  }
});

// Scroll Event & Intersection Observer_________________________________________

// Observer
const observerCallBack = (entries, observer) => {
  // این کلا یه ارایه میده که یه عضو داره
  entries; // [IntersectionObserverEntry]

  const [entry] = entries;

  entry;
  entry.isIntersecting; // threshold نشون میده طبق false یا true بهمون
  entry.intersectionRatio; // درصد همپوشانی رو میگه
  entry.target; // خود المنت رو میده

  observer.unobserve(entry.target); // باعث میشه هیچکدوم از سکشن های دوباره ما ابسرو نشن و به پرفورمنس کمک میکنه
};

const observerOptions = {
  root: null, // بدیم نسبت به ویوپرت بررسی میکنه null رو بهش بدیم که اگه null بهش بدیم یا el میتونیم یک

  threshold: 0, // اجرا بشه CallBack Fn به درصد هست و میخاییم بگیم چند درصد هم پوشانی کردن
  // و اکثر مواقع هم بهش 0 میدیم یعنی همپوشانی با ویو پورت وقتی صفر شد کد اجرا شه

  rootMargin: '-90px', // باعث میشه هدر ما 90 پیکسل کوتاه تر شه و زودتر بچسبه
  // برسه و بچسبه threshold در واقع با کوتاه کردنش باعث میشیم همپوشانی زودتر به عدد
  // روت مارجین اجباری نیس ولی دوتای اول اجباریه
};
// اندازه هر چند درصد که نوشتیم اونجا هم پوشانی داشتن اجرا میشه root با el1 هرموقع

// شد یا خارج شد) فراخوانی می‌شود viewport تابعی که وقتی وضعیت عنصر تغییر کرد (مثلاً وارد callback
// callback تنظیماتی برای تعیین چگونگی و زمان فعال شدن options
const observingSection = new IntersectionObserver(observerCallBack, observerOptions);

// بشه رو انتخاب میکنیم observe حالا عنصری که میخاییم
// observingSection.observe(document.querySelector('.el1'))

// Event Propagation in Practice - Bubbling & Capturing_________________________________________

// el1 > el2 > el3 فرض کنیم ترتیب چایلد و پرنت المنت ها
// که چایلد همه هست کلیک کنی روی بقیه هم اعمال میشه el3 تو الان روی

document.querySelector('.el3').addEventListener('click', (e) => {
  this.style.backgroundColor = ''; // el3 دیس درواقع به المنتی اشاره میکنه که روش ایونت زدی یعنی میشه

  // ( e.currentTarget === this === .el3 ) هست this در واقع همون e.currentTarget

  // اگه فقط بخایی روی اون المنت که کلیک شده اثر کنه و روی پرنت هاش اثر نکنه
  e.stopPropagation();
});

document.querySelector('.el2').addEventListener('click', (e) => {
  this.style.backgroundColor = ''; // کلیک شه این هم اعمال میشه el3 روی

  // ( e.target ) دقیقا عنصری که روش کلیک شده رو میده

  // ( e.currentTarget ) به المنتی که روی ایونت لیستنر زدی اشاره میکنه
});

document.querySelector('.el1').addEventListener('click', (e) => {
  this.style.backgroundColor = ''; // ایناس parent کلیک شه این هم اعمال میشه چون el2 و el3 روی

  // console.log( e.target ) دقیقا عنصری که روش کلیک شده رو میده

  // console.log( e.currentTarget ) به المنتی که روی ایونت لیستنر زدی اشاره میکنه
});

// مشترک هستن parent ها که دارای یک child ست کردن ایونت برای
//* el1 > el2 > el3 فرض کنیم ترتیب چایلد و پرنت المنت ها
document.querySelector('.el1').addEventListener('click', (e) => {
  e.preventDefault();

  e.target; // این دقیقا عنصری رو میده که روش کلیک شده

  if (e.target.classList.contains('.el2')) console.log('clicked');
  // با این شروط چک میکنیم که توی کلاس هاشون المنت 3 یا 2 باشه
  if (e.target.classList.contains('.el3')) console.log('clicked');
});

// Dom Traversing_________________________________________

// .firstElementChild , .lastElementChild
// اخرین و اولین چایلد
divTag.firstElementChild.style.color = '';
divTag.lastElementChild.style.color = '';

// .parentElement
// رو میده divTag به ما - parent گرفتن اولین
document.querySelector('.el1').parentElement;

// .closest()
// این متد از خود اون المنت شروع میکنه میره بالا تا به یه المنتی برسه که کلاس مورد نظر رو داشته باشه
document.querySelector('.el1').closest('.divTag').style.background = '';

// .previousElementSibling , .nextElementSibling
// سیبلینگ بعدی و قبلی
document.querySelector('.el3').previousElementSibling; // رو میده el2
document.querySelector('.el3').nextElementSibling; // رو میده el4

document.querySelector('.el3').previousSibling; // #text
document.querySelector('.el3').nextSibling; // #text

// Lifecycle Dom Events_________________________________________

// بهش میگیم وقتی عکس لود شد این کلاس رو حذف کن
document.querySelector('.el1').addEventListener('beforeunload', () => entry.target.classList.remove('lazy-img'));

// Mouse Hover_________________________________________

const el4 = document.querySelector('.el4');
function handleEvent() {}

el4.addEventListener('mouseover', handleEvent);
el4.addEventListener('mouseout', handleEvent);
// فرق بالایی با پایینی اینه که بالایی بابل میشه
el4.addEventListener('mouselive', handleEvent);
el4.addEventListener('mouseenter', handleEvent);

el4.removeEventListener('eventName', handleEvent); // حذف ایونت
```
