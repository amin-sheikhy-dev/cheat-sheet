```ts
// Variable type ____________________________________________________________________________________
let firstName: string;
firstName = 'amin';

const age: number = 23;

const male: boolean = true;
const female: boolean = false;

let anyVals: any = 1384;
anyVals = '21';
anyVals = false;

// Union types = چند مقدار گزاشتن برای یه متغیر
let threeVals: string | number | boolean = 'ali';
threeVals = 23;
threeVals = true;

// Array ____________________________________________________________________________________
const names: string[] = ['amin', 'ali', 'mamad', 'reza'];
const names2: Array<string> = ['amin', 'ali', 'mamad', 'reza']; // Generic types

const anyArrays: (string | number | boolean)[] = [-3, 'amin', true, 88688, false, '0'];
const anyArrays2: Array<string | number | boolean> = [-3, 'amin', true, 88688, false, '0']; // Generic types

// آرایه ای از ابجکت
type Person = {
  name: string;
  age: number;
  isActive: boolean;
};

const personArray: Person[] = [
  { name: 'sara', age: 23, isActive: false },
  { name: 'amin', age: 22, isActive: true },
  { name: 'reza', age: 29, isActive: false },
  { name: 'fateme', age: 42, isActive: true },
];

// Tuples = ساخت ارایه با مقدار و تایپ مشخص
const arr: [string, number] = ['amin', 12];

const arr2: [string, number, boolean] = ['ali', 23, true];
// arr2 = ['ali', 23] اینو قبول نمیکنه

// Object ____________________________________________________________________________________
const information: {
  name: string;
  age: string | number;
  friends: string[];
} = {
  name: 'amin',
  age: 21,
  friends: ['ali', 'mamad', 'reza'],
};

// undefined یا null این تایپ به معنای هرچی جز
let varable: {} = 'hassan';
// الان پایینی ها ارور میده
// let varable: {} = null
// let varable: {} = undefined

// حتما باید یک ابجکت باشه - کلید آن استرینگ است - ولیو های آن یا استرینگ یا نامبر
type DataTypes = Record<string, number | string>;

const data: DataTypes = {
  name: 'amin',
  age: 21,
  // male: true --> error
};

type Num = Record<number, boolean | string>;

const nums: Num = {
  10: 'ten',
  4: true,
  9: 'nine',
  0: false,
  // name: 'amin' --> error
  // 6: 2 --> error
};

const numbers = {
  num1: 1.43,
  num2: -3.23,
} satisfies Record<string, number>; // بررسی کن که این ابجکت با تایپ ها سازگار باشد

// enum = یجور تایپ های سفارشی هستن چون میتونیم ازشون به عنوان تایپ استفاده کنیم
// اگر مقداری داده نشه مقادیر به صورت پیشفرض از 0 شروع و به صورت خودکار افزایش پیدا میکنن
enum Role {
  admin, // 0
  editor, // 1
  guest, // 2
}

const aminRole: Role = Role.admin; // aminRole = 0

const ahmadRole: Role = Role.guest; // ahmadRole = 2

// میتونی خودتم براشون مقدار تایین کنی
enum Direction {
  up = 'w',
  down = 's',
  left = 'a',
  right = 'd',
}

// custom type
type EvenNums = 2 | 4 | 6 | 8;
const num: EvenNums = 4;
const num: EvenNums = 3; // ❌

type Roles = 'admin' | 'editor' | 'guest';
const rezaRole: Roles = 'editor';

function access(user: Roles) {}

type ArrayType = [1 | -1, boolean, false | true, 'amin' | 'ali'];
const arr: ArrayType = [1, true, false, 'amin'];

// Function ____________________________________________________________________________________
// نوع ورودی ها باید مشخص شه ولی نوع خروجی های فانکشن اختیاریه
function boolean(a: number, b: number): boolean {
  return a === b;
}

const add = (a: number, b: number): number => a + b;

// داد بهش void تابعی که چیزی رو ریترن نمیکنه میشه مقدار
function Something(message: string): void {
  console.log(message);
}
// تابع اجرا می‌شود و تمام می‌شود ولی چیزی برنمی‌گرداند

// را میتوان گفت هرگز پایان نمیابند یا تابع به صورت عادی پایان نمیابد never توابع با تایپ
function throwingError(errorMessage: string): never {
  throw new Error(errorMessage);
}

function infiniteLoop(): never {
  while (true) {}
}

// function type = برای متغیر ها تایپ فانکشن مشخص میکنیم

// تابعی که خروجی خاصی نمیدهد
const log: () => void = () => console.log('hi');

// تابعی با تایپی که دوتا عدد ورودی بگیره و خروجی هم عدد باشه
const multiply: (a: number, b: number) => number = (x, y) => x * y;

// تایپ تابعی که خروجیش عدده
type NumberFunc = () => number;
const getRandom: NumberFunc = () => Math.random();

type GetString = (message: string) => string;

const sayHello: GetString = function () {
  return 'Hello';
};

// تابعی که ورودیش یک فانکشنه
function adder(addFn: (num1: number, num2: number) => number) {
  addFn(2, 4);
}

type UserType = {
  name: string;
  age: number;
  greet: () => string; // تایپش تابعی هست که باید استرینگ ریترن کنه
};

const amin: UserType = {
  name: 'amin sheikhy',

  age: 21,

  greet: function () {
    return `Hi ${this.name}`;
  },

  // یا میشه از ارو فانکشن استفاده کرد
  greet: () => 'Hi amin',
};

// Type casting
// الان ما به تایپ اسکریپت میگیم که یه المنت اینپوت رو ریترن و ذخیره میکنیم
const input = document.getElementById('user-name') as HTMLInputElement | null;
// می‌گوید TypeScript نوع واقعی عنصر را تغییر نمی‌دهد فقط به as اینجا
// من بهتر از تو می‌دانم این مقدار چه تایپی دارد
console.log(input?.value);

// unknown type
// بعضی وقتا ما مواردی پیش میاد که ما نمیدونیم تایپمون چیه مثلا دیتایی که از سرور میگیریم
// شرط بزاریم براش if این هست که باید قبل استفاده ازش با any فرقش با تایپ
function process(val: unknown) {
  if (typeof val === 'object' && !!val && 'log' in val && typeof val.log === 'function') {
    val.log();
  }
}

// Optional Value
// این ویژگی بیشتر در تابع ها استفاده میشه - با گزاشتن علامت سوال جلوی ورودی تابع
function throwError(message?: string) {
  throw new Error(message || 'error');
}

type person = {
  name: string;
  age: number;
  // الان ویژگی رول اختیاریه
  role?: 'admin' | 'user' | 'owner';
};

//

type FileData = {
  path: string;
  content: string;
};

type Status = {
  isOpen: boolean;
  errorMessage?: string;
};

type AccessedFileData = FileData & Status;
```
