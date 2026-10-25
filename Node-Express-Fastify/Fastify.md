# Fastify

فریم‌ورک وب برای Node.js - جایگزین سریع‌تر Express با تایپ‌اسکریپت بهتر، لاگر داخلی و اعتبارسنجی با Schema.

فرق اصلی با Express:

- سرعت بالاتر (از سریع‌ترین فریم‌ورک‌های Node)
- لاگینگ داخلی با Pino - بدون پکیج اضافه
- اعتبارسنجی و سریالایز داخلی با JSON Schema - بدون Zod و...
- اکوسیستم plugin محور - هر قابلیت یک پکیج `@fastify/...`

## نصب و اولین سرور

```bash
npm i fastify
```

```bash
npm i tsx -D
```

```bash
npx tsx watch src/server.ts
```

```ts
import Fastify from 'fastify';

const app = Fastify({
  logger: true, // لاگر داخلی Pino - لاگ خودکار هر درخواست
});

app.get('/', async (request, reply) => {
  return { message: 'hello' }; // برگرداندن آبجکت = پاسخ JSON
});

// host: '0.0.0.0' برای اینکه از شبکه و داکر هم در دسترس باشه
await app.listen({ port: 3000, host: '0.0.0.0' });
```

> در Fastify هندلرها async هستند و **return** می‌کنیم - به جای res.json - خطا هم با **throw** داده میشه - به جای next(err)

## مسیریابی (Routing)

```ts
app.get('/users/:id', async (request, reply) => {
  return { id: request.params.id };
});
// پارامتر مسیر

// کوئری استرینگ - /users/search?q=amin&page=2
app.get('/users/search', async (request) => {
  const { q, page } = request.query;

  return { q, page };
});

// بدنه درخواست
app.post('/users', async (request, reply) => {
  const user = request.body;

  reply.code(201); // کد وضعیت

  return user;
});

app.put('/users/:id', async (request) => {});

app.patch('/users/:id', async (request) => {});

app.delete('/users/:id', async (request, reply) => {
  reply.code(204);

  return; // بدون محتوا
});
```

## request و reply

```ts
request.params; // پارامترهای مسیر - { id: '12' }
request.query; // کوئری استرینگ - { q: 'amin' }
request.body; // بدنه JSON
request.headers; // هدرها
request.headers.authorization;
request.ip; // آی‌پی کلاینت
request.url; // مسیر کامل
```

```ts
reply.code(201); // کد وضعیت
reply.header('x-custom', 'value'); // ست کردن هدر
reply.redirect('/login'); // ریدایرکت
reply.send(data); // پاسخ - یا با return بدهیم
```

## تایپ body و params در TypeScript

با جنریک‌های Fastify:

```ts
interface CreateBody {
  name: string;
  email: string;
}

interface IdParams {
  id: string;
}

app.post<{ Body: CreateBody }>('/users', async (request, reply) => {
  // request.body از نوع CreateBody هست
  const { name, email } = request.body;
});

app.get<{ Params: IdParams }>('/users/:id', async (request) => {
  // request.params.id از نوع string
  return { id: request.params.id };
});
```

## Schema - اعتبارسنجی داخلی

کشنده‌ترین قابلیت Fastify - با JSON Schema ورودی اعتبارسنجی و خروجی سریالایز می‌شود:

```ts
const createBodySchema = {
  type: 'object',

  required: ['name', 'email'],

  properties: {
    name: { type: 'string', minLength: 2 },

    email: { type: 'string', format: 'email' },

    age: { type: 'integer', minimum: 18 },

    role: { type: 'string', enum: ['user', 'admin'] },
  },

  additionalProperties: false, // فیلد اضافه خطا بده
};

app.post(
  '/users',
  {
    schema: {
      body: createBodySchema, // اعتبارسنجی ورودی - نامعتبر = خطای 400 خودکار
    },
  },
  async (request) => {
    // از اینجا به بعد body تضمین شده معتبره
    return { created: true };
  }
);
```

schema برای خروجی - فیلدهای حساس مثل پسورد از پاسخ حذف می‌شوند:

```ts
app.get(
  '/users/:id',
  {
    schema: {
      response: {
        200: {
          type: 'object',

          properties: {
            id: { type: 'string' },
            name: { type: 'string' },
            // password تعریف نشده = در پاسخ نمیاد - مثل Exclude در class-transformer
          },
        },
      },
    },
  },
  async (request) => {
    return { id: '1', name: 'Amin', password: 'secret' }; // password خروجی نمیره
  }
);
```

## استفاده از Zod به جای JSON Schema

```bash
npm i fastify-type-provider-zod zod
```

