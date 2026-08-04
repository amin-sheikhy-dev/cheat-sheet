```js
const restaurant = {
  name: 'Classico Italiano',

  location: 'Via Angelo Tavanti 23, Firenze, Italy',

  categories: ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'],

  starterMenu: ['Focaccia', 'Bruschetta', 'Garlic Bread', 'Caprese Salad'],

  mainMenu: ['Pizza', 'Pasta', 'Risotto'],

  openingHours: {
    thu: { open: 12, close: 22 },

    fri: { open: 11, close: 23 },

    sat: { open: 0, close: 24 },
  },

  // اسم این تابع سفارش هست و  قراره عدد بدیم بهش سفارشو بده و توی ریترن از ارایه استفاده کردیم
  order: function (starterIndex, mainIndex) {
    return [this.starterMenu[starterIndex], this.mainMenu[mainIndex]];
  },

  // به این تابع باید ابجکت بدی با این مقادیری که توی ارگومانش هست
  // و مقدار پیش فرض هم داریم بهشون میدیم اگه کاربر چیزی وارد نکرد چجور باشه
  orderDelivery: function ({ time = '20:00', adress = 'no adress', mainIndex = 0, starterIndex = 0 }) {
    return `Order Received! ${this.starterMenu[starterIndex]} and ${this.mainMenu[mainIndex]} will be delivered to ${adress} at ${time}`;
  },

  // ساخت تابعی ک میگه پاستای شما همراه با چه چیزی است
  orderPasta: function (ing1, ing2, ing3) {
    return `Your pasta arrived with ${ing1}, ${ing2} and ${ing3}`;
  },

  orderPizza: function (mainIngredient, ...otherIngredient) {
    return `Your pizza arrived with ${mainIngredient} and ${otherIngredient}`;
  },
};

restaurant.orderPizza('Mushrooms', 'olives', 'onions', 'thyme');

// به ابجکت هایی ک تحت عنوان ورودی به تابع داده میشن اپشن گفته میشه
restaurant.orderDelivery({
  time: '14:3',
  adress: 'mehrabi street, kooche 6',
  mainIndex: 2,
  starterIndex: 2,
});

// الان تو این فقط ادرس رو دادیم و قراره از مقادیر پیش فرض تابع استفاده کنیم
restaurant.orderDelivery({});
restaurant.orderDelivery('');

// Destructuring array_________________________________________

const array = [1, 2, 3];

const [one, two, three] = array;
// one = 1 , two = 2 , three = 3

// میخام فقط مقادیر اول و دوم رو از گتگوری بکشم بیرون
const [first, second] = restaurant.categories;

// حالا اگه فقط مقادیر اول و سوم رو بخاییم
const [first2, , second2] = restaurant.categories; // مقدار دومی رو اسکیپ کردیم در واقع

// Switching Variable with Destructuring in array
let [main, secondery] = restaurant.categories; // الان مقدار اولی اصلیه و دومی ثانویه

// حالا اگه بخاییم جای اون دوتا رو با هم عوض کنیم باید یعنی دومی بیاد اصلی و اصلی بره دومی

// روش برای جا به جایی
[main, secondery] = [secondery, main];

// Nested array Destructuring
// دی استراکچرینگ ارایه های تو در تو
const nestedArr = [2, 3, [5, 6]];

const [minimal, , nested] = nestedArr;
// minimal = 2 , nested = [5, 6]

// الان با این کار حتی ارایه های داخل ارایه هم اسم دارن
const [minimall, , [nested1, nested2]] = nestedArr;
// minimal = 2 , nested1 = 5 , nested2 = 6

// Default value for array values
// بدون مقدار دیفالت
const [ali, mamad, javad] = ['name1', 'name3'];
// ali = 'name1' , mamad = undefined , javad = 'name3'

// الان همه مقادیر یه مقدار دیفالت دارن
const [hassan = 'name1', ahmad = 'name2', kiarash = 'name3'] = ['one name', 'two name'];

// Destructuring Object_________________________________________

const { name, openingHours, categories } = restaurant;

// مقادیر با اسم های دلخواهمون ذخیره شه
const { name: restaurantName, openingHours: hours, categories: tags } = restaurant;

// Default value for Object
// باعث میشه موقع گرفتن ای پی آی اگر متدی وجود نداشت بهمون اندیفایند تحویل نده
const { Menu = [], starterMenu: starters = [] } = restaurant;

/* 
  openingHours: {
    thu: { open: 12, close: 22 },

    fri: { open: 11, close: 23 },

    sat: { open: 0, close: 24 },
  },
*/

const {
  fri: { open: o, close: c }, // میخام اپن و کلوز رو رو از روز فرایدی و ابجکت ساعات باز بودن بردارم و مقدار دلخواه براشون ست کنم
} = openingHours;
// o = 11
// c = 23

// Spread Operator_________________________________________

// وظیفه‌ش اینه که محتوای یک آرایه یا شیء رو باز کنه و جدا جدا وارد جای جدیدی کنه

const array2 = [7, 8, 9];
// ...array2 = 7 , 8 , 9

const newArray = [1, 2, ...array2];
// newArray = [1, 2, 7, 8, 9]

// مثال دیگر
const minMaxArr = [12, 23, 45, 6, 75, 1, -1, 0, -32];
Math.max(...minMaxArr); // 75
Math.min(...minMaxArr); // -32

// کار روی استرینگ
const str3 = 'amin';
const letters = [...str3, '', 'S'];
// letters = ['a', 'm', 'i', 'n', '', 'S']

// ساخت رستوران جدید با مقادیر رستوران قبلی و اضافه کردن یه سری چیزا
const newRestaurant = { ...restaurant, founded: 1390, founder: 'amin sheikhy' };

// Rest Operator_________________________________________

// توی اسپرید اپریدور سه نقطه سمت راست مساوی قرار میگرفت ولی توی رست اپریدور سه نقطه سمت چپ قرار میگیره
// رست برعکس اسپرید از مقادیر یه ارایه یا ابجکت میسازه

const arr2 = [0, 2, 3, ...[4, 8]]; // الان این سه نقطه سمت راست مساویه

const [A, B, ...others] = [2, 3, 4, 5, 6, 7, 8];
// A = 2 , B = 3 , others = [4, 5, 6, 7, 8]

/* 
  openingHours: {
    thu: { open: 12, close: 22 },

    fri: { open: 11, close: 23 },

    sat: { open: 0, close: 24 },
  },
*/
// استفاده ازش تو ابجکت ها
const { sat, ...weekdays } = restaurant.openingHours;
// sat = {open: 0, close: 24}
// weekdays = {thu: {…}, fri: {…}}

// ساخت تابعی ک هر چقدر عدد به عنوان ورودی دادی جمع کنه
// این تابع الان از رست اپریدور استفاده کرد تا تمام ورودی هارو داخل ارایه نامبرز جمع کنه
const add = function (...numbers) {
  return numbers;
};
add(2);
add(2, 3, 6);
add(7, 8, 4, 2, 5, 6, 7);
const G = [22, 33, 77];
add(...G); // اگه بخاییم به فانکشن ارایه بدیم باید قبلش ارایه رو باز کنیم با اسپرید اپریدور

// Short Circuiting (AND: &&)  (OR: ||)_________________________________________

//  OR: ||
// از سمت چپ شروع میکنه میره سمت راست اولین مقدار ترو ک بخوره اون رو ریترن میکنه و اگه همه مقادیر فالس بودن اخرین مقدار فالس رو ریترن میکنه
// هرکدوم از مقادیر اگه سمت چپیش درست باشه به سمت راستی نگاه نمیکنه اصلا
// اوز زمانی فالس ریترن میکنه که تمامی مقادیرش فالس باشن پس اگه یکیش ترو باشه همونو ریترن میکنه

3 || 'jonas';
// 3

'' || 'jonas';
// jonas

true || 0;
// true

undefined || null;
// null

false || 0;
// 0

undefined || 0 || '' || 'Hello' || 23 || null;
// Hello

//  AND: &&
// در کل از سمت چپ شروع میکنه میره سمت راست اولین مقدار فالسی ک بخوره اون رو ریترن میکنه اگه همه مقادیر ترو بودن اخرین مقدار ترو رو ریترن میکنه
// هرکدوم از مقادیر اگه سمت چپیش غلط باشه به سمت راستی نگاه نمیکنه اصلا
// اند زمانی ترو ریترن میکنه که تمامی مقادیرش ترو باشن پس اگه یکیش فالس باشه همونو ریترن میکنه
// دقیقا برعکس اور عمل میکنه

// جوابارو حدس بزن
0 && 'jonas';
// 0

7 && 'jonas';
// 'jonas'

// این الان همینجوری به سمت راست حرکت میکنه تا یه ولیو درست غلط پیدا کنه
'Hello' && 23 && null && 'jonas';
// null

// Nullish Operator ( ?? )_________________________________________

// فقط دو مقدار اندیفایند و نال رو فالسی در نظر میگیره و بقیه چیزا مثل استرینگ خالی و عدد صفر رو فالسی ولیو در نظر نمیگیره
// && دقیقا مثل

0 ?? '' ?? 'hello' ?? 23;
// 23

// (AND , OR) Assignment Operators_________________________________________

const rest1 = {
  name: 'amin',
  numGuests: 0,
};

rest1.numGuests = rest1.numGuests || 10;
// حالا اگه تعداد مهمانان صفر باشه اینجوری 0 رو ریترن میکنه چون بالاخره مهمانان تعدادشون 0 هم میشه
rest1.numGuests = rest1.numGuests ?? 10;

// Coding Challenge_________________________________________

const game = {
  team1: 'Bayern Munich',

  team2: 'Borrussia Dortmund',

  players: [
    ['Neuer', 'Pavard', 'Martinez', 'Davies', 'Kimmich', 'Goretzka', 'Coman', 'Gnarby', 'Lewandowski'],

    ['Burkiy', 'Schulz', 'Hummels', 'Akanji', 'Haakiimi', 'Weiigl', 'Wittssel', 'Hazard', 'Braandt'],
  ],

  score: '4:0',

  scored: ['Lewandowski', 'Gnarby', 'Lewandowski', 'Hummels'],

  date: 'Nov 9th, 2037',

  odds: { team1: 1.33, x: 3.25, team2: 6.5 },
};

// گام 1 — ساخت آرایه بازیکنان برای هر تیم
const players1 = [...game.players[0]];
const players2 = [...game.players[1]];

// گام 2 — جدا کردن دروازه‌بان و بقیه بازیکنان تیم یک
const [gk, ...fieldPlayers] = [...game.players[0]];

// گام 3 — ترکیب همه بازیکنان در یک آرایه
const allPlayers = [...players1, ...players2];

// گام 4 — اضافه کردن 3 بازیکن جایگزین به تیم یک
const players1Final = [...players1, 'Thiago', 'Coutinho', 'Periscic'];

// گام 5 — استخراج ضرایب شانس از ابجکت
const { team1, x: draw, team2 } = game.odds;

// for-of Loop (Looping Array)_________________________________________

// کار مارو برای لوپ زدن روی ارایه ها اسون تر میکنه
const lettersArray = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j'];

// توی هر لوپ اعضای ارایه رو میده
for (const i of lettersArray) {
  i;
  // 'a'
  // 'b'
  // 'c'
  // 'd'
  // 'e'
  // 'f'
  // 'g'
  // 'h'
  // 'i'
  // 'j'
}

// این لوپ با لوپ بالایی هیچ فرقی نداره
for (let i = 0; i < lettersArray.length; i++) lettersArray[i];

// این متد روی یک ارایه اگه باشه برای هر عضو ارایه یه ارایه کوچک ایندکس و ولیو میده
// برای هر عضو ارایه یک ارایه دو عضوی جدید ریترن میکنه که هر کدوم از اون ارایه ها شامل
//  برای هر ارایه یک ایتریتور برمیگردونه که میشه این
for (const letter of lettersArray.entries()) {
  letter;
  // [0, 'a']
  // [1, 'b']
  // [2, 'c']
  // [3, 'd']
  // [4, 'e']
  // [5, 'f']
  // [6, 'g']
  // [7, 'h']
  // [8, 'i']
  // [9, 'j']
}

// میشه از دی استراکچرینگ هم استفاده کرد
for (const [index, value] of lettersArray.entries()) {
  (index, value);
  // 0    'a'
  // 1    'b'
  // 2    'c'
  // 3    'd'
  // 4    'e'
  // 5    'f'
  // 6    'g'
  // 7    'h'
  // 8    'i'
  // 9    'j'
}

// Enhanced Object Literals_________________________________________

const calcAge5 = (birthYear) => 1404 - birthYear;
const days = ['mon', 'tue', 'wed', 'thu', 'fri', 'sat', 'sun'];

const aminSheikhy = {
  // calcAge5: calcAge5  قبلا وقتی میخاستیم یه متد به ابجکت اضافه کنیم اینجوری مینوشتیم
  calcAge5, // ولی الان اینجوری هم میتونیم این یعنی یه ابجکت با همین اسم میسازه که با بیرونی یکیه دقیقا

  firstName: 'amin',

  lastName: 'sheikhy',

  // و یچیز دیگه اینکه میتونیم بجای اینکه براشون دستی اسم بنویسیم بیاییم از اکسپریشن ها یا محاسبات ها استفاده کنیم
  [days[0]]: 'monDay',
  [days[1]]: 'tueDay',

  age: 1404 - 1384,

  job: 'programmer',

  friends: ['matin', 'bahar', 'mmdReza'],

  // قبلا باید کلمه فانکشن رو مینوشتیم
  // calcAge6: function (birthYear) {
  //   return 1404 - birthYear;
  // },

  // ولی تو جاوااسکریپت 6 به بعد میتونیم کلمه فانکشن و دو نقطه رو حذف کنیم
  calcAge6(birthYear) {
    return 1404 - birthYear;
  },
};

// Optional Chaining ( ?.)_________________________________________

// داریم میگیم در ابجکت ساعات باز کردن روز مان وجود داره؟ اگه داشت ساعت باز بودنشو بردار اگه روز مان نبود بگو تو روز مان بستس کلا
// و این نالیش اپریدور باعث میشه نقدار اندیفایند نده به ما
restaurant.openingHours.mon?.open ?? 'The restaurant is not open on Mondays';

// این ارور میده چون از یه چیز اندیفاید نمیتونیم اپن رو برداریم
// restaurant.openingHours.mon.open

// ساعات باز بودن وجود داره؟ اگه داره روز مان وجود داره؟ اگه داشت ساعت باز بودنو بده حتی اگه یکیشم نبود بگو تو روز مان بستس کلا
restaurant.openingHours?.mon?.open ?? 'The restaurant is not open on Mondays';

// Looping Objects: Object Keys, Values, Entries_________________________________________

const openHour = {
  shanbe: { open: 8, close: 22 },

  yekShanbe: { open: 12, close: 23 },

  seShanbe: { open: 0, close: 24 },
};

const properties = Object.keys(openHour); // این الان از کلید های اون ابجکت یک ارایه به ما تحویل میده
// properties = ['shanbe', 'yekShanbe', 'seShanbe']

// این از ولیو های ابجکت یه ارایه به ما تحویل میده
const values = Object.values(openHour);
// values = [ {open: 8, close: 22} , {open: 12, close: 23} , {open: 0, close: 24} ]

// یک ابجکت رو به ارایه ای از ارایه ها تبدیل میکنه هر عضو ارایه خودش شامل ارایه اس ک هرکدوم از اونا مقدار اولش ولیو و مقدار دومش کلید هست
const entries = Object.entries(openHour);
// entries = [['shanbe', {open: 8, close: 22}] , ['yekShanbe', {open: 12, close: 23}] , ['seShanbe', {open: 0, close: 24}]]

// Set_________________________________________

// ست ها مثل ارایه ها ایتربل هستن
// هیچ راهی وجود نداره ما بتونیم مقادیر مختلف رو از ست ها بگیریم
// تکراری هارو حذف میکنه
const ordersSet = new Set(['Pasta', 'Pizza', 'Pizza', 'Risotto', 'Pasta', 'Pizza']);

// یه ست میتونه استرینگ هم باشه
new Set('Jonas');
// Set(5) {'J', 'o', 'n', 'a', 's'}

// .size
// میتونیم سایز ست هارو هم اندازه بگیریم
// این تعداد انواع رو میگه نه تعداد کل
ordersSet.size; // 3

// .has
// واسه اینکه ببینیم اون چیز هست یا نه
ordersSet.has('Pizza'); // true
ordersSet.has('Bread'); // false

// .add
// بخاییم ایتمی اضافه کنیم مثلا
ordersSet.add('Bread');
ordersSet.add('Bread');
ordersSet.add('Bread');
ordersSet.add('Bread');
// نون فقط یه بار اضافه شده با اینکه چند بار اضافه کردیم بخاطر اینکه ست تمامی مقادیرش یونیک هست

// .delete
// یک مقدار رو حذف کنیم
ordersSet.delete('Risotto');

// .clear
// این متد ست مارو کلا خالی میکنه
// ordersSet.clear()

// میتونیم روشون لوپ بزنیم
for (const i of ordersSet) i; // Pasta Pizza Bread

const staff = ['Waiter', 'Chef', 'Waiter', 'Manager', 'Chef', 'Waiter'];
// حالا مقادیر داخل ارایه رو میریزیم داخل ست
const staffUnique = new Set(staff);
// staffUnique = Set(3) {'Waiter', 'Chef', 'Manager'}

// حالا اگه بخاییم ست رو به ارایه تبدیل کنیم
// اسپرید اپریدور همونطور روی ست هم کار میکنه چون ست ها هم ایتربل هستن
const staffArray = [...new Set(staff)];
// staffArray = ['Waiter', 'Chef', 'Manager']

// میخاییم تعداد نوع حروفایی ک استفاده شده رو در بیاریم
const newSetStr = new Set('jonasschmedtmann').size; // 11

// Map_________________________________________

// مپ ها شبیه ابجکت ها هستن فقط میشه هرچیزیو به عنوای کلید داد بهشون

// .set
// اگه بخاییم ایتمی اضافه کنیم باید از متد ست اضافه کنیم
const rest = new Map();
rest.set('name', 'Classico Italiano');
rest.set(1, 'Firenze, Italy');
rest.set(2, 'Lisbon, Portugal');
rest; // Map(3) {'name' => 'Classico Italiano', 1 => 'Firenze, Italy', 2 => 'Lisbon, Portugal'}

// میشه چنتا ست پشت سر هم زد
rest
  .set('categories', ['Italian', 'Pizzeria', 'Vegetarian', 'Organic'])
  .set('open', 9)
  .set('close', 24)
  .set(true, 'We are open') // حتی میشه بولین هارو به عنوان کلید در نظر گرفت
  .set(false, 'We are closed');

// .get
// خب حالا وقتی بخاییم دیتایی رو از مپ بگیریم باید از متد گت استفاده کنیم
rest.get('categories'); // ['Italian', 'Pizzeria', 'Vegetarian', 'Organic']
rest.get('close'); // 24

// .has
// اگه بخاییم بفهمیم یه کلید داخل مپ ما هست یا ن از این متد استفاده میکنیم
rest.has('categories'); // true

// .delete
// حالا اگه بخاییم یه کلید رو حذف کنیم
rest.delete(2); // مثلا اینجا میخاییم لوکیشن دوم رو حذف کنیم

// .size
// اگه بخاییم ببینیم چنتا مقدار -کلید ولیو- داریم
rest.size; //7

// .clear
// اگه بخاییم مپ تخلیه و خالی شه
// rest.clear()

// حتی میشه ارایه رو به عنوان کلید داد
rest.set([1, 2], 'test');
// الان این به ما اندیفایند تحویل میده چون ادرسشون توی هیپ فرق میکنه و روش درست اضافه کردن ارایه به عنوان کلید نیست
rest.get([1, 2]); // undefined

// روش بهتر برای اضافه کردن ارایه به عنوان کلید
const r = [1, 2];
rest.set(r, 'TEST');
rest.get(r); // 'TEST'

// Maps Iteration_________________________________________

const question = new Map([
  ['question', 'What is the best programming language in the world?'],
  [1, 'C++'],
  [2, 'Python'],
  [3, 'JavaScript'],
  ['correct', 3],
  [true, 'Correct✅'],
  [false, 'Try again❌'],
]);

// میتونیم روی مپ لوپ بزنیم
for (const i of question) {
  i;
  // ['question', 'What is the best programming language in the world?']
  // [1, 'C++']
  // [2, 'Python']
  // [3, 'JavaScript']
  // ['correct', 3]
  // [true, 'Correct✅']
  // [false, 'Try again❌']
}

// قراره یه سوال بسازیم که با توجه به جواب کاربر پاسخ صحیح رو با پاسخ ها داخل مپ مقایسه کنه
question.get('question');

// میتونیم مقدار آی رو دی اسراکچر کنیم
for (const [key, value] of question) typeof key === 'number' && `answer ${key} : ${value}`;

const answer = Number(3);

question.get(question.get('correct') === answer);

// بخاییم کلید ها و ولیو های مپ رو در بیاریم
// این یک مپ ایتردور میسازه
// MapIterator {'question', 1, 2, 3, 'correct', true, false}
question.keys();
// MapIterator {'What is the best programming language in the world?', 'C++', 'Python', 'JavaScript', 3, 'Correct✅', 'Try again❌'}
question.values();

// روش بهتر اینه که تمام عناصرش رو باز میکنیم و اعضاشو داخل یه ارایه بریزی
[...question.keys()]; // ['question', 1, 2, 3, 'correct', true, false]
[...question.values()]; // ['What is the best programming language in the world?', 'C++', 'Python', 'JavaScript', 3, 'Correct✅', 'Try again❌']

// Coding Challenge_________________________________________

const gameEvents = new Map([
  [17, '⚽️ GOAL'],
  [36, '🔁 Substitution'],
  [47, '⚽️ GOAL'],
  [61, '🔁 Substitution'],
  [64, '🔶 Yellow card'],
  [69, '🔴 Red card'],
  [70, '🔁 Substitution'],
  [72, '🔁 Substitution'],
  [76, '⚽️ GOAL'],
  [80, '⚽️ GOAL'],
  [92, '🔶 Yellow card'],
]);

// چند نوع ایونت تو بازی اتفاق افتاده
const events = [...new Set([...gameEvents.values()])];
// ['⚽️ GOAL', '🔁 Substitution', '🔶 Yellow card', '🔴 Red card']

// overwive of strings methods_________________________________________

'amin'[0]; // a
// گرفتن یه حرف

'amin'.at(2); // i
'amin'.at(-2); // n
// گرفتن یه حرف و فرقش اینه میتونی منفی هم بدی بهش

'amin'.indexOf('m'); // 1
// توی کدوم جایگاه - کلمه هم میشه داد
// lastIndexOf مثل متد بالا ولی از اخر شروع میکنه

'amin'.slice(1, 3); // mi
// قسمتی رو قاچ میکنه و همونو میده بهت - اخری رو نمیده یعنی از 1 تا 2

'amin'.toUpperCase(); // AMIN
// با حروف بزرگ مینویسه

'AMIN'.toLowerCase(); // amin
// با حروف کوچک مینویسه

'   amin \n'.trim(); // amin
// میشه endLine متد کاربردی که باعث حذف اسپیس های اضافی و

'amin'.replace('n', 'r'); // amir
// تحویل میده amir جایگزین اولین حرف با دومین پارامتر یعنی الان بهمون
// replaceAll مثل متد بالا ولی با این تفاوت که همه رو جایگزین میکنه

'amin+sheikhy'.split('+'); // ['amin', 'sheikhy']
// اون علامت رو ورمیداره و حالا هرچی کلمه موند اونارو میریزه داخل ارایه

['amin', 'sheikhy'].join('+'); // amin+sheikhy
// هرچی کلمه داخل ارایه هست رو با اون علامت به هم میچسبونه

'amin'.padEnd(13, '^'); // amin^^^^^^^^
// چنتا ستاره یا همون علامتی که دادی اضافه کنه تا به 13 برسه - به اخرش اضافه میکنه
'amin'.padStart(13, '^'); // ^^^^^^^^amin
// چنتا ستاره یا همون علامتی که دادی اضافه کنه تا به 13 برسه - به اولش اضافه میکنه

'-amin-'.repeat(5); //-amin--amin--amin--amin--amin-
// یه استرینگ بساز که امین رو 5 بار تکرار کند

'amin'.endsWith('in'); // true
// پایانش با اینه؟ - بولین ریترن میکنه

'amin'.startsWith('am'); // true
// شروعش با اینه؟ - بولین ریترن میکنه

'amin'.includes('i'); // true
// ریترن میکنه false یا true چک میکنه وجود داره یا نه و

// .at( 'string' )_________________________________________

// گرفتن حرف یا کاراکتز مشخص از یه استرینگ
const plane = 'A320';
plane[0]; // A
plane[1]; // 3
plane[2]; // 2
'B737'[0]; // میتونیم این کارو مستقیم روی خود استرینگ هم انجام بدیم

const airline = 'TAP Air Portugal';

airline.length; // 16
// میتونیم حتی تعداد حروفش هم بگیریم که البته با اسپیس حساب میکنه

// .indexOf( 'string' ) .lastIndexOf( 'string' )_________________________________________

// اولین حرف آر توی کدوم پوزیشنه
airline.indexOf('r'); // 6

// اخرین حرف آر توی کدوم پوزیشنه
airline.lastIndexOf('r'); // 10

// میگرده دنبال اولین ایندکسی که این حروفا پشت هم تکرار شدن
airline.indexOf('Portugal'); // 8

// .slice( start, end )_________________________________________

// یه قسمت از استرینگی که بخای رو برمیداره قاچ میکنه همونو بهت میده
// این الان 4 5 6 رو میده و خود 7 حساب نیست
airline.slice(4, 7); // Air
// اگه پایانو بهش ندی از اون عدد تا اخرش قاچ میکنه
airline.slice(4); // Air Portugal
// یعنی اینا یه استرینگ جدید ریترن میکنه و به قبلی دست نمیزنه ولی اگه بخاییم ازش استفاده کنیم باید داخل متغیر ذخیرش کنیم

// پیدا کردن اولین کلمه
airline.slice(0, airline.indexOf(' ')); // TAP
// پیدا کردن اخرین کلمه
airline.slice(airline.lastIndexOf(' ') + 1); // Portugal

// پوزیشن منفی هم میشه داد
// منفی بزاری در واقع از سمت راست شروع میکنه شمردن
airline.slice(-2); // al
// از اولین حرف تا حرف منفی
airline.slice(0, -1); // TAP Air Portuga

// .toLowerCase() .toUpperCase()_________________________________________

// با حروف کوچک بنویسه یا حروف بزرگ
airline.toLowerCase(); // tap air portugal
airline.toUpperCase(); // TAP AIR PORTUGAL

// تعمیر یه کلمه از لحاظ حروف
const lastName3 = 'sHeiKhY';
const lastName4 = lastName3[0].toUpperCase() + lastName3.slice(1).toLowerCase(); // Sheikhy

// .trim( 'string' )_________________________________________

// این متد کاربردی هست و باعث میشه اسپیس ها حذف بشن
const loginEmail = '  hello@jonas.io \n';
const trimmedEmail = loginEmail.trim(); // 'hello@jonas.io'

// .replace( 'replaced' , 'target' )_________________________________________

// یه متدی هست چیزی رو بخاییم جایگزین کنیم
const priceGB = '288,97£';
// اولین ورودی داخل پرانتز رو با دومین ورودی داخل پرانتز جایگرین میکنیم
const priceUS = priceGB.replace('£', '$').replace(',', '.'); // 288.97$

// میخایم اسپیس هارو بگیریم
'mohammad amin sheikhy'.replaceAll(/ /g, ''); // mohammadaminsheikhy

// حتی میتونیم کلمات رو هم جایگزین کنیم
const announcement = 'All passengers come to boarding door 23. Boarding door 23!';
announcement.replace('door', 'gate');
announcement.replaceAll('door', 'gate'); // اگه متد آل اخرش اضافه کنیم همه کلمات جایگزین میشن نه فقط اولی

// .includes( 'string' ) .startsWith('string') .endsWith('string')_________________________________________

// متد هایی که بولین ریترن میکنن
const plane2 = 'Airbus A320neo';
// میگه وجود داره یا نه
plane.includes('A320'); // true
// شروعش با چی بوده
plane.startsWith('Air'); // true
// پایانش با چی بوده
plane.endsWith('neo'); // true

// .split( 'character' )_________________________________________

// این متد استرینگ رو با + تیکه تیکه میکنه و اونارو میزاره داخل ارایه
'a+very+nice+string'.split('+'); // ['a', 'very', 'nice', 'string']
'Jonas Schmedtmann'.split(' '); // ['Jonas', 'Schmedtmann']

// .join( 'character' )_________________________________________

// متد جوین با اون حرفی ک بهش دادیم ارایه رو تبدیل میکنه به استرینگ و اعضاشو به هم میچسبونه
['Jonas', 'Schmedtmann'].join(' '); // 'Jonas Schmedtmann'
['Jonas', 'Schmedtmann'].join('----'); // 'Jonas----Schmedtmann'

// .padStart( length , 'character' ) .padEnd( length , 'character' )_________________________________________

// حرف دیفالت این متد اسپیس هست یعنی چیزی بهش ندی خودش اسپیس مبده
// اولی تعداد کاراکتر چند تا باشه و دومی میگه چی اضافه کنه تا برسه به اون تعداد
'amin'.padStart(13, '-'); // ---------amin
'amin'.padEnd(13, '-'); // amin---------
// حرف دیفالت این متد اسپیس هست یعنی چیزی بهش ندی خودش اسپیس مبده

// خب حالا تابعی میسازیم که 4 رقم اخر شماره کارتو نشون بده بقیشو حذف کنه
const maskCreditCard = function (number) {
  return number.slice(-4).padStart(number.length, '*');
};
maskCreditCard('6063731179072401');
maskCreditCard('6063731179072434930490010000');

// .repeat( number )_________________________________________

// یه استرینگ میسازه که این 4 بار داخلش تکرار شده
'-amin*'.repeat(4); // -amin*-amin*-amin*-amin*

// ساخت تابعی که به تعداد هواپیما ها ایموجی لاگ بگیره
const planesInLine = function (n) {
  return `There are ${n} planes in line ${'✈️'.repeat(n)}`;
};
planesInLine(5); // There are 5 planes in line ✈️✈️✈️✈️✈️
planesInLine(3); // There are 3 planes in line ✈️✈️✈️

// چک کردن اینکه کاربر اسمشو کامل نوشته یا نه
const fullName = 'amin sheikhy';
fullName.includes(' '); // true

// Coding Challenge_________________________________________

const textarea = ` unDersCore_case  
   First_name
