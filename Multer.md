# Multer

کتابخانه‌ای برای مدیریت آپلود فایل در Express. خود Express به‌صورت پیش‌فرض فقط متن و JSON را می‌فهمد و برای multipart/form-data (فرمی که فایل دارد) به Multer نیاز است.

## نصب

```bash
npm i multer
```

```bash
npm i -D @types/multer
```

## فرم HTML سمت کلاینت

فرم آپلود فایل حتماً باید `enctype` زیر را داشته باشد وگرنه فایل ارسال نمی‌شود:

```tsx
<form action="/profile" method="POST" encType="multipart/form-data">
  <input type="file" name="avatar" />
  <button type="submit">Upload</button>
</form>
```

## راه‌اندازی اولیه

```ts
import express from 'express';
import multer from 'multer';

const app = express();

// فایل‌های آپلود شده به صورت موقت در این پوشه ذخیره میشوند
const upload = multer({ dest: 'uploads/' });

app.post('/profile', upload.single('avatar'), (req, res) => {
  // اسم فیلد فرم را به single میدهیم

  console.log(req.file); // اطلاعات فایل آپلود شده

  console.log(req.body); // بقیه فیلدهای متنی فرم

  res.json({ message: 'uploaded' });
});
```

## انواع آپلود

```ts
// one file - req.file
upload.single('avatar');

// چند فایل با فیلدهای مختلف - req.files به صورت آبجکت
upload.fields([
  { name: 'avatar', maxCount: 1 },
  { name: 'gallery', maxCount: 8 },
]);

// چند فایل با یک فیلد - req.files به صورت آرایه
upload.array('gallery', 8); // حداکثر 8 فایل

// فقط متن - هیچ فایلی قبول نکنه ولی فیلدهای فرم پر بشن
upload.none();
```

## ذخیره با نام و پسوند صحیح (diskStorage)

با `dest` فایل بدون پسوند و با نام تصادفی ذخیره می‌شود. با diskStorage کنترل کامل روی نام و مسیر فایل داریم:

```ts
import multer from 'multer';
import path from 'node:path';

const storage = multer.diskStorage({
  // فایل کجا ذخیره شود
  destination: (req, file, cb) => {
    cb(null, 'uploads/');
  },

  // اسم فایل چطور ساخته شود
  filename: (req, file, cb) => {
    const uniqueSuffix = Date.now() + '-' + Math.round(Math.random() * 1e9);

    cb(null, file.fieldname + '-' + uniqueSuffix + path.extname(file.originalname));
  },
});

const upload = multer({ storage });
```

## فیلتر نوع فایل

```ts
import multer from 'multer';

const fileFilter = (req: express.Request, file: Express.Multer.File, cb: multer.FileFilterCallback) => {
  // فقط عکس مجاز است
  if (/^image\/(jpeg|png|webp)$/.test(file.mimetype)) {
    cb(null, true);
  } else {
    cb(new Error('Only image files are allowed'));
  }
};

const upload = multer({ storage, fileFilter });
```

## محدودیت حجم

```ts
const upload = multer({
  storage,
  limits: {
    fileSize: 2 * 1024 * 1024, // حداکثر 2 مگابایت
  },
});
```

## نگه داشتن فایل در حافظه (memoryStorage)

فایل روی دیسک نوشته نمی‌شود و به صورت Buffer در `req.file.buffer` هست - برای آپلود مستقیم به S3 یا Cloudinary مناسبه:

```ts
const upload = multer({ storage: multer.memoryStorage() });

app.post('/upload', upload.single('avatar'), (req, res) => {
  console.log(req.file?.buffer); // Buffer
});
```

## مدیریت خطا

```ts
import multer from 'multer';

const upload = multer({ dest: 'uploads/' });

app.post('/profile', (req, res) => {
  upload.single('avatar')(req, res, (err) => {
    if (err instanceof multer.MulterError) {
      // خطاهای خود multer مثل LIMIT_FILE_SIZE
      return res.status(400).json({ error: err.code });
    }

    if (err) {
      // خطای فیلتر خودمون مثل Only image files are allowed
      return res.status(400).json({ error: err.message });
    }

    res.json({ message: 'uploaded', file: req.file });
  });
});
```

## سرو کردن فایل‌های استاتیک

```ts
import express from 'express';

app.use('/uploads', express.static('uploads'));

// حالا فایل از این آدرس در دسترسه
// http://localhost:3000/uploads/avatar-1234.jpg
```

## مثال کامل با Zod

اعتبارسنجی فیلدهای متنی فرم با Zod و فایل با multer:

```ts
import { z } from 'zod';

const createProductSchema = z.object({
  title: z.string().min(2),
  price: z.coerce.number().positive(),
});

app.post('/products', upload.single('image'), (req, res) => {
  const result = createProductSchema.safeParse(req.body);

  if (!result.success) {
    return res.status(400).json({ errors: result.error.flatten().fieldErrors });
  }

  // result.data.title
  // result.data.price
  // req.file.filename

  res.json({ message: 'created' });
});
```

> در Fastify از `@fastify/multipart` یا `fastify-multer` استفاده می‌شود - نمونه‌اش در فایل NPM.md هست.
