```ts
type User = {
  id: number;
  name: string;
  email: string;
  age: number;
};
```

> # Utility Type

# Partial

تمام Property ها رو اختیاری می‌کنه

```ts
type UpdateUser = Partial<User>;

// نتیجه
type UpdateUser = {
  id?: number;
  name?: string;
  email?: string;
  age?: number;
};
```

# Required

تمام Property ها رو اجباری می‌کنه

اگر Property ای اختیاری باشه اجباری می‌شه

```ts
type CompleteUser = Required<User>;
```

# Readonly

تمام Property ها رو فقط خواندنی می‌کنه دیگه نمی‌تونی Property ها رو تغییر بدی

```ts
type ReadonlyUser = Readonly<User>;

// نتیجه
const user: ReadonlyUser = {
  id: 1,
  name: 'Amin',
  email: 'test@test.com',
  age: 25,
};

user.name = 'Ali'; // Error
```

# Pick

فقط Property های مشخص‌ شده رو انتخاب می‌کنه

```ts
type UserPreview = Pick<User, 'id' | 'name'>;

// نتیجه
type UserPreview = {
  id: number;
  name: string;
};
```

# Omit

و Property های مشخص‌ شده رو حذف می‌کنه

```ts
type CreateUser = Omit<User, 'id'>;

// نتیجه
type CreateUser = {
  name: string;
  email: string;
  age: number;
};
```

---

> # Union Type

# Exclude

اعضایی از Union رو که با U سازگار هستن حذف می‌کنه

```ts
type Status = 'pending' | 'success' | 'failed';

type Result = Exclude<Status, 'pending'>;

// 'success' | 'failed'
```

# Extract

فقط اعضایی رو نگه می‌داره که با U سازگار هستن

```ts
type Status = 'pending' | 'success' | 'failed';

type Result = Extract<Status, 'success' | 'failed'>;

// 'success' | 'failed'
```

# NonNullable

و null و undefined رو حذف می‌کنه

```ts
type Value = string | number | null | undefined;

type Result = NonNullable<Value>;

// string | number
```

# Record

یک Object Type با کلیدهای مشخص و Value های مشخص می‌سازه

```ts
type Role = 'admin' | 'user' | 'owner';

type Permissions = Record<Role, string[]>;

const permissions: Permissions = {
  admin: ['create', 'delete'],
  user: ['read'],
  owner: ['read', 'write'],
};
```

# ReturnType

و Type خروجی یک Function رو استخراج می‌کنه

```ts
function getUser() {
  return { id: 1, name: 'Amin' };
}

type User = ReturnType<typeof getUser>;

// نتیجه
type User = {
  id: number;
  name: string;
};
```

# Parameters

و Type پارامترهای یک Function رو به شکل Tuple استخراج می‌کنه

```ts
function createUser(name: string, age: number) {}

type Args = Parameters<typeof createUser>;

// [name: string, age: number]
```

# ConstructorParameters

پارامترهای Constructor یک Class رو استخراج می‌کنه

```ts
class User {
  constructor(
    public name: string,
    public age: number
  ) {}
}

type Args = ConstructorParameters<typeof User>;

// [name: string, age: number]
```

# InstanceType

و Type نمونه ساخته‌شده از یک Class رو استخراج می‌کنه

```ts
class User {
  name = 'Amin';
}

type UserInstance = InstanceType<typeof User>;
```

# Awaited

و Type داخل Promise رو استخراج می‌کنه؛ حتی اگر Promise تو در تو باشه

```ts
type Result = Awaited<Promise<Promise<User>>>;

// User
```
