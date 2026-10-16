### Plain Object => Class Instance

```ts
class User {
  name: string;
  age: number;

  getInfo() {
    return `${this.name} - ${this.age}`;
  }
}

const data = {
  name: 'Amin',
  age: 27,
};

const user = plainToInstance(User, data);

user instanceof User; // true
user.getInfo();

//

const users = [
  { name: 'Amin', age: 27 },
  { name: 'Ali', age: 30 },
];

const result = plainToInstance(User, users);
result[0] instanceof User; // true
result[1] instanceof User; // true
```

### Class Instance => Plain Object

```ts
class UserDto {
  id: number;
  name: string;

  @Exclude()
  password: string;

  constructor(id: number, name: string, password: string) {
    this.id = id;
    this.name = name;
    this.password = password;
  }

  @Expose()
  get fullName() {
    return `User: ${this.name}`;
  }
}

const user = new UserDto(1, 'Amin', '123456');

const result = instanceToPlain(user);
```

```json
{
  "id": 1,
  "name": "Amin",
  "fullName": "User: Amin"
}
```

### Type - Transform

```ts
export class UsersDto {
  @Type(() => Number)
  age: number;

  // مهم - این باعث باگ میشه چون استرینگ فالس به بولین که تبدیل بشه میشه ترو و باگ میشه
  @Type(() => Boolean)
  isActive: boolean;

  // روش درست استفاده از اینه
  @Transform(({ value }) => {
    if (value === 'true') return true;
    if (value === 'false') return false;

    return value;
  })
  @IsBoolean()
  isActive: boolean;
  // 'true' => true
  // 'false' => false
  // 'hello' => error

  @Transform(({ value }) => (typeof value === 'string' ? value.trim() : value))
  @IsString()
  name: string;
}
```

### Expose - Exclude

```ts
export class User {
  id: number;
  name: string;
  email: string;

  @Exclude()
  password: string;
}

//

const user = new User();

user.id = 1;
user.name = 'Amin';
user.email = 'amin@example.com';
user.password = 'secret123';

const result = instanceToPlain(user);
```

```json
{
  "id": 1,
  "name": "Amin",
  "email": "amin@example.com"
}
```

```ts
@Exclude()
export class UserResponseDto {
  @Expose()
  id: number;

  @Expose()
  name: string;

  @Expose()
  email: string;

  password: string;
}
```

### Groups

```ts
class User {
  id: number;
  name: string;
  email: string;
  role: string;
}

@Exclude()
export class User {
  @Expose()
  id: number;

  @Expose()
  name: string;

  @Expose({ groups: ['admin'] })
  email: string;

  @Expose({ groups: ['admin'] })
  role: string;
}

const user = new User();
user.id = 1;
user.name = 'Amin';
user.email = 'amin@example.com';
user.role = 'admin';

// برای کاربر عادی
instanceToPlain(user);

// برای ادمین
instanceToPlain(user, {
  groups: ['admin'],
});
```

### Nested Objects - @Type()

```ts
export class CustomerDto {
  @IsString()
  name: string;

  @IsEmail()
  email: string;
}

export class ProductDto {
  @IsString()
  name: string;

  @IsInt()
  quantity: number;
}

export class CreateOrderDto {
  // یک ابجکت تو در تو
  @ValidateNested()
  @Type(() => CustomerDto)
  customer: CustomerDto;

  // آرایه‌ای از ابجکت ها
  @IsArray()
  @ValidateNested({ each: true })
  @Type(() => ProductDto)
  products: ProductDto[];
}
```

```json
{
  "customer": {
    "name": "Amin",
    "email": "amin@example.com"
  },
  "products": [
    {
      "name": "Keyboard",
      "quantity": 2
    },
    {
      "name": "Mouse",
      "quantity": 1
    }
  ]
}
```
