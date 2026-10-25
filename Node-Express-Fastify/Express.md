# Express

فریم‌ورک وب برای Node.js - ساخت HTTP سرور، مسیریابی (Routing)، Middleware و API.

## نصب و راه‌اندازی

```bash
npm i express
```

```bash
npm i -D @types/express @types/node
```

```bash
npm i tsx -D
```

```bash
npx tsx watch src/server.ts
```

## اولین سرور

```ts
import express from 'express';

const app = express();

// برای خواندن req.body به صورت JSON - بدون این خط req.body برابر undefined هست
app.use(express.json());

app.get('/', (req, res) => {
  res.json({ message: 'hello' });
});

app.listen(3000, () => {
  console.log('Server running on http://localhost:3000');
});
```

```bash
npm run dev
```

## مسیریابی (Routing)

```ts
import express from 'express';

const app = express();
app.use(express.json());

app.get('/users', (req, res) => {});

app.post('/users', (req, res) => {});

app.put('/users/:id', (req, res) => {});

app.patch('/users/:id', (req, res) => {});

app.delete('/users/:id', (req, res) => {});
```

```bash
curl http://localhost:3000/users
```

## req - درخواست

```ts
app.get('/users/:id/posts/:postId', (req, res) => {
  // پارامترهای مسیر
  req.params.id; // '12'
  req.params.postId; // '5'

  // کوئری استرینگ - /users/search?q=amin&page=2
  req.query.q; // 'amin'
  req.query.page; // '2'

  // هدرها
  req.headers.authorization;

  // بدنه درخواست - نیاز به express.json() داره
  req.body;
});
```

## res - پاسخ

```ts
res.json({ message: 'ok' }); // پاسخ JSON

res.status(201).json({ user }); // با کد وضعیت دلخواه

res.send('Hello'); // متن

res.redirect('/login'); // ریدایرکت

res.download('/files/report.pdf'); // دانلود فایل
```

کدهای وضعیت رایج:

```text
200 موفقیت
201 ساخته شد - بعد از POST
204 موفق بدون محتوا - بعد از DELETE
400 درخواست نامعتبر - خطای اعتبارسنجی
401 احراز هویت نشده
403 دسترسی نداری
404 پیدا نشد
409 تضاد - مثل ایمیل تکراری
500 خطای سرور
```

## Middleware

تابعی که بین درخواست و روت قرار می‌گیرد. ترتیب مهم است - فقط برای روت‌های بعدی اعمال می‌شود.

```ts
app.use(express.json());
// خودش یک میدلور هست - برای همه روت‌ها

const logger = (req: express.Request, res: express.Response, next: express.NextFunction) => {
  console.log(`${req.method} ${req.url}`);

  next(); // بدون next() درخواست همینجا گیر می‌کنه
};

app.use(logger);

// میدلور فقط برای یک روت
app.get('/users', logger, (req, res) => {});

// میدلور برای همه روت‌های یک مسیر
app.use('/api', logger);
```

## ساخت API استاندارد (REST)

```ts
interface User {
  id: number;
  name: string;
  email: string;
}

let users: User[] = [{ id: 1, name: 'Amin', email: 'amin@gmail.com' }];

// لیست همه
app.get('/api/users', (req, res) => {
  res.json(users);
});

// گرفتن یکی
app.get('/api/users/:id', (req, res) => {
  const user = users.find((u) => u.id === Number(req.params.id));

  if (!user) {
    return res.status(404).json({ error: 'User not found' });
  }

  res.json(user);
});

// ساخت
app.post('/api/users', (req, res) => {
  const { name, email } = req.body;

  if (!name || !email) {
    return res.status(400).json({ error: 'Name and email are required' });
  }

  const newUser = { id: Date.now(), name, email };

  users.push(newUser);

  res.status(201).json(newUser);
});

// ویرایش
app.put('/api/users/:id', (req, res) => {
  const user = users.find((u) => u.id === Number(req.params.id));

  if (!user) {
    return res.status(404).json({ error: 'User not found' });
  }

  user.name = req.body.name ?? user.name;
  user.email = req.body.email ?? user.email;

  res.json(user);
});

// حذف
app.delete('/api/users/:id', (req, res) => {
  users = users.filter((u) => u.id !== Number(req.params.id));

  res.status(204).send();
});
```

