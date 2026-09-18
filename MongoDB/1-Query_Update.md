```js
db.users.insertOne({ name: 'Amin', age: 27, city: 'Qom' });
```

نتیجه

```json
{
  "_id": ObjectId("..."),
  "name": "Amin",
  "age": 27,
  "city": "Qom"
}
```

```js
db.collection.method(filter, update); // تقریباً تمام عملیات به این شکل هستند

db.users.insertOne({ name: 'Amin', age: 27, city: 'Qom' });

db.users.insertMany([
  { name: 'Ali', age: 22 },
  { name: 'Sara', age: 30 },
  { name: 'Reza', age: 19 },
]);

db.users.find(); // همه کاربران

// فقط کاربری که اسمش علی باشد
db.users.find({ name: 'Ali' });

// افراد بالای 20 سال
db.users.find({ age: { $gt: 20 } });

// فقط اولین سند را برمی‌گرداند
db.users.findOne({ name: 'Sara' });

// سن علی رو ۲۵ کن
db.users.updateOne({ name: 'Ali' }, { $set: { age: 25 } });

// سن همه افراد زیر 18 سال را 18 کن
db.users.updateMany({ age: { $lt: 18 } }, { $set: { age: 18 } });

// کاربر با اسم علی حذف شه
db.users.deleteOne({ name: 'Ali' });

// حذف همه افراد 18 ساله
db.users.deleteMany({ age: 18 });
```

```json
[
  {
    "name": "Ali",
    "age": 22,
    "city": "Tehran"
  },
  {
    "name": "Sara",
    "age": 30,
    "city": "Shiraz"
  },
  {
    "name": "Amin",
    "age": 27,
    "city": "Qom"
  },
  {
    "name": "Reza",
    "age": 18,
    "city": "Tehran"
  }
]
```

### Query operators

```js
// Comparison _________________________________________________________________________

// Greater Than افرادی که سنشان بیشتر از 25 است
db.users.find({
  age: { $gt: 25 },
});

// Greater Than or Equal سن بیشتر یا مساوی 27
db.users.find({
  age: { $gte: 27 },
});

// Less Than سن کمتر از 25
db.users.find({
  age: { $lt: 25 },
});

// Less Than or Equal
db.users.find({
  age: { $lte: 22 },
});

// برابر با
db.users.find({
  age: { $eq: 30 },
});
// ساده ترش
db.users.find({
  age: 30,
});

// Not Equal مساوی نباشه
db.users.find({
  age: { $ne: 18 },
});

// افرادی که شهرشان تهران یا قم باشد
db.users.find({
  city: {
    $in: ['Tehran', 'Qom'],
  },
});

// برعکس بالایی
db.users.find({
  city: {
    $nin: ['Tehran', 'Qom'],
  },
});

// Logical _________________________________________________________________________

// سن بالای ۲۰ و شهر تهران
db.users.find({
  $and: [{ age: { $gt: 20 } }, { city: 'Tehran' }],
});
// ساده ترش
db.users.find({
  age: { $gt: 20 },
  city: 'Tehran',
});

// افرادی که یا در تهران هستند یا سنشان 30 است
db.users.find({
  $or: [{ city: 'Tehran' }, { age: 30 }],
});

// $not — نقیض شرط
db.collection('users').find({
  age: { $not: { $gt: 18 } },
});

// $nor — هیچکدوم درست نباشه
db.collection('users').find({
  $nor: [{ role: 'banned' }, { role: 'deleted' }],
});

// Array _________________________________________________________________________

// $all — آرایه باید همه این مقادیر رو داشته باشه
db.collection('users').find({ hobbies: { $all: ['reading', 'coding'] } });

// $size — طول دقیق آرایه
db.collection('users').find({ hobbies: { $size: 3 } });

// $elemMatch — حداقل یه عنصر آرایه همه این شرط‌ها رو داشته باشه
db.collection('orders').find({
  items: { $elemMatch: { productId: '123', quantity: { $gte: 2 } } },
});

// Element _________________________________________________________________________

// $exists — فیلد وجود داشته باشه یا نه
db.collection('users').find({ phone: { $exists: true } });

// $type — نوع فیلد
db.collection('users').find({ age: { $type: 'number' } });

// Regex _________________________________________________________________________

// $regex — جستجوی الگو (شبیه LIKE در SQL)
db.collection('users').find({ email: { $regex: '@gmail.com$' } });

// case-insensitive
db.collection('users').find({ name: { $regex: 'amin', $options: 'i' } });

// sort _________________________________________________________________________

// سن از کم به زیاد
db.users.find().sort({
  age: 1,
});

// سن از زیاد به کم
db.users.find().sort({
  age: -1,
});

// pagination _________________________________________________________________________

db.users.find().limit(2); // اولین دو نفر

db.users.find().skip(2); // دو نفر اول را رد کن

db.users.find().skip(10).limit(10);

// مثال ترکیبی
db.users
  .find({ age: { $gt: 20 } })
  .sort({ age: -1 })
  .limit(5);
```

