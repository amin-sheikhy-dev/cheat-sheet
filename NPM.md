## تغییر Registry

```bash
npm config set registry https://mirror-npm.runflare.com
```

```bash
npm config set registry https://mirror2.chabokan.net/npm/
```

```bash
npm config set registry https://package-mirror.liara.ir/repository/npm/
```

بازگرداندن Registry پیش‌ فرض

```bash
npm config set registry https://registry.npmjs.org/
```

مشاهده Registry فعلی

```bash
npm config get registry
```

تست اتصال npm

```bash
npm ping
```

بررسی نصب بودن یک پکیج

```bash
npm list packageName
```

مشاهده نسخه npm

```bash
npm --version
```

مشاهده نسخه Node.js

```bash
node --version
```

توقف اجرای دستور

```bash
Ctrl + C
```

## کش کردن پکیج ها

```bash
npm install -g verdaccio
```

```bash
npm set registry http://localhost:4873/
```

```bash
npm config delete registry
```

زدن این دستور در ترمینال و بازکردن ادرس http://localhost:4873 مرورگر

```bash
verdaccio
```

## ساخت پروژه با Vite

```bash
npm create vite@latest
```

```bash
npm i
```

```bash
npm run dev
```

---

## ساخت پروژه با Next.js

```bash
npx create-next-app@latest
```

```bash
npm run dev
```

### برای Build گرفتن

```bash
npm run build
```

```bash
npm start
```

---

## نصب Tailwind CSS 3

```bash
npm i -D tailwindcss@3 postcss autoprefixer
```

```bash
npx tailwind init -p
```

فایل‌های زیر باید ساخته شوند:

- tailwind.config.js
- postcss.config.js

## تنظیم tailwind.config.js

