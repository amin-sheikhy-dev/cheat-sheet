```js
const accounts = [
  { owner: 'amin mohammadi', movements: [200, 450, -400, 3000, -650, -130, 70, 1300, 200, 10], pin: 1111 },

  { owner: 'zahra yousefi', movements: [200, -200, 340], pin: 3333 },

  { owner: 'ali hosseini', movements: [5000, 3400, -150, -790], pin: 2222 },

  { owner: 'Sara ahmadi', movements: [30, -12], pin: 4444 },
];
const [account1, account2, account3, account4] = accounts;

// push , pop , shift , unshift , indexOf , includes_________________________________________

const numbers = [1, 2, 3, 4];

// یک مقدار به اخر ارایه اضافه میکنه
numbers.push('@'); // [1, 2, 3, 4] ==> [1, 2, 3, 4, '@']

// یک مقدار از اخر ارایه پاک میکنه
numbers.pop(); // [1, 2, 3, 4] ==> [1, 2, 3]

// یک مقدار رو از اول ارایه پاک میکنه
numbers.shift(); // [1, 2, 3, 4] ==> [2, 3, 4]

// یک مقدار رو به اول ارایه اضافه میکنه
numbers.unshift('%'); // [1, 2, 3, 4] ==> ['%', 1, 2, 3, 4]

// متد بسیار کارامد ک میگه این مقدار در کدوم جایگاه تابع هست
numbers.indexOf('3'); // 2

// به ما میگه که یک مقدار در ارایه وجود دارد یا نه - فقط بولین ریترن میکنه
numbers.includes(3); // true
numbers.includes(8); // false

let myArray = ['a', 'b', 'c', 'd', 'e'];
// .slice()_________________________________________

// متد اسلایس دقیقا مثل همونی که توی استرینگ ها بود یه قسمت از ارایه رو تیکه میکنه - ارایه اصلی دستکاری نمیشه
myArray.slice(1, 3); // ['b', 'c']
// اگه ورودی اخر رو نزاری تا اخرش اسلایس میکنه - ورودی اخر رو نمیده یعنی  1 و 2 رو میده

// منفی هم میشه داد که از اخر شروع میشه
myArray.slice(-2); // ['d', 'e']
// دوتا مورد اخریو میاره الان

// از اول شروع میکنه اسلایس کردن و تا اخر میره که یعنی یه کپی از کل ارایه بهمون میده
myArray.slice();

// .splice(index, deleteCount, item1, item2, …… )_________________________________________

// این متد تقریبا شبیه متد اسلایس هست - ارایه اصلی رو دست کاری میکنه

// از ایندکس 3 شروع میکنه برداشتن و تا اخر میره
myArray.splice(3); // ['d', 'e']

myArray = ['a', 'b', 'c', 'd', 'e'];

// از ایندکس 1 تا 3 بردار - عدد اخر یعنی خود 3 هم شامل میشه
myArray.splice(1, 3);
myArray = ['a', 'b', 'c', 'd', 'e'];

// اضافه کردن مقدار
// صفر باشد هیچ مقدار حذف نمیشود deleteCount اگر
myArray.splice(1, 0, '#');
// myArray = ['a', '#', 'b', 'c', 'd', 'e']

myArray = ['a', 'b', 'c', 'd', 'e'];

// جایگزین کردن یه عنصر - از ایندکس 3 یک مقدار رو حذف کن
myArray.splice(3, 1, '*');
// myArray = ['a', 'b', 'c', '*', 'e']

myArray = ['a', 'b', 'c', 'd', 'e'];

myArray.splice(1, 3, '$', '@', '%');
// myArray = ['a', '$', '@', '%', 'e']

myArray = ['a', 'b', 'c', 'd', 'e'];

// .toSpliced()
// عمل میکنه فقط با این تفاوت که ارایه اصلی رو تغییر نمیده و ارایه جدید ریترن میکنه splice دقیقا شبیه متد

// .reverse()_________________________________________

// متدی که ارایه رو برعکس میکنه - ارایه اصلی دستکاری میشه
myArray.reverse();
// myArray = ['e', 'd', 'c', 'b', 'a']

myArray = ['a', 'b', 'c', 'd', 'e'];

// .toReversed()
// فقط با این تفاوت که ارایه اصلی رو تغییر نمیده و ارایه جدید ریترن میکنه reverse دقیقا مثل متد

// Array.concat(Array2)_________________________________________

// متدی که دو ارایه رو ترکیب میکنه - ارایه اصلی دستکاری نمیشه
let myArray2 = ['f', 'g', 'h', 'i', 'j'];

const letter = myArray.concat(myArray2);
// letter = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']

// دقیقا مثل اسپرید اپریدور
const letter2 = [...myArray, ...myArray2];
// letter2 = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']

// .join()_________________________________________

// اعضای یک ارایه رو به استرینگ تبدیل میکنه
letter.join('*');
// 'a*b*c*d*e*f*g*h*i*j'

// .at()_________________________________________

// متودی که بخای یه مقدار رو از ارایه بگیری - تقریبا مثل همون براکته فقط تفاوتش اینه منفی هم میشه داد بهش
myArray = ['a', 'b', 'c', 'd', 'e'];
// این دوتا یه نتیجه میدن
myArray[2]; // 'c'
myArray.at(2); // 'c'

// عضو اخر ارایه رو بگیری راحت تری at بخای با متد
myArray[myArray.length - 1]; // 'e'
myArray.at(-1); // 'e'

// .forEach( (value, index, array) => {} )_________________________________________

// ورودی سوم خود ارایه هست - ورودی دوم ایندکس مقادیر - میگیره که ورودی اولش مقادیر ارایه CallBack Fn این متد یه

const arr6 = [1, 9, -3, -2, 5, 4, 0];

arr6.forEach((value) => {
  value >= 0 ? `${value} is Positive` : `${value} is Negative`;
});
// هست FirstClass function هست و فانکشن داخلش یه HighOrder function یه forEach خود
// استفاده کرد یعنی تا اخر ارایه رو لوپ میزنه continue و break های keyWord نمیشه از forEach توی حلقه

// ForEach with Maps & Sets
// هم کلید ها هم مقادیر رو میده
const currencies = new Map([
  ['<USD>', '-United States dollar-'],
  ['<EUR>', '-Euro-'],
  ['<GBP>', '-Pound sterling-'],
]);

currencies.forEach((value, key, map) => {
  (value,
    // -United States dollar-
    // -Euro-
    // -Pound sterling-

    key);
  // <USD>
  // <EUR>
  // <GBP></GBP>
});

const currenciesUnique = new Set(['amin', 'dorsa', 'amin', 'mamad', 'zahra', 'zahra']);

// ست ها ایندکس یا کلید مشخصی ندارن و با همون مقدارشون کپی شده
currenciesUnique.forEach((value, _, set) => {
  // و همچنان مقادیر تکراری رو نمیده
  value;
  // 'amin'
  // 'dorsa'
  // 'mamad'
  // 'zahra'
});

// .map( (value, index, array) => {} )_________________________________________

// اینه که ارایه جدید ریترن میکنه forEach تنها فرقش با

let movements = [200, 3000, -650, -130, 70];

// اگه میخاستیم همین کارو با حلقه معمولی انجام بدیم باید اول یه ارایه خالی میساختیم بعد مقدارشو پر میکردیم ولی این متد خودش ارایه جدیدو میسازه
const movementUSD = movements.map((mov) => {
  return Math.round(mov * 1.1);
});
// [220, 495, -440, 3300, -715, -143, 77, 1430]

const movementsDescriptions = movements.map((mov, i) => `Movement ${i + 1}: ${mov > 0 ? 'deposits' : 'withdrew'} ${Math.abs(mov)}`);
// ['Movement 1: deposits 200', 'Movement 2: deposits 3000', 'Movement 3: withdrew 650', 'Movement 4: withdrew 130', 'Movement 5: deposits 70']

// .filter( (value, index, array) => {} )_________________________________________

// اعضای یک ارایه رو بر اساس یه شرط فیلتر میکنه

// movements = [200, 3000, -650, -130, 70]

// میخام تمام مقادیر مثبت رو فیلتر کنه
const deposits = movements.filter((mov) => {
  return mov > 0;
});
// [200, 3000, 70]

// میخام تمام مقادیر منفی رو فیلتر کنه
const withdrawal = movements.filter((mov) => mov < 0);
// [-650, -130]

// .reduce( (acc, value, index, array) => {} ,acc )_________________________________________

// می‌تونه جمع ضرب میانگین یا حتی ساختن یک آبجکت جدید باشه
// ورودی دوم که دریافت میکنه مقدار اولیه اکیومیلتور هست

// movements = [200, 3000, -650, -130, 70]

const balance = movements.reduce((acc, val, i, arr) => {
  `${i}: | acc = ${acc} | val = ${val}`;

  //  0: | acc = 0 | val = 200
  //  1: | acc = 200 | val = 3000
  //  2: | acc = 3200 | val = -650
  //  3: | acc = 2550 | val = -130
  //  4: | acc = 2420 | val = 70

  return acc + val;
}, 0);

// میبینیم که همه مقادیر جمع شدن
balance; // 2490

// پیدا کردن مقدار ماکسیمم یه ارایه با ردیوس
const max = movements.reduce((acc, mov) => {
  if (acc > mov) {
    return acc;
  } else {
    return mov;
  }
}, movements[0]);

max; // 3000

// reduce چند تا مثال پیشرفته از متد

const nums = [1, 4, 6, 7, 2, 2, 1, 4, 7, 1]; // ساخت ارایه واحد و یونیک

const unique = nums.reduce((acc, val) => {
  if (!acc.includes(val)) acc.push(val);

  return acc;
}, []); // [1, 4, 6, 7, 2]

const strings = ['a', 'b', 'b', 'a', 'a', 'b', 'c', 'a', 'a', 'c', 'd', 'd', 'e']; // شمارش تعداد تکرار هر حرف
const $count = strings.reduce((acc, str) => {
  acc[str] = (acc[str] || 0) + 1;

  return acc;
}, {}); // {a: 5, b: 3, c: 2, d: 2, e: 1}

// .find( (value, index, array) => {} )_________________________________________

// عمل میکنه ولی با این تفاوت که وقتی اولین شرط برقرار شد اون عضو ارایه رو ریترن میکنه و فقط یک مقدار ریترن میکنه filter تقریبا مثل متود

const findMethod = [23, 1, 3, 45, 6, 3, 3, 3, 3, 3, 1].find((mov) => mov === 3);
findMethod; // 3

// مثلا میخاییم اکانت جسیکا دیویس رو پیدا کنیم
const jessicaAcc = accounts.find((account) => account.owner === 'Jessica Davis');

jessicaAcc; // {owner: 'Jessica Davis', movements: Array(4), interestRate: 1.5, pin: 2222, currency: 'EUR', …}

// .findIndex( (value, index, array) => {} )_________________________________________

// عمل میکنه با این تفاوت که ایندکس اون عضو ارایه رو ریترن میکنه find دقیقا مثل متد

const findIndex = ['amin', 'dorsa', 'matin', 'zahra', 'mamad'].findIndex((numIndex) => numIndex === 'zahra');
findIndex; // 3

// movements = [200, 3000, -650, -130, 70]

// .findLast( (value, index, array) => {} )_________________________________________

// هست فقط از اخر شروع میکنه شمردن find مثل همون متد
const lastWithdrawal = movements.findLast((mov) => mov < 0);
lastWithdrawal; // -130

// .findLastIndex( (value, index, array) => {} )_________________________________________

// فقط با این تفاوت که از اخر شروع میکنه findIndex مثل همون متد
const lastWithdrawalIndex = movements.findLastIndex((mov) => mov < 0);
lastWithdrawalIndex; // 3

// .some( (value, index, array) => {} )_________________________________________

// مقدار های ترو یا فالس ریترن میکنه و با این تفاوت ک میشه براش شرط نوشت includes مثل متود
// ریترن میکنه True یکی از المنت های ارایه شرط رو برقرار کنه

// movements = [200, 3000, -650, -130, 70]

const deposist2000 = movements.some((mov) => mov > 2000); // ایا واریزی بیش از 2000 هزار دلار هست؟

const withdraw130 = movements.some((mov) => mov === -130); // ایا 130 دلار از حساب کم شده؟

// .every( (value, index, array) => {} )_________________________________________

// ریترن کنه True اینه که باید همه اعضای ارایه اون شرط رو برقرار کنن تا some فرقش با متد

const everyMtd = movements.every((mov) => mov > 0); // ایا همه تراکنش ها واریز هستن؟

const everyMtd2 = account4.movements.every((mov) => mov > 0); // همه تراکنش های این حساب واریز هستن؟

// Coding Challenge_________________________________________

const breeds = [
  { breed: 'German Shepherd', averageWeight: 32, activities: ['fetch', 'swimming'] },

  { breed: 'Dalmatian', averageWeight: 24, activities: ['running', 'fetch', 'agility'] },

  { breed: 'Labrador', averageWeight: 28, activities: ['swimming', 'fetch'] },

  { breed: 'Beagle', averageWeight: 12, activities: ['digging', 'fetch'] },

  { breed: 'Husky', averageWeight: 26, activities: ['running', 'agility', 'swimming'] },

  { breed: 'Bulldog', averageWeight: 36, activities: ['sleeping'] },

  { breed: 'Poodle', averageWeight: 18, activities: ['agility', 'fetch'] },
];

// و گرفتن مقدار وزن میانگین آن Husky پیدا کردن نژاد
const huskyWeight = breeds.find((dogBreed) => dogBreed.breed === 'Husky').averageWeight;
// 26

// را دارد running و fetch پیدا کردن اولین نژادی که همزمان فعالیت‌های
const dogBothActivities = breeds.find((dog) => dog.activities.includes('fetch') && dog.activities.includes('running')).breed;
// Dalmatian

// استخراج تمام فعالیت‌های همه نژادها در یک آرایه واحد
const allActivities = breeds.flatMap((dog) => dog.activities);
// ['fetch', 'swimming', 'running', 'fetch', 'agility', 'swimming', 'fetch', 'digging', 'fetch', 'running', 'agility', 'swimming', 'sleeping', 'agility', 'fetch']

// و تبدیل دوباره به آرایه Set حذف فعالیت‌های تکراری با استفاده از
const allActivitiesSet = [...new Set(allActivities)];
// ['fetch', 'swimming', 'running', 'agility', 'digging', 'sleeping']

// دارند swimming فیلتر کردن نژادهایی که فعالیت
const swimmingAdjacent = breeds.filter((dog) => dog.activities.includes('swimming'));
// [{…}, {…}, {…}]

// بررسی اینکه آیا وزن میانگین همه نژادها بیشتر از 10 است یا نه
breeds.every((dog) => dog.averageWeight > 10);
// true

// بررسی اینکه آیا حداقل یک نژاد وجود دارد که 3 فعالیت یا بیشتر داشته باشد
breeds.some((dog) => dog.activities.length >= 3);
// true

// انجام می‌دهند و پیدا کردن بیشترین وزن بین آن‌ها fetch گرفتن وزن نژادهایی که
const fetchWeights = breeds.filter((dog) => dog.activities.includes('fetch')).map((dog) => dog.averageWeight);
Math.max(...fetchWeights);
// 32

// .sort()_________________________________________

// این متد ارایه اصلیو دستکاری میکنه
const owners = ['Jonas', 'Zach', 'Adam', 'Martha'];

// شده sort الان ارایه بر اساس حروف
owners.sort();
// ['Adam', 'Jonas', 'Martha', 'Zach']

// این متد اول همه ورودی هارو به استریگ تبدیل میکنه و بعد مرتب میکنه
// ترتیب مرتب کردنشم اینجوره ک اول منفی ها و بعد حرف اول عددارو نگا میکنه
// movements = [200, 3000, -650, -130, 70]

movements.sort(); // [-130, -650, 200, 3000, 70]

movements = [200, 3000, -650, -130, 70];

movements.sort((a, b) => a - b); // -650, -130, 70, 200, 3000]

movements = [200, 3000, -650, -130, 70];

movements.sort((a, b) => b - a); // [3000, 200, 70, -130, -650]

// .toSorted()
// عمل میکنه و تنها فرقش اینه ارایه اصلی رو تغییر نمیده و ارایه جدید میشه باهاش ریترن کرد sort دقیقا مثل متد

// Object.groupBy( array, (value, index, array) => {} )_________________________________________

// ورودی اول یک ارایه میگیره و ورودی دوم یک فانکشن - طبق شرط هایی که در اون فانکشن نوشتیم یه ابجکت برای ما تولید میکنه
// می‌شود undefined نداشته باشه نتیجه return میدی باید حتما یک مقدار ریترن کنه و اگر Object.groupBy تابعی که به

// ورودی های فانکشن عضو های ارایه هست
const groupedMovements = Object.groupBy(movements, (mov) => (mov > 0 ? 'deposit' : 'withdrawal'));

const items = ['@', -1, 4, 9, '%', -21, 34, '#', -3, 0];

const groupedArr = Object.groupBy(items, (item) => {
  if (typeof item !== 'number') return 'non-numeric';

  if (item > 0) return 'positive';

  if (item < 0) return 'negative';

  if (item === 0) return 'zero';

  return 'unknown❗';
});

// .fill( value, start, end )_________________________________________

// ساخت ارایه
new Array(1, 2, 3, 4, 5, 6, 7); // [1, 2, 3, 4, 5, 6, 7]

// یه ارایه درست میکنه از 7 مقدار خالی
let x2 = new Array(7);
// x2 = [empty × 7]

x2.fill(1); // اگه ایندکس شروع و پایان ندیم از اول تا اخر حساب میکنه
// x2 = [1, 1, 1, 1, 1, 1, 1]
// کل مقادیر با 1 پر شدن

x2 = new Array(7);

// از ایندکس 3 شروع کن تا اخر همرو 1 بزار
x2.fill(1, 3);
// x2 = [empty × 3, 1, 1, 1, 1]

x2 = new Array(7);

// از ایندکس 3 تا 5 همرو 1 بزار
// اخری رو شامل نمیشه یعنی 3 و 4 پر میشه ولی 5 نه
x2.fill(1, 3, 5);
// x2 = [empty × 3, 1, 1, empty × 2]

let arr7 = [1, 2, 3, 4, 5, 6, 7];
// اخری رو شامل نمیشه یعنی 2 و 3 پر میشه ولی 4 نه
arr7.fill('x', 2, 4);
// arr7 = [1, 2, 'x', 'x', 5, 6, 7]

// .with( index, value )_________________________________________

// این متد ارایه جدید ریترن میکنه با عضو جدیدی که ما جایگزین عضوای قبلی کردیم
const newArr = ['a', 'b', 'c', 'd', 'e'].with(1, '#');
// newArr = ['a', '#', 'c', 'd', 'e']

// Array.from( { length:  }, ( value, index, array ) => {} )_________________________________________

// توی هر تکرار به هر عضو ارایه و ایندکس اون عضو دسترسی داره filter و map میگیره که مثل متد های CallBack Fn ای که بهش دادیم میسازه و ورودی دوم length یک ارایه جدید با

// بهش دادیم CallBack function و توی هر عضو عدد 2 رو ریترن کنه که توی length یک ارایه به 6
const arr8 = Array.from({ length: 6 }, () => 2);
arr8; // [2, 2, 2, 2, 2, 2]

// ریترن میکند میشه مقدار اون عضو ارایه CallBack Fn دقت شود چیزی که
const arr9 = Array.from({ length: 5 }, (val, i) => i + 1);
arr9; // [1, 2, 3, 4, 5]

// .flat()_________________________________________

// برای تخت‌ کردن آرایه‌های تو‌در‌تو استفاده می‌شود
[1, [2, [3, [4]]]].flat();
// [1, 2, 3, 4]

// .flatMap( (value, index, array) => {} )_________________________________________

// هست و این متد اول مپ میکنه و بعد فلت flat و map دقیقا ترکیب دوتا متد flatMap متد
const bankDepositSum = accounts.flatMap((acc) => acc.movements);
// [200, 450, -400, 3000, -650, -130, 70, 1300, 200, 10, 5000, 3400, -150, -790, 200, -200, 340]

// prefix ++_________________________________________

let X = 10;
// console.log(X++) 10

// این خط بالا یکی بهش اضافه کرده ولی همونجا ریترن نمیکنه و توی خط پایینی میبینیم ک ریترن شده
// console.log(X) 11

// ولی بجاش از این استفاده کنیم همونجا استفاده میکنه و همونجا هم ریترن میکنه
// اون + ها باید سمت چپ گزاشته بشن نه راست
// console.log(++X) 12

// Coding Challenge_________________________________________

const dogs = [
  { weight: 22, curFood: 250, owners: ['Alice', 'Bob'] },

  { weight: 8, curFood: 200, owners: ['Matilda'] },

  { weight: 13, curFood: 275, owners: ['Sarah', 'John', 'Leo'] },

  { weight: 18, curFood: 244, owners: ['Joe'] },

  { weight: 32, curFood: 340, owners: ['Michael'] },
];

// ساخت مقدار غذای توصیه‌شده برای هر سگ
dogs.forEach((dog) => (dog.recFood = Math.round(dog.weight ** 0.75 * 28)));

// و بررسی کم یا زیاد بودن غذای آن Sarah پیدا کردن سگ
const sarahDog = dogs.find((dog) => dog.owners.includes('Sarah'));

`Sarah dog eats too ${sarahDog.recFood > sarahDog.curFood ? 'much' : 'little'}`;

// استخراج صاحبان سگ‌هایی که زیاد یا کم غذا می‌خورند
const ownersTooLittle = dogs.filter((dog) => dog.recFood > dog.curFood).flatMap((dog) => dog.owners);
// ['Matilda', 'Sarah', 'John', 'Leo']
const ownersTooMuch = dogs.filter((dog) => dog.recFood < dog.curFood).flatMap((dog) => dog.owners);
// ['Alice', 'Bob', 'Joe', 'Michael']

// بررسی وجود حداقل یک سگ با غذای کاملاً برابر مقدار توصیه‌ شده
const dogFood = dogs.some((dog) => dog.curFood === dog.recFood);
// false

// بررسی اینکه آیا همه سگ‌ها غذای مناسب می‌خورند
const curDogFood = dogs.every((dog) => dog.curFood < dog.recFood * 1.1 && dog.curFood > dog.recFood * 0.9);
// false

// فیلتر کردن سگ‌ هایی که غذای مناسبی دارند
const dogsEatingOkay = dogs.filter((dog) => dog.curFood < dog.recFood * 1.1 && dog.curFood > dog.recFood * 0.9);
// [{…}, {…}]

// گروه‌ بندی سگ‌ ها بر اساس مقدار غذای مصرفی
const dogsGroupedByPortion = Object.groupBy(dogs, (dog) => {
  if (dog.recFood > dog.curFood) return 'too-much';

  if (dog.recFood < dog.curFood) return 'too-little';

  if (dog.recFood === dog.curFood) return 'exact';
});
// { too-little: [{…}, {…}], too-much: [{…}, {…}, {…}] }

// گروه‌ بندی سگ‌ ها بر اساس تعداد صاحبان
const dogsGroupedByOwners = Object.groupBy(dogs, (dog) => {
  if (dog.owners.length >= 3) return '3-owners';

  if ((dog.owners.length = 2)) return '2-owners';

  if ((dog.owners.length = 1)) return '1-owners';
});
// { 1-owners: [{…}, {…}, {…}], 2-owners: [{…}], 3-owners: [{…}] }

// Coding Challenge_________________________________________

// n! / (r! * (n-r)!) چالش محاسبه فرمول
function combination(n, r) {
  // تابع محاسبه فاکتوریل
  function factorial(num) {
    let result = 1;
    for (let i = 2; i <= num; i++) result *= i;
    return result;
  }

  let numerator = factorial(n);
  let denominator = factorial(r) * factorial(n - r);

  [2, 3, 5, 7].forEach((num) => {
    while (numerator % num === 0 && denominator % num === 0) {
      numerator = numerator / num;
      denominator = denominator / num;
    }
  });

  return numerator / denominator;
}

combination(4, 0);
combination(6, 1);
combination(8, 2);
```
