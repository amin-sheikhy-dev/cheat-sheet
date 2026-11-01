# Swagger (@nestjs/swagger)

داکیومنت تعاملی و خودکار API بر اساس استاندارد OpenAPI - از داخل مرورگر می‌شود درخواست تست کرد.

> Swagger فقط **مستندسازی** میکند - اعتبارسنجی همچنان کار ValidationPipe و class-validator است - این دو به هم ربطی ندارند

## نصب

```bash
npm i @nestjs/swagger
```

> از نسخه ۷ به بعد swagger-ui داخلی است - دیگر نیازی به نصب جداگانه swagger-ui-express نیست

## راه‌اندازی در main.ts

```ts
import { NestFactory } from '@nestjs/core';
import { SwaggerModule, DocumentBuilder } from '@nestjs/swagger';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  const config = new DocumentBuilder()
    .setTitle('Shop API')
    .setDescription('مستندات API فروشگاه')
    .setVersion('1.0')
    .setContact('Amin', 'https://aminsheikhy.dev', 'aminsheikhy59@gmail.com')
    .addTag('users', 'مدیریت کاربران')
    .addTag('rooms', 'مدیریت اتاق ها')
    .addBearerAuth(
      { type: 'http', scheme: 'bearer', bearerFormat: 'JWT' }, // JWT در هدر Authorization
      'access-token' // اسم scheme - در @ApiBearerAuth استفاده میشه
    )
    .build();

  const document = SwaggerModule.createDocument(app, config);

  SwaggerModule.setup('docs', app, document);
  // مسیر داکیومنت: http://localhost:3000/docs
  // خروجی JSON خام: http://localhost:3000/docs-json

  await app.listen(3000);
}
bootstrap();
```

اگر global prefix داری، مسیر داکیومنت مستقل از آن است ولی بهتر است هماهنگ باشد:

```ts
app.setGlobalPrefix('api');

SwaggerModule.setup('api/docs', app, document); // http://localhost:3000/api/docs
```

## گزینه‌های UI

```ts
SwaggerModule.setup('docs', app, document, {
  customSiteTitle: 'Shop API Docs',

  customCss: '.topbar { display: none }', // مخفی کردن نوار بالای صفحه

  swaggerOptions: {
    persistAuthorization: true, // توکن بعد از رفرش صفحه نمیره
    docExpansion: 'none', // لیست‌ها بسته باشن - none | list | full
    filter: true, // فیلد جستجو بین endpoint ها
    tryItOutEnabled: true,
    displayRequestDuration: true, // زمان هر درخواست
  },
});
```

## غیرفعال کردن در Production

```ts
if (process.env.NODE_ENV !== 'production') {
  // کل راه اندازی swagger فقط اینجا
  const config = new DocumentBuilder() /* ... */
    .build();

  const document = SwaggerModule.createDocument(app, config);

  SwaggerModule.setup('docs', app, document);
}
```

## @ApiTags - دسته‌بندی

روی کنترلر - endpoint ها را در UI کنار هم گروه‌بندی می‌کند:

```ts
@ApiTags('users')
@Controller('users')
export class UsersController {}
```

## @ApiProperty - مستندسازی DTO

فیلدهای DTO را برای UI تعریف می‌کند - بدون آن خروجی response در UI خالی است:

```ts
import { ApiProperty, ApiPropertyOptional, ApiHideProperty } from '@nestjs/swagger';

export class CreateUserDto {
  @ApiProperty({
    description: 'نام کاربر',
    example: 'Amin',
    minLength: 2,
    maxLength: 50,
  })
  name: string;

  @ApiPropertyOptional({
    description: 'ایمیل',
    example: 'amin@gmail.com',
    format: 'email',
  })
  email?: string; // با علامت سوال خودش Optional میشه ولی @ApiPropertyOptional خواناتر است

  @ApiProperty({
    description: 'سن',
    example: 27,
    minimum: 18,
    maximum: 100,
    default: 18,
  })
  age: number;

  @ApiProperty({
    description: 'نقش',
    enum: Role, // enum
    example: Role.USER,
  })
  role: Role;

  @ApiProperty({
    description: 'مهارت ها',
    type: [String], // آرایه - داخل براکت نوع اعضا
    example: ['node', 'react'],
  })
  skills: string[];

  @ApiProperty({
    description: 'آدرس',
    type: AddressDto, // آبجکت تو در تو - مثل @ValidateNested
  })
  address: AddressDto;

  @ApiHideProperty() // در داکیومنت نیاد - مثل پسورد
  password: string;
}
```

> روت‌هایی که DTO دارند به صورت خودکار body درست می‌گیرند - فقط کافیست `@Body() dto: CreateUserDto` باشد

## CLI Plugin - حذف نیاز به @ApiProperty

