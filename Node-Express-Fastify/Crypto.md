## createHash — هش کردن داده

```ts
import { createHash } from 'crypto';

// sha256
const hash = createHash('sha256').update('your text').digest('hex');
// a948904f2f0f479b8f936...

// sha512
const hash = createHash('sha512').update('your text').digest('hex');

// md5 — فقط برای checksum، نه امنیت
const hash = createHash('md5').update('your text').digest('hex');

// خروجی به فرمت‌های مختلف
createHash('sha256').update('your text').digest('hex'); // رشته hex
createHash('sha256').update('your text').digest('base64'); // رشته base64
createHash('sha256').update('your text').digest(); // Buffer
```

## randomBytes — بایت تصادفی امن

```ts
import { randomBytes } from 'crypto';

// تولید token تصادفی
const token = randomBytes(32).toString('hex'); // 64 کاراکتر hex
const token = randomBytes(32).toString('base64'); // 44 کاراکتر base64

// کاربرد — salt برای هش پسورد
const salt = randomBytes(8).toString('hex'); // 16 کاراکتر

// کاربرد — توکن reset پسورد یا OTP
const resetToken = randomBytes(32).toString('hex');
```

## randomUUID — تولید UUID v4

```ts
import { randomUUID } from 'crypto';

const id = randomUUID();
// 'f47ac10b-58cc-4372-a567-0e02b2c3d479'
```

## randomInt — عدد تصادفی امن

```ts
import { randomInt } from 'crypto';

// بین 0 تا 999999
const otp = randomInt(0, 1_000_000);
// 483920

// با padding — همیشه 6 رقم
const otp = String(randomInt(0, 1_000_000)).padStart(6, '0');
// '048392'
```

## createHmac — امضای پیام

```ts
import { createHmac } from 'crypto';

// ساخت امضا
const signature = createHmac('sha256', secret).update(payload).digest('hex');

// کاربرد رایج — تایید webhook
function verifyWebhook(payload: string, secret: string, receivedSig: string): boolean {
  const expectedSig = createHmac('sha256', secret).update(payload).digest('hex');

  return timingSafeEqual(Buffer.from(expectedSig), Buffer.from(receivedSig));
}
```

## timingSafeEqual — مقایسه امن

```ts
import { timingSafeEqual } from 'crypto';

// ❌ روش ناامن — زمان مقایسه لو میده
if (token === userToken) { ... }

// ✅ روش امن — زمان ثابته، timing attack نداره
const isValid = timingSafeEqual(
  Buffer.from(token),
  Buffer.from(userToken),
);

// کاربرد رایج — تایید OTP یا token
function verifyToken(received: string, expected: string): boolean {
  if (received.length !== expected.length) return false;

  return timingSafeEqual(
    Buffer.from(received),
    Buffer.from(expected),
  );
}
```

## scrypt — هش پسورد با salt

```ts
import { randomBytes, scrypt as _scrypt } from 'crypto';
import { promisify } from 'util';

const scrypt = promisify(_scrypt);

// هش کردن
async function hashPassword(password: string): Promise<string> {
  const salt = randomBytes(8).toString('hex');
  const hash = (await scrypt(password, salt, 32)) as Buffer;
  return `${salt}.${hash.toString('hex')}`;
}

// مقایسه
async function verifyPassword(password: string, stored: string): Promise<boolean> {
  const [salt, storedHash] = stored.split('.');
  const hash = (await scrypt(password, salt, 32)) as Buffer;
  return timingSafeEqual(Buffer.from(storedHash), hash);
}
```