## Router - تفکیک روت‌ها

```ts
// routes/users.routes.ts
import { Router } from 'express';

const router = Router();

router.get('/', (req, res) => {});

router.get('/:id', (req, res) => {});

export default router;
```

```ts
// server.ts
import usersRouter from './routes/users.routes';

app.use('/api/users', usersRouter);
```

## اعتبارسنجی ورودی با Zod

```ts
import { z } from 'zod';

const createUserSchema = z.object({
  name: z.string().min(2),
  email: z.string().email(),
});

app.post('/api/users', (req, res) => {
  const result = createUserSchema.safeParse(req.body);

  if (!result.success) {
    return res.status(400).json(result.error.flatten().fieldErrors);
  }

  // result.data.name
  // result.data.email

  res.status(201).json({ message: 'created' });
});
```

## مدیریت خطا

باید 4 آرگومان داشته باشه تا express بفهمه error handler هست

```ts
app.use((err: Error, req: express.Request, res: express.Response, next: express.NextFunction) => {
  console.error(err);

  res.status(500).json({ error: 'Internal server error' });
});
```

روت 404 - باید قبل از error handler باشد:

```ts
app.use((req, res) => {
  res.status(404).json({ error: 'Route not found' });
});
```

## سرو فایل‌های استاتیک و فولدر عمومی

```ts
import path from 'node:path';

app.use(express.static('public'));

// فایل‌های داخل public از روت در دسترس میشن
// public/style.css -> http://localhost:3000/style.css
```

## متغیر محیطی با dotenv

```bash
npm i dotenv
```

```ts
import 'dotenv/config';

const port = process.env.PORT || 3000;
```

```text
PORT=3000
DATABASE_URL=...
JWT_SECRET=...
```

## ساختار پیشنهادی پروژه

```text
src/
├── server.ts
├── routes/
│   └── users.routes.ts
├── controllers/
│   └── users.controller.ts
├── middlewares/
│   └── logger.ts
└── lib/
    └── db.ts
```

## احراز هویت با JWT

```bash
npm i jsonwebtoken
```

```bash
npm i -D @types/jsonwebtoken
```

```ts
import jwt from 'jsonwebtoken';

app.post('/api/login', (req, res) => {
  const user = { id: 1, email: 'amin@gmail.com' };

  const token = jwt.sign({ userId: user.id }, process.env.JWT_SECRET!, { expiresIn: '15m' });

  res.json({ token });
});
```

```ts
const authMiddleware = (req: express.Request, res: express.Response, next: express.NextFunction) => {
  const token = req.headers.authorization?.split(' ')[1];

  if (!token) {
    return res.status(401).json({ error: 'No token provided' });
  }

  try {
    const payload = jwt.verify(token, process.env.JWT_SECRET!);

    req.userId = payload;
    next();
  } catch {
    res.status(401).json({ error: 'Invalid token' });
  }
};

app.get('/api/profile', authMiddleware, (req, res) => {
  res.json({ message: 'secret data' });
});
```

## CORS

```bash
npm i cors
```

```bash
npm i -D @types/cors
```

```ts
import cors from 'cors';

app.use(cors()); // همه دامنه‌ها

app.use(
  cors({
    origin: 'http://localhost:5173', // فقط این دامنه
    credentials: true, // برای کار با cookie
  })
);
```

## Cookie

```bash
npm i cookie-parser
```

```bash
npm i -D @types/cookie-parser
```

```ts
import cookieParser from 'cookie-parser';

app.use(cookieParser());

// ست کردن
res
  .cookie('token', 'abc123', {
    httpOnly: true, // جاوااسکریپت کلاینت بهش دسترسی نداره
    secure: true, // فقط HTTPS
    maxAge: 15 * 60 * 1000,
  })
  .json({ message: 'logged in' });

// خوندن
req.cookies.token;

// حذف
res.clearCookie('token');
```

## آپلود فایل

برای آپلود فایل از Multer استفاده می‌شود - فایل جداگانه `Multer.md` را ببین.
