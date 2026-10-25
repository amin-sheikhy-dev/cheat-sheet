# Nodemailer

کتابخانه ارسال ایمیل در Node.js - با SMTP سرویس‌هایی مثل Gmail کار می‌کند.

## نصب

```bash
npm i nodemailer
```

```bash
npm i -D @types/nodemailer
```

## ساخت Transporter

```ts
import nodemailer from 'nodemailer';

const transporter = nodemailer.createTransport({
  host: process.env.SMTP_HOST,
  port: 465,
  secure: true, // پورت 465 هست true برای

  auth: {
    user: process.env.SMTP_USER,
    pass: process.env.SMTP_PASS, // Gmail پسورد اپ نیست پسورد حساب - برای
  },
});
```

## ارسال ایمیل ساده

```ts
await transporter.sendMail({
  from: '"Amin" <amin@example.com>', // اسم نمایشی + آدرس
  to: 'ali@example.com, sara@example.com', // میتونه چند نفر باشه
  subject: 'Hello',

  text: 'متن ساده', // اگر ساپورت HTML نکنه کلاینت

  html: '<h1>Hello</h1><p>This is an email</p>', // ترجیح داده میشه
});
```

نتیجه:

```json
{
  "accepted": ["ali@example.com"],
  "rejected": [],
  "messageId": "<b658f8ca-6296-ccf4-8306-87d57a669782@example.com>"
}
```

## Gmail

برای Gmail باید App Password ساخت (نه پسورد عادی حساب):

1. فعال کردن 2-Step Verification روی حساب گوگل
2. ساخت App Password از myaccount.google.com/apppasswords
3. استفاده از اون 16 کاراکتر به عنوان pass

```ts
const transporter = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    user: 'aminsheikhy59@gmail.com',
    pass: 'app password 16 char',
  },
});
```

## محتوای HTML تمیز

```ts
await transporter.sendMail({
  from: '"Shop" <noreply@shop.com>',
  to: user.email,
  subject: 'Welcome!',
  html: `
    <div style="font-family: sans-serif; max-width: 500px; margin: auto;">
      <h2>سلام ${user.name}</h2>
      <p>به فروشگاه ما خوش اومدی</p>
      <a href="https://shop.com" style="background: #2563eb; color: #fff; padding: 10px 20px; border-radius: 8px; text-decoration: none;">
        شروع کن
      </a>
    </div>
  `,
});
```

## قالب با EJS

```ts
import ejs from 'ejs';

const html = await ejs.renderFile('views/emails/welcome.ejs', {
  name: user.name,
  link: verifyLink,
});

await transporter.sendMail({
  from: '"Shop" <noreply@shop.com>',
  to: user.email,
  subject: 'Welcome',
  html,
});
```

```ejs
<!-- views/emails/welcome.ejs -->
<h2>سلام <%= name %></h2>
<a href="<%= link %>">تایید ایمیل</a>
```

## ارسال فایل ضمیمه

```ts
await transporter.sendMail({
  from: '"Shop" <noreply@shop.com>',
  to: user.email,
  subject: 'Your invoice',
  html: '<p>فاکتور شما ضمیمه است</p>',

  attachments: [
    { filename: 'invoice.pdf', path: './files/invoice.pdf' },

    // فایل از روی دیسک - filename اسمیه که گیرنده میبینه
    { filename: 'report.pdf', path: '/path/to/report.pdf' },

    // فایل از روی محتوا
    { filename: 'hello.txt', content: 'Hello world' },
  ],
});
```

## ایمیل تایید ثبت‌نام (مثال واقعی)

```ts
import crypto from 'node:crypto';
import nodemailer from 'nodemailer';

const transporter = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    user: process.env.SMTP_USER,
    pass: process.env.SMTP_PASS,
  },
});

async function sendVerificationEmail(to: string, token: string) {
  const verifyUrl = `https://mysite.com/verify-email?token=${token}`;

  await transporter.sendMail({
    from: '"My Site" <noreply@mysite.com>',
    to,
    subject: 'تایید ایمیل',
    html: `
      <p>برای تایید ایمیل روی لینک زیر کلیک کن</p>
      <a href="${verifyUrl}">تایید ایمیل</a>
      <p>این لینک 15 دقیقه اعتبار دارد</p>
    `,
  });
}

// استفاده در روت ثبت‌نام
const token = crypto.randomBytes(32).toString('hex');

await sendVerificationEmail(user.email, token);
```

## تست بدون ارسال واقعی

```ts
const transporter = nodemailer.createTransport({
  jsonTransport: true,
});

const info = await transporter.sendMail({
  from: 'test@test.com',
  to: 'user@example.com',
  subject: 'Test',
  html: '<p>Test</p>',
});

console.log(info.message); // خود ایمیل به صورت JSON
```

و برای دیدن پیش‌نمایش واقعی ایمیل‌ها موقع توسعه، سایت Ethereal را ببین - یک اکانت تستی می‌سازه و ایمیل‌ها را در مرورگر نشون میده:

```ts
const testAccount = await nodemailer.createTestAccount();

const transporter = nodemailer.createTransport({
  host: 'smtp.ethereal.email',
  port: 587,
  auth: {
    user: testAccount.user,
    pass: testAccount.pass,
  },
});

const info = await transporter.sendMail({ ... });

console.log(nodemailer.getTestMessageUrl(info)); // لینک پیش‌نمایش در مرورگر
```

## استفاده در NestJS

```ts
// npm i @nestjs-modules/mailer nodemailer
// npm i -D @types/nodemailer

import { MailerService } from '@nestjs-modules/mailer';

@Injectable()
export class MailService {
  constructor(private readonly mailerService: MailerService) {}

  async sendWelcome(to: string, name: string) {
    await this.mailerService.sendMail({
      to,
      subject: 'Welcome',
      template: 'welcome', // templates/welcome.ejs
      context: { name },
    });
  }
}
```

```ts
// app.module.ts
MailerModule.forRootAsync({
  inject: [ConfigService],
  useFactory: (config: ConfigService) => ({
    transport: {
      host: config.get('SMTP_HOST'),
      port: 465,
      secure: true,
      auth: {
        user: config.get('SMTP_USER'),
        pass: config.get('SMTP_PASS'),
      },
    },
    defaults: { from: '"My Site" <noreply@mysite.com>' },
    template: { dir: 'src/templates', adapter: new EjsAdapter(), options: { strict: true } },
  }),
})
```
