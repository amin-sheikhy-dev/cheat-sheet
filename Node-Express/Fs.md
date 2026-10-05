```ts
import fs from 'node:fs';
import fs from 'node:fs/promises'; // مدرن تر
```

```ts
const data = await fs.readFile('test.txt', 'utf-8');

await fs.writeFile('test.txt', 'Hello Node.js!'); // اگر دوبار روی یک فایل بزنی محتوای جدید جایگزین میشه

// مثال
const data = await fs.readFile('test.txt', 'utf-8');

const newData = data + ' World!';

await fs.writeFile('test.txt', newData);
```

```ts
await fs.appendFile('test.txt', 'New log\n');
// می‌خواهیم به انتهای فایل چیزی اضافه کنیم، بدون اینکه محتوای قبلی پاک شود

// مثال
async function log(message: string) {
  await fs.appendFile('test.txt', `${new Date().toISOString()} - ${message}\n`);
}

await log('Server started');
await log('User logged in');
```

```ts
await fs.unlink('test.txt'); // حذف فایل

// این را نباید برای پوشه استفاده کنی
await fs.unlink('my-folder');
```

```ts
await fs.unlink('test.txt');
await fs.rm('temp');
// حذف فایل یا پوشه

// uploads
// ├── image.jpg
// ├── document.pdf
// └── avatar.png
await fs.rm('uploads', {
  recursive: true, // برای حذف خود پوشه و تمام محتویاتش
});
```

```ts
await fs.mkdir('data');

await fs.mkdir('data/users/images'); // اگه پوشه های دیتا و یوزر نباشه ارور میده

await fs.mkdir('data/users/images', { recursive: true }); // اینجوری ارور نمیده و پوشه هارو میسازه
```

```ts
const files = await fs.readdir('data');
// محتویات یک پوشه

// data
// ├── users.json
// ├── products.json
// └── images/
files = ['images/', 'products.json', 'users.json'];

const items = await fs.readdir('data', {
  withFileTypes: true, // اطلاعات نوع هر ایتم
});

for (const item of items) {
  if (item.isFile()) {
    console.log('File:', item.name);
  }

  if (item.isDirectory()) {
    console.log('Directory:', item.name);
  }
}
```

```ts
await fs.rename('data/old-name.txt', 'data/new-name.txt');
// تغییر نام فایل یا پوشه - جابه‌جا کردن فایل یا پوشه

// تغییر مسیر
await fs.rename('data/user.json', 'backup/user.json');
```

```ts
const filePath = path.join('data', 'users', 'users.json');

console.log(filePath); // data/users/users.json

//

const filePath = '/home/amin/data/users.json';

// نام فایل را از مسیر می‌گیرد
console.log(path.basename(filePath)); // users.json

// پسوند فایل
console.log(path.extname(filePath)); // .json
```

```ts
const info = await fs.stat('test.txt');

console.log(info);

Stats {
  size: 123,
  birthtime: ...,
  mtime: ...,
  ...
}
// ایا این فایله
console.log(info.isFile()); // true

//

const info = await fs.stat('uploads');
// ایا این پوشه اس
console.log(info.isDirectory()); // true

//

const info = await fs.stat('video.mp4');
console.log(info.size); // 5242880 حجم
```

### CRUD

```ts
async function getUsers() {
  const data = await fs.readFile('users.json', 'utf-8');

  return JSON.parse(data);
}

async function createUser(name: string) {
  const data = await fs.readFile('users.json', 'utf-8');

  const users = JSON.parse(data);

  const newUser = { id: Date.now(), name };

  users.push(newUser);

  await fs.writeFile('users.json', JSON.stringify(users, null, 2));

  return newUser;
}

async function updateUser(id: number, name: string) {
  const data = await fs.readFile('users.json', 'utf-8');

  const users = JSON.parse(data);

  const user = users.find((user: any) => user.id === id);

  if (!user) {
    return null;
  }

  user.name = name;

  await fs.writeFile('users.json', JSON.stringify(users, null, 2));

  return user;
}

async function deleteUser(id: number) {
  const data = await fs.readFile('users.json', 'utf-8');

  const users = JSON.parse(data);

  const filteredUsers = users.filter((user: any) => user.id !== id);

  await fs.writeFile('users.json', JSON.stringify(filteredUsers, null, 2));
}
```

```ts
await fs.copyFile('avatar.jpg', 'backup/avatar.jpg');
```