```js
/** @type {import('tailwindcss').Config} */
export default {
  darkMode: 'class', // برای دارک مود ضروریه
  content: ['./src/**/*.{html,js,jsx,ts,tsx}', './public/index.html'],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

## تنظیم global.css یا index.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## Dark Mode

```css
@layer base {
  body {
    @apply bg-white dark:bg-gray-900;
  }
}
```

## مرتب‌ سازی کلاس‌ های Tailwind

```bash
npm i -D prettier prettier-plugin-tailwindcss
```

ابتدا ساخت یک فایل
`.prettierrc`
و قرار دادن متن زیر داخلش

```
.prettierrc
```

```json
{
  "plugins": ["prettier-plugin-tailwindcss"],
  "printWidth": 150,
  "singleQuote": true,
  "jsxSingleQuote": true,
  "tsxSingleQuote": true,
  "semi": true,
  "trailingComma": "es5",
  "bracketSpacing": true,
  "tabWidth": 2,
  "useTabs": false,
  "arrowParens": "always",
  "htmlWhitespaceSensitivity": "ignore"
}
```

---

# کتابخانه‌ها

## React Router

```bash
npm i react-router-dom
```

## Redux Toolkit

```bash
npm i @reduxjs/toolkit
```

```bash
npm i react-redux
```

## Zustand

```bash
npm i zustand
```

## React Query

```bash
npm i @tanstack/react-query
```

## React Icons

```bash
npm i react-icons
```

## Framer Motion

```bash
npm i framer-motion
```

## Swiper

```bash
npm i swiper
```

## Supabase

```bash
npm i @supabase/supabase-js
```

## SQLite

```bash
npm i better-sqlite3
```

```bash
npm i sqlite3
```

## Next Themes

```bash
npm i next-themes
```

## Next Client Cookies

```bash
npm i next-client-cookies
```

## Next Intl

```bash
npm i next-intl
```

## Jalali Date

```bash
npm i date-fns-jalali
```

## TypeScript (Global)

```bash
npm i -g typescript
```

---

sudo systemctl disable mongod نمی‌خوای دفعه بعد که سیستم روشن شد خودش بالا بیاد
sudo systemctl enable mongod دفعه بعد که سیستم روشن شد خودش بالا بیاد
sudo systemctl stop mongod خاموش
sudo systemctl start mongod روشن

sudo systemctl enable mysql
sudo systemctl disable mysql
sudo systemctl stop mysql
sudo systemctl start mysql

sudo systemctl enable postgresql
sudo systemctl disable postgresql
sudo systemctl stop postgresql
sudo systemctl start postgresql

systemctl is-active mongod mysql postgresql

npm i express
npm start

npm i mysql2

npm i sequelize

npm i mongodb

npm i mongoose

npm i express-session

npm i connect-mongodb-session

npm i bcryptjs
npm i bcrypt
npm i argon2

npm i dotenv

npm i zod

npm i multer

npm i jsonwebtoken

npm i nanoid

---

npm i -g @nestjs/cli
nest --version

nest new my-nest-app

npm run start:dev

nest g mo messages ساخت ماژول
nest g co messages ساخت کنترلر
nest g s messages ساخت سرویس
nest g res messages یه کراد کامل

npm i class-validator class-transformer

npm i typeorm | npm i typeorm@^0.3.20
npm i @nestjs/typeorm

npm i pg
npm i sqlite3

npm i cookie-session @types/cookie-session

---

npm i @nestjs/jwt @nestjs/passport passport passport-jwt bcrypt
npm i -D @types/passport-jwt @types/bcrypt

---

npm i @nestjs/config

npm i @nestjs-modules/mailer nodemailer
npm i -D @types/nodemailer

npm i ejs
npm i -D @types/ejs

npm i @nestjs/swagger

npm i @nestjs/throttler

npm i multer
npm i -D @types/multer

npm i @nestjs/serve-static

npm i @nestjs/schedule

npm i @imagekit/nodejs

npm i @nestjs/mongoose mongoose

استفاده از فستیفای
npm i @nestjs/platform-fastify
npm rm @nestjs/platform-express

```ts
import { NestFactory } from '@nestjs/core';
import { FastifyAdapter, NestFastifyApplication } from '@nestjs/platform-fastify';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create<NestFastifyApplication>(AppModule, new FastifyAdapter());
  await app.listen(3000, '0.0.0.0');
}
bootstrap();
```

npm i -D prisma
npm i @prisma/client @prisma/adapter-pg pg
npx prisma init

npm i -D prisma@7
npm i @prisma/client@7 @prisma/adapter-pg@7 pg
npx prisma init --datasource-provider postgresql

npm i @nestjs/drizzle drizzle-orm pg
npm i -D drizzle-kit @types/pg

---

module,controller,service

validation

nestjs/config => module , class , main.ts

Swagger

Throttler

TypeORM / TypeORM repo

Middleware

upload file
npm i fastify-multer
npm i @nest-lab/fastify-multer

```ts
import { NestFactory } from '@nestjs/core';
import { FastifyAdapter, NestFastifyApplication } from '@nestjs/platform-fastify';
import fastifyMulter from 'fastify-multer';

async function bootstrap() {
  const app = await NestFactory.create<NestFastifyApplication>(AppModule, new FastifyAdapter());

  await app.register(fastifyMulter.contentParser);

  await app.listen(3000, '0.0.0.0');
}
bootstrap();

//

import { FileInterceptor, File } from '@nest-lab/fastify-multer';

@Post()
@UseGuards(JwtGuard, RolesGuard)
@Roles(Role.Admin, Role.Owner)
@UseInterceptors(FileInterceptor('image'))
createRoom(
  @Body() roomData: CreateRoomDto,
  @UploadedFile(imagePipe(true)) image: File,
) {
  return this.roomsService.createRoom(roomData, image);
}
```

npm i @fastify/multipart

```ts
import multipart from '@fastify/multipart';

await app.register(multipart, {
  limits: { fileSize: 2 * 1024 * 1024 },
});

@Post()
@UseGuards(JwtGuard, RolesGuard)
@Roles(Role.Admin, Role.Owner)
async createRoom(@Req() req: FastifyRequest) {
  const file = await req.file();               // فایل
  const buffer = await file.toBuffer();

  // اعتبارسنجی دستی
  if (!/^image\/(jpeg|png|webp)$/.test(file.mimetype)) {
    throw new BadRequestException('Invalid file type');
  }

  // فیلدهای متنی: file.fields
  return this.roomsService.createRoom(/* ... */, buffer);
}
```

nest Exceptions

imagekit - mail

decorator - metadata

jwt - passport

Guards

interceptor

custom pipes

code hash algoritm - jwt & passport algoritm

schedule
