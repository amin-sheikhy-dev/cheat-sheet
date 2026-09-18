### Index

فرض کن Collection users شامل ۱۰ میلیون کاربر است.

اگر Index نداشته باشی، مجبور است تمام ۱۰ میلیون Document را یکی‌یکی بررسی کند تا به ایمیل موردنظر برسد.

اگر روی email ایندکس بسازی:

```js
db.users.createIndex({
  email: 1,
});
```

حالا MongoDB محل ایمیل‌ها را از قبل می‌داند و سریع‌تر به نتیجه می‌رسد.

عدد 1 و -1 یعنی ایندکس های صعودی و نزولی

```js
db.users.createIndex({
  age: 1,
});

db.users.createIndex({
  age: -1,
});
```

فرض کن فروشگاه داری.

کاربر هر روز این کوئری را اجرا می‌کند:

اگر روی category ایندکس نداشته باشی، هر بار همه محصولات بررسی می‌شوند.

```js
db.products.find({
  category: 'Laptop',
});

// سرعت جستجو بسیار بیشتر می‌شود
db.products.createIndex({
  category: 1,
});
```

فرض کن ایمیل کاربران نباید تکراری باشد.

حالا اگر دو کاربر با یک ایمیل ثبت شوند، MongoDB خطا می‌دهد.

این دقیقاً مشابه UNIQUE در SQL است.

```js
db.users.createIndex({ email: 1 }, { unique: true });
```

گاهی روی دو فیلد با هم جستجو می‌کنی. به این می‌گویند Compound Index.

```js
db.users.find({
  city: 'Qom',
  age: 27,
});

db.users.createIndex({
  city: 1,
  age: 1,
});
```

مشاهده Indexها - حذف Index

```js
db.users.getIndexes();

db.users.dropIndex('email_1');
```

### Aggregation Pipeline

قابلیتی در MongoDB است که داده‌ها را در چند مرحله (Stage) پردازش می‌کند و برای گزارش‌گیری، گروه‌بندی، محاسبات آماری و تبدیل داده‌ها استفاده می‌شود.قابلیتی در MongoDB است که داده‌ها را در چند مرحله (Stage) پردازش می‌کند و برای گزارش‌گیری، گروه‌بندی، محاسبات آماری و تبدیل داده‌ها استفاده می‌شود.

داخل آرایه، مراحل پشت سر هم اجرا می‌شوند.

```js
db.products.aggregate([Stage1, Stage2, Stage3]);
```

مثل find() است.

```js
// $match

db.products.aggregate([
  {
    $match: { category: 'Laptop' },
  },
]);
```

می‌خواهیم بدانیم از هر دسته چند محصول داریم.

```js
db.products.aggregate([
  {
    $group: { _id: '$category', total: { $sum: 1 } },
  },
]);
```

```json
[
  {
    "_id": "Laptop",
    "total": 2
  },
  {
    "_id": "Phone",
    "total": 3
  }
]
```

ترکیب چند Stage

فقط لپ‌تاپ‌ها میانگین قیمتشان را حساب کن.

```js
db.products.aggregate([
  {
    $match: { category: 'Laptop' },
  },
  {
    $group: { _id: '$category', averagePrice: { $avg: '$price' } },
  },
]);
```

### طراحی دیتابیس در MongoDB

داده را داخل همان Document ذخیره کنیم یا در Collection جدا؟

این همان تصمیم بین Embed و Reference است.

روش اول: Embed

یعنی داده‌های مرتبط را داخل همان Document ذخیره کنیم.

```json
{
  "_id": 1,
  "name": "Amin",
  "address": {
    "city": "Qom",
    "street": "Imam"
  }
}
```

آدرس داخل خود کاربر است. - نیازی به Query دوم نیست. - سرعت خواندن بسیار خوب است.

چه زمانی Embed کنیم؟

وقتی:

داده همیشه همراه موجودیت اصلی خوانده می‌شود - اندازه داده زیاد نیست - داده مستقل از موجودیت اصلی نیست.

مثال‌ها:

آدرس کاربر - تنظیمات کاربر - آیتم‌های یک سفارش - اطلاعات پروفایل

روش دوم: Reference

حالا فرض کن کاربران و پست‌ها را داریم

اگر تمام پست‌ها را داخل کاربر ذخیره کنیم:

```json
{
  "name": "Amin",
  "posts": [
      ...
      ...
      ...
      ... // هزاران پست
  ]
}
```

این Document بسیار بزرگ می‌شود.

و Collection کاربران

```json
{
  "_id": 1,
  "name": "Amin"
}
```

و Collection پست‌ها

فقط شناسه کاربر را نگه می‌داریم. این یعنی Reference.

```json
{
  "_id": 20,
  "title": "...",
  "userId": 1
}
```

چه زمانی Embed و چه زمانی Reference؟

و Embed زمانی مناسب است که داده کوچک، وابسته و معمولاً همراه با موجودیت اصلی خوانده شود.

و Reference زمانی مناسب است که داده بزرگ، مستقل، قابل اشتراک یا دارای تغییرات زیاد باشد.