### Update operators

فرض کن این سند رو داریم:

```json
{
  "_id": "1",
  "name": "Amin",
  "age": 25,
  "loginCount": 3,
  "hobbies": ["reading", "coding"],
  "cart": { "items": [] }
}
```

هم برای تغییر، هم برای اضافه کردن فیلد جدید استفاده می‌شه.

```js
await db.collection('users').updateOne({ _id: '1' }, { $set: { name: 'Amin Karimi' } });

await db.collection('users').updateOne(
  { _id: '1' },
  { $unset: { age: '' } } // مقدار مهم نیست، معمولاً '' می‌نویسن - ایج رو کلا پاک میکنه
);

await db.collection('users').updateOne(
  { _id: '1' }, //
  { $inc: { loginCount: 1 } }
);

await db.collection('users').updateOne(
  { _id: '1' }, //
  { $push: { hobbies: 'gaming' } }
);

// اگه مقدار از قبل تو آرایه باشه، اضافه نمی‌کنه
await db.collection('users').updateOne(
  { _id: '1' },
  { $addToSet: { hobbies: 'coding' } } // از قبل هست
);

await db.collection('users').updateOne(
  { _id: '1' }, // حذف از آرایه بر اساس شرط
  { $pull: { hobbies: 'gaming' } }
);

// آخرین عنصر رو حذف کن
await db.collection('users').updateOne(
  { _id: '1' },
  { $pop: { hobbies: 1 } } // 1 = از آخر، -1 = از اول
);

await db.collection('users').updateOne(
  { _id: '1' },
  {
    $set: { name: 'Amin K.' },
    $inc: { loginCount: 1 },
    $push: { hobbies: 'photography' },
  }
);
```

این ساختار در مانگودیبی بسیار رایج است

```json
{
  "name": "Amin",
  "orders": [
    {
      "product": "Laptop",
      "price": 1200
    },
    {
      "product": "Mouse",
      "price": 40
    }
  ]
}

// هر داکیومنت یک شناسه یکتا دارد
{
  "_id": ObjectId("6880f5f2e2c5...")
}
```

```js
db.users.findOne({
  _id: ObjectId('6880f5f2e2c5...'),
});
```

جستجو داخل آبجکت

```json
{
  "name": "Amin",
  "address": {
    "city": "Qom",
    "street": "Imam"
  }
}
```

همه کاربران شهر قم

```js
db.users.find({
  'address.city': 'Qom',
});
```

جستجو داخل Array

```json
{
  "skills": ["Node.js", "React", "MongoDB"]
}
```

و MongoDB خودش بررسی می‌کند که آیا "React" داخل آرایه وجود دارد یا نه.

```js
db.users.find({
  skills: 'React',
});
```

جستجو داخل Array of Objects

```json
{
  "orders": [
    {
      "product": "Laptop",
      "price": 1200
    },
    {
      "product": "Iphon",
      "price": 1000
    },
    {
      "product": "Keyboard",
      "price": 300
    },
    {
      "product": "Mouse",
      "price": 120
    }
  ]
}
```

```js
db.users.find({
  'orders.product': 'Laptop',
});
```
