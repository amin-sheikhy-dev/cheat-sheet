```js
// Default Parameters_________________________________________

const bookings = [];

const createBooking = function (flightNum, numPassengers = 1, price = 199 * numPassengers) {
  const booking = {
    // استفاده میکنیم Enhanced از
    flightNum,
    numPassengers,
    price,
  };

  bookings.push(booking);
};

createBooking('LH123');
createBooking('LH123', 2, 800);
createBooking('LH123', 2);
createBooking('LH123', 5);
// اگه بخاییم ورودی دوم اسکیپ کنیم
createBooking('LH123', undefined, 1000);

// How Passing Arguments Works (Primitive Vs Reference)_________________________________________

const flight = 'LH234';

const aminSheikhy84 = {
  name: 'amin sheikhy',
  passport: 2348729384,
};

const checkIn = function (flightNum, passenger) {
  flightNum = 'LH999';

  passenger.name = 'Mr.' + passenger.name;

  passenger.passport === 2348729384 ? 'Checked in ✅' : 'Wrong passport! ❌';
};

checkIn(flight, aminSheikhy84);

flight; // تایپ هست حتی با اینکه داخل تابع مقدارشو عوض کردیم ولی عوض نشد Primitive اینجا مقدار متغیر ما تغییر نکرده چون
aminSheikhy84; // تایپ هستش و مقدارش عوض میشه Reference ولی اینجا مقدار ابجکت نیم ما عوض شد وقتی داخل تابع رفت چون

// (First Class) & (Higher Order) Functions_________________________________________

// میشود upperFirstWord و oneWord دارد که شامل تابع های First Class هست که داخل خودش دو تابع Higher Order یک تابع transformer تابع
// OneWord : First Class function
const oneWord = function (str) {
  return str.replace(/ /g, '').toLowerCase();
};
// UpperFirstWord : First Class function
const upperFirstWord = function (str) {
  const [first, ...others] = str.split(' ');
  return [first.toUpperCase(), ...others].join(' ');
};

// Higher Order function
const transformer = function (str, fn) {
  return `Your String: ${fn(str)} - By: ${fn.name}`;
  // fn.name = اسم فانکشن رو با متد نیم میشه دراورد
};

transformer('JavaScript is the best !', upperFirstWord);
// Your String: JAVASCRIPT is the best !
// By: upperFirstWord

transformer('JavaScript is the best !', oneWord);
// Your String: javascriptisthebest!
// By: oneWord

// میشه call هست High Order function که خودش یک addEventListener هست که توسط CallBack function درواقع یک high الان
const high = function () {
  return '🖐️';
};

document.body.addEventListener('click', high);

// functions Retuning functions_________________________________________

const greet = function (greeting) {
  return function (name) {
    return (greeting, name);
  };
};

const greeterHey = greet('Hey');
greeterHey('amin');
greeterHey('jonas');
greet('Hello')('amin');

// ها Arrow function همون مثال با
// چون توی ارو فانکشن ریترن همیشه لازم نیس پس میشه کلین کد زد
const greetArr = (greeting) => (name) => `${greeting} ${name}`;

// (Call , Apply , Bind) Methods_________________________________________

const lufthansa = {
  airline: 'lufthansa',

  iataCode: 'LH',

  bookings: [],

  book(flightNum, name) {
    `${name} booked a seat on ${this.airline} flight ${this.iataCode}${flightNum}`;

    this.bookings.push({ flight: `${this.iataCode}${flightNum}`, name: name });
  },
};

lufthansa.book(239, 'Jonas Schmedtmann');
lufthansa.book(635, 'amin sheikhy');

const eurowings = {
  airline: 'Eurowings',

  iataCode: 'EW',

  bookings: [],
};

const book = lufthansa.book; // یه تابع مستقل شده و داخل هیچ ابجکتی نیس book حالا

//  book(23, 'Sarah Williams')
// ست میشه چون ابجکت نداره برای خودش undifined اش روی this key word کار نمیکنه چون

// Call method_________________________________________

// function.call ( this Object, Argument 1, Argument 2, ……… )
// ست میکنه eurowings رو روی book فانکشن this key word متد کال درواقع
book.call(eurowings, 23, 'sara williams'); // {airline: 'Eurowings', iataCode: 'EW', bookings: [{…}, {…}]}

// هم میشه کرد lufthansa همین کارو برای
book.call(lufthansa, 127, 'jamze miller');
// {airline: 'lufthansa', iataCode: 'LH', bookings: [{…}, {…}, {…}, {…}, {…}], book: ƒ}

// دیگه airLine با یک
const swiss = {
  airline: 'Swiss',

  iataCode: 'LX',

  bookings: [],
};

book.call(swiss, 583, 'Mary Cooper');

// Apply method_________________________________________

// function.bind ( this Object, [Argument 1, Argument 2, ……… ] )
// هستش فقط با این تفاوت که ورودی خودش ارایه میگیره Call مثل متد
const flightData = [234, 'Gray Wilson'];
book.apply(swiss, flightData); // ورودی دوم فقط ارایه قبول میکنه

book.apply(swiss, [174, 'jamze wisler']);

// Bind method_________________________________________

// function.bind ( this Object, Argument 1, Argument 2, ……… )
// اش روی اون ابجکتی ست شده که مشخص کردیم this key word مثل متد کاله فقط با این تفاوت که نمیاد فانکشن رو همونجا کال بکنه بجاش فانکشن جدید ریترن میکنه که
const bookLH = book.bind(lufthansa);
const bookEW = book.bind(eurowings);
const bookLX = book.bind(swiss);

bookEW(231, 'linkon baroz');
bookLX(19, 'jakoob watson');

// رو باید بریم name و دیگه هر پروازی بدیم فقط ورودی دوم یعنی FlightNum الان ما اینجا ورودی 23 رو دادیم به عنوان
const bookLH23 = book.bind(lufthansa, 23);
bookLH23('albert hawking');
bookLH23('robert oppenheimer');

// partial application with bind method_________________________________________

// ساخت تابعی که 10 درصد مالیات بگیره
const addTax = (rate, value) => value + value * rate;
addTax(0.1, 250); // 275

// میشه تابع های جدید با ورودی های ثابت ساخت bind در واقع با متد
// حالا میخاییم مقدار ثابت مالیات روی 0.23 ست بشه
const addVat = addTax.bind(null, 0.23);
addVat(80); // 98.4

// Immediately Invoked Function Expressions (IIFE)_________________________________________

// یعنی یک فانکشن کلا یه بار اجرا شه بعدش دیگ اجرا نشه
// به این صورته که کل تابع رو داخل پرانتز میزاری و پرانتز هم جلوش میزاری واسه ران شدن

(function () {
  'This will never run again';
})();

(() => 'This will ALSO never run again')();

// Closure_________________________________________

function outer() {
  let name = 'Amin';

  setTimeout(() => {
    console.log(name);
  }, 1000);
}

outer();

// رو اپدیت کنه passenger و وقتی نتیجه رو داخل کنسول ببینیم متوجه میشیم ک تونسته مقدار
// تموم شده CallStack وجود نداره چون اجرا شدنش توی secureBooking در صورتی که متغیر
// به عبارت دیگه کلوژر باعث میشه که یک فانکشن بتونه به تمام متغیر هایی که اون فانکشن در اونجا به وجود امده دسترسی داشته باشه
// بوده Booker محل تولد فانکشن secureBooking در واقع میشه گفت

// مثال دیگه
function passengers(passengersNum, time) {
  `will start boarding in ${time} second`;

  // هنوز در حال اجراس و به متغیر های فانکشن پرنتش دسترسی داره setTimeout اش اجرا شده و تموم شده ولی تابع EC ما passengers اینجا تابع

  function boarding() {
    return `we are now boarding all ${passengersNum} passengers`;
  }

  boarding();
}
passengers(180, 3);

// مثال

function outer() {
  let count = 0;

  function inner() {
    count++;

    console.log(count);
  }

  return inner;
}

const counter = outer();

counter();
counter();
counter();

// کاربردش خصوصی کردن دیتا ها

function createCounter() {
  let count = 0;

  return {
    increment() {
      count++;
    },

    getCount() {
      return count;
    },
  };
}

const counter = createCounter();

counter.increment();

counter.getCount();

counter.count; // ارور میده و نمیشه بهش دسترسی داشت
```