با فعال کردن plugin رسمی، دکوریتورهای @ApiProperty از روی تایپ‌ها و دکوریتورهای class-validator خودکار ساخته می‌شوند:

```json
// nest-cli.json
{
  "compilerOptions": {
    "deleteOutDir": true,
    "plugins": [
      {
        "name": "@nestjs/swagger/plugin",
        "options": {
          "introspectComments": true // کامنت های بالای فیلد = description و example میشن
        }
      }
    ]
  }
}
```

بدون هیچ @ApiProperty ای داکیومنت کامل ساخته میشه

```ts
export class CreateUserDto {
  /** نام کاربر */
  @IsString()
  @MinLength(2)
  name: string;

  /** سن کاربر */
  @IsInt()
  @Min(18)
  age: number;
}
```

## type های ترکیبی - PartialType و...

همان مپد تایپ‌های class-validator ولی برای Swagger هم فیلدها را درست میسازد:

```ts
import { PartialType, PickType, OmitType, IntersectionType } from '@nestjs/swagger';

// همه فیلدها اختیاری - برای آپدیت
export class UpdateUserDto extends PartialType(CreateUserDto) {}

// فقط فیلدهای مشخص
export class UserPreviewDto extends PickType(CreateUserDto, ['name', 'email'] as const) {}

// حذف فیلدهای مشخص - مثل پاسخ بدون پسورد
export class UserResponseDto extends OmitType(CreateUserDto, ['password'] as const) {}

// ترکیب چند DTO
export class FullUserDto extends IntersectionType(UserInfoDto, UserAccountDto) {}
```

## @ApiOperation و @ApiResponse

```ts
@ApiTags('users')
@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Post()
  @ApiOperation({
    summary: 'ساخت کاربر جدید',
    description: 'ایمیل باید منحصر‌به‌فرد باشد',
    // deprecated: true, // علامت خطرناک میخورد
  })
  @ApiCreatedResponse({
    type: UserResponseDto,
    description: 'کاربر ساخته شد',
  })
  @ApiBadRequestResponse({ description: 'داده نامعتبر' })
  @ApiConflictResponse({ description: 'ایمیل تکراری' })
  create(@Body() createUserDto: CreateUserDto) {
    return this.usersService.create(createUserDto);
  }

  @Get(':id')
  @ApiOperation({ summary: 'گرفتن یک کاربر' })
  @ApiOkResponse({ type: UserResponseDto })
  @ApiNotFoundResponse({ description: 'کاربر پیدا نشد' })
  findOne(@Param('id') id: string) {
    return this.usersService.findOne(id);
  }

  @Get()
  @ApiOperation({ summary: 'لیست همه کاربران' })
  @ApiOkResponse({ type: [UserResponseDto] }) // آرایه - داخل براکت
  findAll() {
    return this.usersService.findAll();
  }

  @Delete(':id')
  @ApiBearerAuth('access-token')
  @ApiOperation({ summary: 'حذف کاربر' })
  @ApiNoContentResponse({ description: 'حذف شد' })
  @ApiForbiddenResponse({ description: 'دسترسی نداری' })
  @HttpCode(204)
  remove(@Param('id') id: string) {
    return this.usersService.remove(id);
  }
}
```

کد کامل response ها:

```ts
@ApiOkResponse          // 200
@ApiCreatedResponse     // 201
@ApiNoContentResponse   // 204
@ApiBadRequestResponse  // 400
@ApiUnauthorizedResponse // 401
@ApiForbiddenResponse   // 403
@ApiNotFoundResponse    // 404
@ApiConflictResponse    // 409
@ApiTooManyRequestsResponse // 429
@ApiInternalServerErrorResponse // 500
```

## پارامترها

```ts
@ApiParam({ name: 'id', example: '550e8400-e29b-41d4-a716-446655440000' })
@Get(':id')
findOne(@Param('id') id: string) {}
// پارامتر مسیر

// کوئری استرینگ
@ApiQuery({ name: 'page', required: false, example: 1, type: Number })
@ApiQuery({ name: 'search', required: false, example: 'amin', type: String })
@ApiQuery({ name: 'sort', enum: ['asc', 'desc'] })
@Get()
findAll(@Query('page') page: number, @Query('search') search: string) {}

// هدر دلخواه
@ApiHeaders([{ name: 'x-api-key', required: true, description: 'کلید API' }])
@Get('external')
getExternal() {}

// بدنه دستی - وقتی DTO نداریم یا خواستیم دقیق باشه
@ApiBody({ type: CreateUserDto })
@Post()
create(@Body() body: any) {}
```

## احراز هویت

دو مرحله دارد - اول تعریف scheme در main.ts بعد اعمال روی روت‌ها:

```ts
.addBearerAuth(
  { type: 'http', scheme: 'bearer', bearerFormat: 'JWT' },
  'access-token',
)
// main.ts - قبلا انجام شد
```

