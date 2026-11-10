# Native — Node.js

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

  return storedHash === hash.toString('hex');
}
```

# bcrypt

```bash
npm i bcrypt
npm i -D @types/bcrypt
```

```ts
import * as bcrypt from 'bcrypt';

// هش کردن
const hash = await bcrypt.hash(password, 10); // 10 = saltRounds

// مقایسه
const isMatch = await bcrypt.compare(password, hash); // true | false
```

---

# argon2

```bash
npm i argon2
```

```ts
import * as argon2 from 'argon2';

// هش کردن — ساده
const hash = await argon2.hash(password);

// هش کردن — با تنظیمات دستی
const hash = await argon2.hash(password, {
  type: argon2.argon2id, // امن‌ترین نوع — ترکیب argon2i و argon2d
  memoryCost: 2 ** 16, // 64MB حافظه
  timeCost: 3, // تعداد iteration
  parallelism: 1, // تعداد thread
});

// مقایسه
const isMatch = await argon2.verify(hash, password); // true | false
```
