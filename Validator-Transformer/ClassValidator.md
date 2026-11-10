# validation decorator

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

# nested dto

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

# mapped types

```ts
export class CreateUserDto {
  name: string;
  email: string;
  password: string;
  age: number;
}
```

> **extends** دقیقا از همون میسازه

```ts
export class SignInDto extends CreateUserDto {}
```

> **PartialType** همرو اختیاری میکنه

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

> **PickType** انتخاب میکنه کدوما باشن

یه پراپرتی هم خودمون اضافه میکنیم

```ts
export class VerifyEmailDto extends PickType(CreateUserDto, ['email']) {
  @Length(4, 4)
  @Matches(/^\d{4}$/, {
    message: 'code must contain exactly 4 digits',
  })
  code: string;
}

// معادل

class UserPreviewDto {
  email: string;
  code: string;
}
```

> **OmitType** انتخاب میکنه کدوما نباشن

```ts
export class PublicUserDto extends OmitType(CreateUserDto, ['password']) {}

// معادل

class PublicUserDto {
  name: string;
  email: string;
  age: number;
}
```

> **IntersectionType** چند تارو با هم ترکیب میکنه

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

> **examples**

```ts
export class CreateReservationDto {
  roomId: string;
  checkIn: string;
  checkOut: string;
  guests: number;
}

// الان داریم میگیم ایدی رو حذف کن و بقیه که موندن رو اخیتاری کن
export class UpdateReservationDto extends PartialType(OmitType(CreateReservationDto, ['roomId'])) {}
```

# custom validator

> async: false

اول اعتبار سنجی رو مینویسیم

```ts
import { ValidatorConstraint, ValidatorConstraintInterface } from 'class-validator';

@ValidatorConstraint({ name: 'isEnglish', async: false })
export class IsEnglishConstraint implements ValidatorConstraintInterface {
  validate(value: unknown): boolean {
    return typeof value === 'string' && /^[a-zA-Z]+$/.test(value);
  }

  defaultMessage(): string {
    return 'Value must contain only English letters';
  }
}
```

تبدیل Validator به Decorator

اما برای استفاده باید اینکارو هم بکنیم

```ts
import { registerDecorator, ValidationOptions } from 'class-validator';
import { IsEnglishConstraint } from './is-english.validator';

export function IsEnglish(validationOptions?: ValidationOptions) {
  return function (object: object, propertyName: string) {
    registerDecorator({
      target: object.constructor,
      propertyName: propertyName,
      options: validationOptions,
      validator: IsEnglishConstraint,
    });
  };
}
```

استفاده

```ts
export class CreateUserDto {
  @IsString()
  @IsEnglish({
    message: 'Username must contain English letters',
  })
  username: string;
}
```

> async: true

فرض کن می‌خوایم بررسی کنیم ایمیل کاربر قبلاً در دیتابیس ثبت نشده باشه

این کار نیاز به ارتباط با دیتابیس داره پس باید از اعتبارسنجی Async استفاده کنیم

```ts
import { ValidatorConstraint, ValidatorConstraintInterface } from 'class-validator';
import { UsersService } from '../users.service';

@Injectable()
@ValidatorConstraint({ name: 'isEmailUnique', async: true })
export class IsEmailUniqueConstraint implements ValidatorConstraintInterface {
  constructor(private readonly usersService: UsersService) {}

  async validate(email: string): Promise<boolean> {
    const user = await this.usersService.findByEmail(email);

    return !user;
  }

  defaultMessage(): string {
    return 'Email already exists';
  }
}
```

تبدیل

```ts
import { registerDecorator, ValidationOptions } from 'class-validator';

import { IsEmailUniqueConstraint } from './is-email-unique.validator';

export function IsEmailUnique(validationOptions?: ValidationOptions) {
  return function (object: object, propertyName: string) {
    registerDecorator({
      target: object.constructor,
      propertyName: propertyName,
      options: validationOptions,
      validator: IsEmailUniqueConstraint,
    });
  };
}
```

```ts
export class SignUpDto {
  @IsEmail()
  @IsEmailUnique({
    message: 'This email is already registered',
  })
  email: string;
}
```

کلاس بالا از UsersService استفاده می‌کنه

ولی class-validator به‌صورت پیش‌فرض کلاس‌های Validator رو از Dependency Injection خود Nest دریافت نمی‌کنه

**`main.ts`**

```ts
import { useContainer } from 'class-validator';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  useContainer(app.select(AppModule), { fallbackOnErrors: true });

  await app.listen(3000);
}

bootstrap();
```

```ts
@Module({
  providers: [UsersService, IsEmailUniqueConstraint],
})
export class UsersModule {}
```

> example

```ts
function IsAfterField(field: string, options?: ValidationOptions) {
  return (object: object, propertyName: string) =>
    registerDecorator({
      name: 'isAfterField',

      target: object.constructor,

      propertyName,

      constraints: [field],

      options: { message: `${propertyName} must be after ${field}`, ...options },

      validator: {
        validate(value: string, args: ValidationArguments) {
          const other = (args.object as any)[args.constraints[0]];

          return typeof value === 'string' && typeof other === 'string' && value > other;
        },
      },
    });
}
```

```ts
function IsNotPastDate(options?: ValidationOptions) {
  return (object: object, propertyName: string) =>
    registerDecorator({
      name: 'isNotPastDate',

      target: object.constructor,

      propertyName,

      options: { message: `${propertyName} cannot be in the past`, ...options },

      validator: {
        validate(value: string) {
          const today = new Date().toISOString().slice(0, 10);

          return typeof value === 'string' && value >= today;
        },
      },
    });
}
```

```ts
export class CreateReservationDto {
  roomId: string;

  @IsNotPastDate()
  checkIn: string;

  @IsAfterField('checkIn')
  checkOut: string;

  guests: number;
}
```
