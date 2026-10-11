و `.nullable()` یعنی فیلد باید وجود داشته باشد ولی مقدارش می‌تواند `null` باشد

و `.optional()` یعنی فیلد می‌تواند اصلاً وجود نداشته باشد یا `undefined` باشد

و `skills:` همه اعضای ارایه استرینگ باشن و هر عضو حداقل `2` حرف باشه و حداکثر طول ارایه `10` و حداقل `1` عضو

و `friend:` ارایه از ابجکت ها و هر ابجکت باید مطابق `Schema` مشخص شده باشه

و `.transform((value) => value)` مقدار `معتبر شده` را می‌گیرد و آن را به مقدار دیگری تبدیل می‌کند

و `.coerce` اگه مقدار استرینگ باشه به اونی که میخای تبدیل میکنه

و `.pipe()` مثلا میگیم بعد از تبدیل توقع داریم چی باشه

و `.union()` هم میتونه عدد باشه هم استرینگ

و `preprocess` زمانی مفیده که بخوایم قبل از اجرای `Schema` منطق آماده‌سازی دلخواهی داشته باشیم

اگر بخواهی وجود فیلدهای اضافه باعث خطا شود می‌توانی از `.strict()` استفاده کنی چون در حالت عادی فیلد های اضافی از ابجکت حذف میشن

```ts
import { z } from 'zod';

const testSchema = z
  .object({
    name: z
      .string()
      .trim()
      .transform((value) => value.toUpperCase()),

    nickname: z.string().optional(),

    bio: z.string().nullable(),

    age: z.coerce.int().min(18).max(100).pipe(z.int().positive()),

    birthYear: z.union([z.string(), z.number()]),

    nationalId: z.preprocess((value) => {
      if (typeof value === 'string') {
        return value.trim() === '' ? undefined : Number(value);
      }

      return value;
    }),

    price: z.number().positive(),

    email: z.email().toLowerCase(),

    password: z.string().min(8).max(20),

    confirmPassword: z.string(),

    isAdmin: z.coerce.boolean(), // اینجا کاملا غلطه چون ورودی استرینگ به اسم فالس هم باشه ترو میده

    role: z.enum(['user', 'admin', 'owner']),

    skills: z.array(z.string().min(2)).min(1).max(10),

    friend: z.array(
      z.object({
        name: z.string(),
        age: z.int(),
      })
    ),
  })
  .refine((data) => data.password === data.confirmPassword, {
    // ورودی این تابع کل ابجکت رو میده
    message: 'Passwords do not match', // پیام خطا
    path: ['confirmPassword'], // مشخص میکنه ارور واسه کدوم فیلد باشه
  })
  .refine((data) => data.email.endsWith('@gmail.com'), {
    // البته این مثال خوبی نیست چون خود زاد چک میکنه و این برای اموزشه
    message: 'Only Gmail addresses are allowed',
  })
  .strict();
```

### .discriminatedUnion()

این قابلیت برای داده‌هایی کاربرد داره که بسته به یک فیلد، ساختار متفاوتی دارن

```ts
const paymentSchema = z.discriminatedUnion('method', [
  z.object({
    method: z.literal('card'),
    cardNumber: z.string().min(16),
  }),

  z.object({
    method: z.literal('wallet'),
    walletId: z.string(),
  }),
]);

paymentSchema.safeParse({
  method: 'card',
  cardNumber: '1234567890123456',
}).success; // true

paymentSchema.safeParse({
  method: 'card',
  walletId: 'wallet_123',
}).success; // false
```

### استخراج `Type` و استفاده مجدد از `Schema` ها

```ts
const userSchema = z.object({
  name: z.string(),
  email: z.string().email(),
  age: z.number(),
});

type User = z.infer<typeof userSchema>;
// معادل
type User = {
  name: string;
  email: string;
  age: number;
};
```

### pick - omit - partial - extend

```ts
const userSchema = z.object({
  name: z.string(),
  email: z.string().email(),
  age: z.number(),
  password: z.string().min(8),
});

// فقط ایمیل و اسم رو لازم داریم
const userInfoSchema = userSchema.pick({
  name: true,
  email: true,
});

// جز پسوورد همه رو میخاییم
const publicUserSchema = userSchema.omit({
  password: true,
});

// حالا همشون اختیاری ان
const updateUserSchema = userSchema.partial();

// فقط ایمیل اختیاریه
const updateUserSchema = userSchema.partial({
  email: true,
});

// همه قبلیارو داره به اضافه شهر
const citySchema = userSchema.extend({
  city: z.string().min(1),
});
```

### فرق `parse` و `safeParse`

```ts
try {
  const user = userSchema.parse(data);

  console.log(user);
} catch (error) {
  console.log(error);
}

const result = userSchema.safeParse(data);
// خروجی
{
  success: true,
  data: ...
}
// یا
{
  success: false,
  error: ...
}

if (!result.success) {
  console.log(result.error);

  return;
}

console.log(result.data);
```

### تمرین جالب

و `partial()` همه فیلدها را اختیاری می‌کند پس می‌توانی فقط `name` یا فقط `email` را بفرستی

و `refine()` اجازه نمی‌دهد یک آبجکت خالی `{}` برای ویرایش ارسال شود

```ts
export const updateUserSchema = z
  .object({
    name: z.string().trim().min(2).max(50),
    email: z.string().trim().email(),
  })
  .partial()
  .refine((data) => Object.keys(data).length > 0, {
    message: 'At least one field must be provided',
  });
```
