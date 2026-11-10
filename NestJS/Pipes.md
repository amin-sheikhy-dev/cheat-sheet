در نست Pipe فقط برای Param نیست

```ts
@Param('id', PositiveIntPipe)

@Query('page', PositiveIntPipe)

@Body('age', PositiveIntPipe)
```

# nest pipes

#### `ParseIntPipe => تبدیل به عدد صحیح`

```ts
@Get(':id')
findOne(@Param('id', ParseIntPipe) id: number) {
  return this.usersService.findOne(id);
}

new ParseIntPipe({
  optional: true, // اختیاری میشه
  errorHttpStatusCode: HttpStatus.BAD_REQUEST, // کد خطا
})
```

#### `ParseFloatPipe => تبدیل به عدد اعشاری`

```ts
// GET /products?price=19.5

@Get()
findProducts(@Query('price', ParseFloatPipe) price: number) {
  return this.productsService.findByPrice(price);
}
```

#### `ParseBoolPipe => Boolean تبدیل به`

```ts
// GET /users?active=true

@Get()
findUsers(@Query('active', ParseBoolPipe) active: boolean) {
  return this.usersService.findByStatus(active);
}

// اختیاری میشه
new ParseBoolPipe({ optional: true })

```

#### `ParseArrayPipe => تبدیل و اعتبارسنجی آرایه`

```ts
// GET /users?ids=1,2,3

@Get()
findMany(@Query('ids', new ParseArrayPipe({ items: Number, separator: ',' })) ids: number[]) {
  return this.usersService.findMany(ids); // [1, 2, 3]
}

new ParseArrayPipe({
  items: Number,  // نوع آیتم‌ ها
  separator: ',', // جدا کننده — پیش‌ فرضش همینی هست که الانه
  optional: true,
})
```

#### `ParseUUIDPipe => UUID اعتبارسنجی`

```ts
// GET /users/6ae6df7a-672c-83ed-a069-3fbac8313c9d

@Get(':id')
findOne(@Param('id', ParseUUIDPipe) id: string) {
  return this.usersService.findOne(id);
}

new ParseUUIDPipe({
  optional: true,
  version: '4',   // نسخه UUID — '3' | '4' | '5'
})
```

#### `ParseEnumPipe => Enum اعتبارسنجی مقادیر`

```ts
export enum ReservationStatus {
  PENDING = 'pending',
  CONFIRMED = 'confirmed',
  CANCELLED = 'cancelled',
}

@Get()
findReservations(@Query('status', new ParseEnumPipe(ReservationStatus)) status: ReservationStatus) {
  return this.reservationsService.findByStatus(status);
}

new ParseEnumPipe(ReservationStatus, {
  optional: true,
  errorHttpStatusCode: HttpStatus.BAD_REQUEST,
})
```

#### `DefaultValuePipe => تعیین مقدار پیش‌فرض`

```ts
// GET /users

// GET /users?limit=20

@Get()
findAll(@Query('limit', new DefaultValuePipe(10), ParseIntPipe) limit: number) {
  return this.usersService.findAll(limit);
}
```

#### `ParseFilePipe => اعتبارسنجی فایل آپلودشده`

```ts
@Post('upload')
@UseInterceptors(FileInterceptor('file'))
uploadFile(
  @UploadedFile(
    new ParseFilePipe({
      validators: [new MaxFileSizeValidator({ maxSize: 2 * 1024 * 1024 }), new FileTypeValidator({ fileType: 'image/jpeg' })],
    }),
  )
  file: Express.Multer.File,
) {
  return file;
}
```

# custom pipes

مثال

```ts
export class CreateUserDto {
  @IsEmail()
  email: string;
  @IsString()
  password: string;
}

//
class EmailDto extends PickType(CreateUserDto, ['email']) {}

@Injectable()
export class ParseEmailPipe implements PipeTransform {
  async transform(value: unknown) {
    // این خط پایین در صورتی بزار که ارسال ایمیل اختیاری باشه
    if (value === undefined || value === '') {
      return undefined;
    }

    const obj = plainToInstance(EmailDto, { email: value });
    const errors = await validate(obj);

    if (errors.length > 0) {
      throw new BadRequestException(errors);
    }

    return obj.email;
  }
}
```

مثال

```ts
@Injectable()
export class ParseRolePipe implements PipeTransform {
  private readonly allowedRoles = [Role.Admin, Role.User];

  transform(value: unknown): Role.Admin | Role.User {
    if (typeof value !== 'string') {
      throw new BadRequestException('role must be a string');
    }

    if (!this.allowedRoles.includes(value as Role)) {
      throw new BadRequestException(`role must be one of: ${this.allowedRoles.join(', ')}`);
    }

    return value as Role.Admin | Role.User;
  }
}
```

مثال

```ts
@Injectable()
export class ParseNumberPipe implements PipeTransform {
  transform(value: string): number {
    const parsedValue = Number(value);

    if (Number.isNaN(parsedValue)) {
      throw new BadRequestException('Value must be a number');
    }

    return parsedValue;
  }
}
```

مثال

```ts
@Injectable()
export class PositiveIntPipe implements PipeTransform {
  transform(value: string): number {
    const parsedValue = Number(value);

    if (!Number.isInteger(parsedValue) || parsedValue <= 0) {
      throw new BadRequestException('ID must be a positive integer');
    }

    return parsedValue;
  }
}
```

مثال

```ts
@Injectable()
export class RangePipe implements PipeTransform {
  constructor(
    private readonly min: number,
    private readonly max: number
  ) {}

  transform(value: string): number {
    const parsedValue = Number(value);

    if (!Number.isInteger(parsedValue) || parsedValue < this.min || parsedValue > this.max) {
      throw new BadRequestException(`Value must be between ${this.min} and ${this.max}`);
    }

    return parsedValue;
  }
}

// استفاده
@Get(':id')
findOne(@Param('id', new RangePipe(1, 100)) id: number) {
  return this.usersService.findOne(id);
}
```
