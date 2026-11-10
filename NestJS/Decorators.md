# Custom Decorator

برای گرفتن ایپی

```ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export const ClientIp = createParamDecorator((data: unknown, context: ExecutionContext) => {
  const request = context.switchToHttp().getRequest();

  return request.ip;
});

// استفاده
@Get()
getSomething(@ClientIp() ip: string) {
  console.log(ip);

  return { ip };
}
```

ساخت دکوراتور برای گرفتن کاربر فعلی

```ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';
import { type AuthenticatedUser } from '../strategies/jwt.strategy.js';

export const CurrentUser = createParamDecorator(
  function (data: keyof AuthenticatedUser | undefined, context: ExecutionContext) {
    const req = context.switchToHttp().getRequest();

    return data ? req.user[data] : req.user;
  }
);

@Get('me') // میشه فقط ایدی کاربر رو گرفت
getMe(@CurrentUser('id') id: string) {
  return this.usersService.getById(id);
}

@Get(':id')
@Roles(Role.Admin, Role.Owner) // میشه کل ابجکت کاربر که شامل ایمیل و نقش و ایدی هست رو گرفت
getOne(@CurrentUser() actor: AuthenticatedUser, @Param('id', ParseUUIDPipe) id: string) {
  // getOne(@Req() request: Request) {}
  // const user = request.user

  return this.usersService.getVisibleById(actor, id);
}
```

برای ست کردن متا دیتا

```ts
import { SetMetadata } from '@nestjs/common';
import { Role } from '../../users/entities/user.entity.js';

export const ROLES_KEY = 'roles';

export function Roles(...roles: Role[]) {
  // SetMetadata برای چسبوندن مقدار اضافی به یه متد یا کلاس استفاده میشه
  return SetMetadata(ROLES_KEY, roles); // ورودی اول میشه کلید و ورودی دوم میشه مقدار اون کلید
}

@Get()
@Roles(Role.Admin, Role.Owner) // استفاده
getUsers() {
  return this.usersService.getUsers();
}
```

# Nest Decorators

- Controller & Routing

```ts
@Controller('users')          // تعریف route prefix
@Get('/list')                 // GET /users/list
@Post()                       // POST /users
@Put(':id')                   // PUT /users/:id
@Patch(':id')                 // PATCH /users/:id
@Delete(':id')                // DELETE /users/:id
@Options()                    // OPTIONS
@Head()                       // HEAD
@All()                        // همه متدها
```

- Parameter

```ts
@Param('id')                  // /users/:id
@Param()                      // /users/123/posts/456 |‌ params: { userId: '123', postId: '456' }
@Query('page')                // /users?page=1
@Query()                      // کل کوئری داخل یه ابجکت میاد
@Body()                       // کل بادی
@Body('name')                 // فقط یه فیلد از بادی
@Headers('authorization')     // یه هدر خاص
@Req()                        // کل request object
@Res()                        // کل response object
@Ip()                         // IP کاربر
@HostParam()                  // subdomain
@UploadedFile()               // یه فایل
@UploadedFiles()              // چند فایل
```

- Module

```ts
@Module({ imports, controllers, providers, exports })
@Global()                     // ماژول رو global کن
@DynamicModule()              // ماژول داینامیک
```

- Provider & Injection

```ts
@Injectable()                 // قابل inject
@Inject('TOKEN')              // inject با token
@Optional()                   // اگه نبود error نده
@InjectRepository(User)       // inject ریپازیتوری TypeORM
```

- Guards, Interceptors, Pipes, Filters

```ts
@UseGuards(JwtGuard)          // اعمال guard
@UseInterceptors(LogInterceptor) // اعمال interceptor
@UsePipes(ValidationPipe)     // اعمال pipe
@UseFilters(HttpExceptionFilter) // اعمال exception filter
```

- Response

```ts
@HttpCode(201)                // کد HTTP response
@Header('Cache-Control', 'none') // set header
@Redirect('/new-url', 301)   // redirect
@Render('index')             // render template
```

- Metadata & Misc

```ts
@SetMetadata('roles', ['admin'])  // ست کردن metadata
@SerializeOptions({ excludeExtraneousValues: true }) // سریالایز
@Version('1')                 // versioning
@Catch(HttpException)         // توی exception filter
```
