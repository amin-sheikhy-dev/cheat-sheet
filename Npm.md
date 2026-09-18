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

```text
Ctrl + C
```

---

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

npm i csurf

npm i connect-flash

npm i nodemailer

npm i nodemailer-sendgrid-transport

ialblupykbxdygzk

npm i dotenv

npm i express-validator

npm i zod

npm i multer

npm i jsonwebtoken

npm i nanoid

---

npm i -g @nestjs/cli
nest --version

nest new my-nest-app

npm run start:dev

nest g module messages
nest g service messages
nest g controller messages
مخفف و ساده شده
nest g mo messages
nest g co messages
nest g s messages
nest g controller messages/messages --flat

npm i class-validator class-transformer

npm i typeorm
npm i typeorm@^0.3.20
npm i @nestjs/typeorm

npm i sqlite3

npm i cookie-session @types/cookie-session
