```ts
// Generic type ____________________________________________________________________________________
// می‌تونیم یک تابع عمومی بنویسیم که با هر نوع داده‌ای کار کنه Generic با استفاده از
// نماد هایی هستن ک بیشتر استفاده میشن K و T هر متن یا حروفی که خاستیم بزاریم ولی T ما میتونیم جای
// باید چه نوع داده ای باشه و اون رو به تابع اعمال می‌کنه T یک نماد برای نوع داده هست وقتی تابع رو صدا می‌زنیم تایپ اسکریپت متوجه میشه که <T>
function merge<T>(a: T, b: T) {
  return [a, b];
}
const arrNums = merge<number>(1, 2);

//

function getFirstElement<T>(arr: T[]): T | undefined {
  return arr.length > 0 ? arr[0] : undefined;
}

const firstNumber = getFirstElement<number>([1, 2, 3]);
const firstBoolean = getFirstElement<boolean>([true, false]);
const firstString = getFirstElement(['a', 'b', 'c', 'd']); // رو حذف کرد و نزاشت کلا چون خودش مشخص میکنه <string> میشه

type UserData<T> = Record<string, T>;

const userApp: UserData<string | boolean> = {
  name: 'amin',
  isAdmin: true,
};

const userCode: UserData<number> = {
  admin1: 2370,
  admin2: 9834,
  admin3: 7854,
};

// Multiple Generic Parameters
function merge<T, U>(a: T, b: U) {
  return [a, b];
}
const numAndBool = merge<number, boolean>(1, true);
const numAndStr = merge(1, 'a'); // میشه نزاشت کلا

// Generic type classes
class ID<T> {
  constructor(public id: T) {}
}

const id1 = new ID('abcd');
const id2 = new ID(123);
const id3 = new ID(true);

// extends
type Length = { length: number };

// باشند length تابع زیر فقط ورودی‌ هایی را می‌پذیرد که حتما دارای خاصیت
function loggingIdentity<T extends Length>(argument: T) {
  return argument;
}

loggingIdentity({ length: 10, value: 3 });

loggingIdentity({ value: 3 }); // ارور میده
loggingIdentity(3); // ارور میده

// Deriving Types From Types ____________________________________________________________________________________
type UserProfile = {
  name: string;
  age: number;
  isActive: boolean;
};

// تمام کلید های یک ابجکت رو میده
type UserProfileKeys = keyof UserProfile; // name , age , isActive

const UserProfileKey1: UserProfileKeys = 'name';
const UserProfileKey2: UserProfileKeys = 'age';

// یک مثال کاربردی
function getProp<T extends object, U extends keyof T>(obj: T, key: U) {
  const val = obj[key];

  if (val === undefined || val === null) {
    throw new Error('value is undefined or null');
  }

  return val;
}

// Mapped Types
// یک مثال کاربردی
type Operations = {
  add: (a: number, b: number) => number;
  subtract: (a: number, b: number) => number;
};

// چون علامت سوال نداره پراپرتی ها اختیاری نیس
type Results<T, U> = Record<keyof T, U>;

const mathResults: Results<Operations, number> = {
  add: 5 + 2,
  subtract: 3 - 1,
};

// مثال
type Person = {
  name: string;
  age: number;
  address: string;
};

// چون از علامت سوال استفاده کردم پراپرتی ها اختیاری میشن
type PartialPerson<T> = {
  [key in keyof T]?: T[key];
};

const user2: PartialPerson<Person> = { name: 'amin' };
// توی بالایی ادرس نزاشتم ولی توی پایینی ادرس گزاشتم چون اختیاریه
const user3: PartialPerson<Person> = { age: 30, address: 'Tehran' };

// readonly
type Car = {
  make: string;
  model: string;
  year: number;
};

type ReadonlyCar = {
  readonly [P in keyof Car]: Car[P];
};

const myCar: ReadonlyCar = { make: 'BMW', model: 'X5', year: 2022 };
myCar.make = 'Audi'; // است readonly ارور میده چون پراپرتی

// template literal type
type UserStatus = 'active' | 'inactive' | 'pending';
type UserName = 'ali' | 'reza' | 'sara';

// هاور کنی نشون میده چیا داری UserInfo روی
type UserInfo = `${UserName}_${UserStatus}`;

let user4: UserInfo = 'ali_active'; // درسته
let user5: UserInfo = 'reza_pending'; // درسته
// let user6: UserInfo = "john_active"   نیست UserName جزو 'john' خطا میده چون
// let user7: UserInfo = "ali_online"   نیست UserStatus جزو 'online' خطا میده چون

// ترکیب کردنش با مپ تایپ
type Mapped = {
  [key in UserInfo]: string | number | boolean;
};

// Conditional Types
// تقریبا یچیزی تو مایه های متغیر های شرطی در جاوا اسکریپت
type GetElementType<T> = T extends any[] ? T[number] : T;
//  T === any[] در واقع اینجوری خوانده میشه  T extends any[]
```