```ts
@Get('profile')
@ApiBearerAuth('access-token')
@UseGuards(JwtGuard)
getProfile() {}
// روی یک endpoint

// یا روی کل کنترلر - همه endpoint ها قفل میشن
@ApiBearerAuth('access-token')
@UseGuards(JwtGuard, RolesGuard)
@Controller('rooms')
export class RoomsController {}
```

بعد در UI دکمه Authorize ظاهر می‌شود و توکن به همه درخواست‌های قفل شده اضافه می‌شود - با `persistAuthorization: true` بعد از رفرش هم میماند.

برای API Key:

```ts
// main.ts
.addApiKey({ type: 'apiKey', name: 'x-api-key', in: 'header' }, 'api-key')

// روی endpoint
@ApiSecurity('api-key')
```

## آپلود فایل

```ts
@ApiConsumes('multipart/form-data')
@ApiBody({
  schema: {
    type: 'object',

    properties: {
      file: { type: 'string', format: 'binary' }, // دکمه انتخاب فایل در UI

      title: { type: 'string' }, // فیلدهای متنی فرم
    },

    required: ['file'],
  },
})
@Post('upload')
@UseInterceptors(FileInterceptor('file'))
uploadFile(@UploadedFile() file: Express.Multer.File, @Body('title') title: string) {
  return { filename: file.filename };
}
```

## مخفی کردن endpoint

```ts
@Get('debug')
@ApiExcludeEndpoint()
debug() {}
// یک endpoint از داکیومنت حذف شود

// کل کنترلر حذف شود
@ApiExcludeController()
@Controller('internal')
export class InternalController {}
```

## نکته Fastify

با آداپتور Fastify هم SwaggerModule کار می‌کند - فقط مسیر setup را دقیق صدا بزن (در Fastify ریدایرکت trailing slash خودکار نیست):

```ts
SwaggerModule.setup('docs', app, document);
// دقیقا /docs - نه /docs/
```

## مثال کامل - جریان واقعی

```ts
// main.ts
const config = new DocumentBuilder()
  .setTitle('Reservation API')
  .setVersion('1.0')
  .addTag('rooms')
  .addBearerAuth({ type: 'http', scheme: 'bearer', bearerFormat: 'JWT' }, 'access-token')
  .build();

SwaggerModule.setup('docs', app, SwaggerModule.createDocument(app, config), {
  swaggerOptions: { persistAuthorization: true },
});
```

```ts
// rooms.controller.ts
@ApiTags('rooms')
@ApiBearerAuth('access-token')
@UseGuards(JwtGuard, RolesGuard)
@Roles(Role.Admin, Role.Owner)
@Controller('rooms')
export class RoomsController {
  constructor(private readonly roomsService: RoomsService) {}

  @Post()
  @ApiOperation({ summary: 'ساخت اتاق' })
  @ApiCreatedResponse({ type: RoomResponseDto, description: 'اتاق ساخته شد' })
  createRoom(@Body() createRoomDto: CreateRoomDto) {
    return this.roomsService.createRoom(createRoomDto);
  }

  @Get()
  @ApiOperation({ summary: 'لیست اتاق ها' })
  @ApiOkResponse({ type: [RoomResponseDto] })
  findAll() {
    return this.roomsService.findAll();
  }
}
```

## جمع‌بندی دکوریتورها

| دکوریتور                      | کجا             | کار                                |
| ----------------------------- | --------------- | ---------------------------------- |
| `@ApiTags()`                  | کنترلر          | گروه‌بندی در UI                    |
| `@ApiOperation()`             | endpoint        | خلاصه و توضیح                      |
| `@ApiOkResponse()` و مشابه‌ها | endpoint        | تعریف پاسخ‌ها با type              |
| `@ApiProperty()`              | DTO             | تعریف فیلد                         |
| `@ApiPropertyOptional()`      | DTO             | فیلد اختیاری                       |
| `@ApiHideProperty()`          | DTO             | حذف فیلد از داکیومنت               |
| `@ApiParam()`                 | endpoint        | پارامتر مسیر                       |
| `@ApiQuery()`                 | endpoint        | کوئری استرینگ                      |
| `@ApiHeaders()`               | endpoint        | هدرهای دلخواه                      |
| `@ApiBody()`                  | endpoint        | بدنه دستی                          |
| `@ApiBearerAuth()`            | کنترلر/endpoint | قفل با JWT                         |
| `@ApiSecurity()`              | کنترلر/endpoint | scheme های دیگر مثل api-key        |
| `@ApiConsumes()`              | endpoint        | نوع body - مثل multipart/form-data |
| `@ApiExcludeEndpoint()`       | endpoint        | مخفی کردن                          |
| `@ApiExcludeController()`     | کنترلر          | مخفی کردن کل کنترلر                |
| `PartialType()` و مشابه‌ها    | DTO             | ترکیب DTO ها                       |