oNe_Some_VarIaBle   
  caLculAtE_mY_AGE
   dElaYed_DePartUre  `;

function pascalCaser(string) {
  if (typeof string !== 'string') return; // برای چک کردن ورودی تابع

  const tempVar = string.split('\n').map((word) => word.trim().toLowerCase());

  const pascalCased = tempVar.map((word) =>
    word
      .split('_')
      .map((letter) => letter.replace(letter[0], letter[0].toUpperCase()))
      .join(' ')
  );

  pascalCased.forEach((word, index) => `${word.padEnd(18)} ${'✅'.repeat(index + 1)}`);
}

pascalCaser(32); // عملی انجام نمیده چون ورودی استرینگ نیس

pascalCaser(textarea);
// Underscore Case    ✅
// First Name         ✅✅
// One Some Variable  ✅✅✅
// Calculate My Age   ✅✅✅✅
// Delayed Departure  ✅✅✅✅✅

// Coding Challenge_________________________________________

const flights =
  '_Delayed_Departure;fao93766109;txl2133758440;11:25+_Arrival;bru0943384722;fao93766109;11:45+_Delayed_Arrival;hel7439299980;fao93766109;12:05+_Departure;fao93766109;lis2323639855;12:30';

// 🔴 Delayed Departure from FAO to TXL (11h25)
//              Arrival from BRU to FAO (11h45)
//   🔴 Delayed Arrival from HEL to FAO (12h05)
//            Departure from FAO to LIS (12h30)

for (const i of flights.split('+')) {
  const [type, from, to, time] = i.split(';');

  `${type.startsWith('_Delayed') ? '🔴' : ''}${type.replaceAll('_', ' ').trim()} from ${from.slice(0, 3).toUpperCase()} To ${to
    .slice(0, 3)
    .toUpperCase()} at time ${time.replace(':', 'h')}`.padStart(50);
}
```
