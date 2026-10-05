```ts
enum Role {
  USER = 'user',
  ADMIN = 'admin',
}

type: 'personal' | 'business';

export class TestDto {
  @IsEmail()
  @IsNotEmpty() // بررسی میکنه مقدار خالی نباشه
  email: string;

  @IsString()
  @MinLength(8) // حداقل تعداد کاراکتر
  @MaxLength(50) // حداکثر تعداد کاراکتر
  password: string;

  @IsString()
  name: string;

  @Matches(/^[a-zA-Z0-9_]+$/) // برای ریجکس
  @Length(3, 20) // حداقل 3 و حداکثر 20
  username: string;

  @IsInt() // فقط اعداد صحیح
  @Min(18)
  age: number;

  @IsNumber() // عدد اعشاری هم قبول میکنه
  price: number;

  @IsEnum(Role)
  role: Role;

  @IsUUID() // 6ac023d7-0258-83ed-b38c-553469994606
  id: string;

  @IsUrl() // https://example.com مثلا
  website: string;

  @IsString()
  @IsOptional() // یعنی این پراپرتی ادرس میتونه اختیاری باشه
  addres?: string; // با علامت سوال فقط به تایپ اسکریپت میگیم اختیاریه

  @IsArray() // بررسی می‌کند مقدار آرایه باشد - به اعضای ارایه کاری نداره
  @IsString({ each: true }) // این بررسی میکنه که همه اعضا استرینگ باشن
  @ArrayMinSize(1) // حداقل تعداد
  @ArrayMaxSize(5) // حداکثر تعداد
  @ArrayUnique() // توی ارایه نباید مقدار تکراری باشه
  tags: string[];

  @IsString()
  @ValidateIf((o) => o.type === 'business') // ارسال شود companyName بود باید business اگر
  companyName?: string;

  @IsIn(['asc', 'desc']) // وقتی مقدار باید یکی از چند مقدار مشخص باشد
  sort: string;

  @IsDate() // مقدار باید تاریخ باشد
  birthDate: Date;

  @IsISO8601() // 2026-10-04T10:30:00Z
  createdAt: string;

  @IsNotEmptyObject()
  settings: object; // "settings": {} رد میشه
}
```

---

```json
{
  "name": "Amin",
  "address": {
    "city": "Qom",
    "postalCode": "1234567890"
  }
}
```

```ts
import { IsString, IsPostalCode, ValidateNested } from 'class-validator';
import { Type } from 'class-transformer';

export class AddressDto {
  @IsString()
  city: string;

  @IsString()
  postalCode: string;
}

export class CreateUserDto {
  @IsString()
  name: string;

  @ValidateNested() // را اجرا کند validation هم قوانین property می‌گوید داخل این validator به
  @Type(() => AddressDto) // ساخته شود AddressDto تو‌در‌تو باید از نوع object به آن می‌گوید هنگام تبدیل داده
  address: AddressDto;
}
```

---

```json
{
  "name": "Amin",
  "addresses": [
    { "city": "Qom", "postalCode": "1234567890" },
    { "city": "Tehran", "postalCode": "0987654321" }
  ]
}
```

```ts
export class CreateUserDto {
  @IsString()
  name: string;

  @IsArray()
  @ValidateNested({ each: true }) // باعث میشه روی تک تک ارایه ها انجام بده
  @Type(() => AddressDto)
  addresses: AddressDto[];
}
```

---

```ts
export class CreateUserDto {
  name: string;
  email: string;
  password: string;
  age: number;
}
```

اختیاری میکنه

```ts
export class UpdateUserDto extends PartialType(CreateUserDto) {}

// معادل

export class UpdateUserDto {
  name?: string;
  email?: string;
  password?: string;
  age?: number;
}
```

انتخاب میکنه کدوما باشن

```ts
export class UserPreviewDto extends PickType(CreateUserDto, ['name', 'email'] as const) {}

// معادل

class UserPreviewDto {
  name: string;
  email: string;
}
```

انتخاب میکنه کدوما نباشن

```ts
export class PublicUserDto extends OmitType(CreateUserDto, ['password'] as const) {}

// معادل

class PublicUserDto {
  name: string;
  email: string;
  age: number;
}
```

ترکیب میکنه

```ts
class UserInfoDto {
  name: string;
  age: number;
}

class UserAccountDto {
  email: string;
  username: string;
}

export class UserDto extends IntersectionType(UserInfoDto, UserAccountDto) {}

// معادل

class UserDto {
  name: string;
  age: number;
  email: string;
  username: string;
}
```