```ts
import { z } from 'zod';
import { serializerCompiler, validatorCompiler, jsonSchemaTransform, type ZodTypeProvider } from 'fastify-type-provider-zod';

const app = Fastify({ logger: true }).withTypeProvider<ZodTypeProvider>();

app.setValidatorCompiler(validatorCompiler);
app.setSerializerCompiler(serializerCompiler);

const createUserSchema = z.object({
  name: z.string().min(2),
  email: z.string().email(),
});

app.post(
  '/users',
  {
    schema: {
      body: createUserSchema,
    },
  },
  async (request) => {
    // request.body تایپ‌شده بر اساس زاد - از روی schema
    const { name, email } = request.body;

    return { name, email };
  }
);
```

## Plugin - ساختار افزونه‌ای

هر قابلیت با `register` اضافه می‌شود و در یک context ایزوله (encapsulated) اجرا می‌شود:

```bash
npm i @fastify/cors
```

```ts
import cors from '@fastify/cors';

await app.register(cors, {
  origin: 'http://localhost:5173',
  credentials: true,
});
```

prefix برای گروهی از روت‌ها:

```ts
// routes/users.routes.ts
import { FastifyInstance } from 'fastify';

export async function usersRoutes(app: FastifyInstance) {
  app.get('/', async () => {
    return []; // مسیر نهایی: /api/users
  });

  app.get('/:id', async (request) => {});
}

// server.ts
await app.register(usersRoutes, { prefix: '/api/users' });
```

### Encapsulation

هر register یک context جدید می‌سازد - decorator و hook فقط داخل همان context و فرزندانش کار می‌کنند:

این decorator فقط داخل usersRoutes کار میکنه

```ts
await app.register(async (users) => {
  users.decorate('helper', () => '...');

  users.get('/a', async () => {});
});

// اینجا app.helper وجود نداره
```

اگر بخواهیم چیزی همه‌جا در دسترس باشد از `fastify-plugin` استفاده می‌کنیم:

```bash
npm i fastify-plugin
```

```ts
import fp from 'fastify-plugin';

// fp wrapper باعث میشه encapsulation دور زده بشه
export default fp(async (app) => {
  app.decorate('myHelper', () => '...');
});
```

## Decorator - اضافه کردن قابلیت به fastify

```ts
app.decorate('config', { dbName: 'shop' }); // مقدار ثابت

// به request - مثلا userId بعد از احراز هویت
app.decorateRequest('userId', '');

// به reply
app.decorateReply('success', function (this: any, data: unknown) {
  return this.code(200).send({ success: true, data });
});

// استفاده در روت
app.get('/me', async (request, reply) => {
  return reply.success({ id: 1 });
});
```

## Hooks - چرخه حیات درخواست

ترتیب اجرا برای هر درخواست:

```text
onRequest → preParsing → preValidation → preHandler → (هندلر) → preSerialization → onSend → onResponse
```

```ts
app.addHook('onRequest', async (request, reply) => {
  request.startTime = Date.now();
}); // لاگ دستی - اول همه چیز

// احراز هویت - قبل از هندلر
app.addHook('preHandler', async (request, reply) => {
  if (!request.headers.authorization) {
    reply.code(401);

    throw new Error('Unauthorized');
  }
});

// آخر کار - بعد از ارسال پاسخ
app.addHook('onResponse', async (request, reply) => {
  console.log(`${request.method} ${request.url} - ${reply.statusCode}`);
});

// خطاها
app.addHook('onError', async (request, reply, error) => {
  console.error(error);
});
```

## مدیریت خطا

```ts
import { FastifyError, FastifyReply, FastifyRequest } from 'fastify';

app.setErrorHandler((error: FastifyError, request: FastifyRequest, reply: FastifyReply) => {
  // خطاهای اعتبارسنجی Schema - fastify خودش statusCode=400 میده
  if (error.validation) {
    return reply.code(400).send({
      error: 'Validation failed',
      fields: error.validation,
    });
  }

  // خطای دستی با کد مشخص
  reply.code(error.statusCode ?? 500);

  return reply.send({ message: error.message });
});

// روت‌های پیدانشده
app.setNotFoundHandler((request, reply) => {
  reply.code(404).send({ error: `Route ${request.method} ${request.url} not found` });
});
```

خطای سفارشی - با throw:

```ts
class HttpError extends Error {
  constructor(
    public statusCode: number,
    message: string
  ) {
    super(message);
  }
}

// در هندلر
throw new HttpError(403, 'Access denied');
```

## احراز هویت با JWT

```bash
npm i @fastify/jwt
```

```ts
import fastifyJwt from '@fastify/jwt';
import fp from 'fastify-plugin';

async function auth(app: FastifyInstance) {
  await app.register(fastifyJwt, {
    secret: process.env.JWT_SECRET!,
  });

  // decorator که همه روت‌ها ازش استفاده کنن
  app.decorate('authenticate', async (request: FastifyRequest, reply: FastifyReply) => {
    try {
      await request.jwtVerify(); // توکن از هدر Authorization: Bearer ... خوانده و بررسی میشه
    } catch (err) {
      reply.code(401);

      throw new Error('Invalid or expired token');
    }
  });
}

export default fp(auth); // fp تا authenticate همه‌جا در دسترس باشه
```

ساخت توکن در لاگین

```ts
app.post('/login', async (request) => {
  const token = app.jwt.sign({ userId: 1 }, { expiresIn: '15m' });

  return { token };
});

// محافظت از روت
app.get('/profile', { preHandler: [app.authenticate] }, async (request) => {
  // request.user از jwtVerify پر شده
  return { user: request.user };
});
```

## Cookie

```bash
npm i @fastify/cookie
```

```ts
import fastifyCookie from '@fastify/cookie';

await app.register(fastifyCookie, { secret: process.env.COOKIE_SECRET });

// خواندن
request.cookies.token;

// ست کردن
reply.setCookie('token', 'abc123', {
  httpOnly: true,
  secure: process.env.NODE_ENV === 'production',
  sameSite: 'lax',
  path: '/',
  maxAge: 15 * 60, // ثانیه - برخلاف express که میلی‌ثانیه است
});

// حذف
reply.clearCookie('token');
```

## آپلود فایل

```bash
npm i @fastify/multipart
```

```ts
import multipart from '@fastify/multipart';

await app.register(multipart, {
  limits: {
    fileSize: 2 * 1024 * 1024, // 2 مگابایت
  },
});

app.post('/upload', async (request, reply) => {
  const file = await request.file(); // فایل و فیلدهای فرم

  if (!file) {
    throw new HttpError(400, 'No file uploaded');
  }

  // اعتبارسنجی نوع فایل
  if (!/^image\/(jpeg|png|webp)$/.test(file.mimetype)) {
    throw new HttpError(400, 'Invalid file type');
  }

  const buffer = await file.toBuffer(); // محتوا در حافظه

  // file.filename - file.mimetype - file.fields = فیلدهای متنی فرم

  return { filename: file.filename, size: buffer.length };
});
```

> برای نوشتن روی دیسک: `import { pipeline } from 'node:stream/promises'` و `await pipeline(file.file, fs.createWriteStream(path))`

## سرو فایل‌های استاتیک

```bash
npm i @fastify/static
```

```ts
import fastifyStatic from '@fastify/static';
import path from 'node:path';

await app.register(fastifyStatic, {
  root: path.join(__dirname, 'public'),
  prefix: '/static/', // http://localhost:3000/static/style.css
});
```

## قالب با EJS

```bash
npm i @fastify/view ejs
```

```ts
import view from '@fastify/view';
import ejs from 'ejs';

await app.register(view, {
  engine: { ejs },
  root: path.join(__dirname, 'views'),
});

app.get('/welcome', async (request, reply) => {
  return reply.view('welcome.ejs', { name: 'Amin' });
});
```

## متغیر محیطی

```bash
npm i dotenv
```

```ts
import 'dotenv/config';

await app.listen({ port: Number(process.env.PORT) || 3000 });
```

## خاموشی تمیز - Graceful Shutdown

```ts
for (const signal of ['SIGINT', 'SIGTERM']) {
  process.on(signal, async () => {
    // اتصال دیتابیس و... را ببند
    await app.close();

    process.exit(0);
  });
}
```

## ساختار پیشنهادی پروژه

```text
src/
├── server.ts
├── app.ts
├── routes/
│   └── users.routes.ts
├── plugins/
│   └── auth.ts
├── schemas/
│   └── user.schema.ts
└── lib/
    └── db.ts
```

## Express vs Fastify

| -           | Express                          | Fastify                 |
| ----------- | -------------------------------- | ----------------------- |
| سرعت        | خوبه                             | سریع‌تر                 |
| لاگر        | پکیج جدا (morgan)                | داخلی با Pino           |
| اعتبارسنجی  | پکیج جدا (Zod/express-validator) | داخلی با Schema         |
| هندلر       | callback با req/res              | async با return/throw   |
| اکوسیستم    | قدیمی‌تر و بزرگ‌تر               | پکیج‌های `@fastify/...` |
| maxAge کوکی | میلی‌ثانیه                       | ثانیه                   |

## Fastify در NestJS

Nest به صورت پیش‌فرض از Express استفاده می‌کند ولی آداپتور Fastify هم دارد - سریع‌تر است:

```bash
npm i @nestjs/platform-fastify
npm rm @nestjs/platform-express
```

```ts
// main.ts
import { NestFactory } from '@nestjs/core';
import { FastifyAdapter, NestFastifyApplication } from '@nestjs/platform-fastify';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create<NestFastifyApplication>(AppModule, new FastifyAdapter());

  await app.listen(3000, '0.0.0.0');
}
bootstrap();
```

تفاوت‌ها در Nest:

- `@Res()` نمی‌شود استفاده کرد مگر با passthrough - چون Fastify reply متفاوت است
- آپلود فایل با `@fastify/multipart` یا `@nest-lab/fastify-multer` - نه multer معمولی
