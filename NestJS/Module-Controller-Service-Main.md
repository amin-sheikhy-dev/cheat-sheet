# main.ts

```ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  const config = app.get(ConfigService);

  app.use(cookieParser());

  app.enableShutdownHooks();

  app.enableCors({ origin: config.get('FRONTEND_URL'), credentials: true });

  app.useGlobalPipes(new ValidationPipe({ transform: true, whitelist: true }));

  if (config.getOrThrow('NODE_ENV') === 'development') {
    const swaggerConfig = new DocumentBuilder()
      .setTitle('Paradise Hotel API')
      .setDescription('API documentation for the hotel reservation system')
      .addBearerAuth()
      .addCookieAuth('refresh_token')
      .build();

    const document = SwaggerModule.createDocument(app, swaggerConfig);

    SwaggerModule.setup('docs', app, document, { swaggerOptions: { docExpansion: 'none' } });
  }

  await app.listen(Number(config.getOrThrow('PORT')));
}

await bootstrap();
```

استفاده از فستیفای

- `npm i @nestjs/platform-fastify`
- `npm rm @nestjs/platform-express`

```ts
import { NestFactory } from '@nestjs/core';
import { FastifyAdapter, NestFastifyApplication } from '@nestjs/platform-fastify';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create<NestFastifyApplication>(AppModule, new FastifyAdapter());
  await app.listen(3000, '0.0.0.0');
}

bootstrap();
```

# module

`users.module.ts`

```ts
@Module({
  // بخای از سرویس یا ماژول استفاده کنی باید اول ایمپورتش کنی
  imports: [TypeOrmModule.forFeature([User]), SessionModule],

  // یعنی: این کنترلر رو بشناس و روت هاش رو ثبت کن
  controllers: [UsersController, UsersAdminController, UsersProfileController],
  // ولی در عمل معمولاً هر ماژول یه کنترلر داره چون اگه منطق زیاد شد بهتره ماژول رو بشکنی
  // تقسیم به چند کنترلر توی یه ماژول بیشتر وقتی منطقیه که روت ها خیلی زیاد باشن ولی همه به یه سرویس وابسته باشن

  // service, guard, interceptor, pipe :باشه providers داره باید توی @Injectable() هر چیزی که
  providers: [UsersService],
  // در صورتی که یه گارد یا پایپ رو فقط توی اون ماژول لازم داشته باشیم

  // اگه بخای بقیه ماژول ها بتونن از این کنترلر استفاده کنن باید اکسپورتش کنی بعد در ماژول مقصد مثل بالا ماژول یوزر رو ایمپورت کنی
  exports: [UsersService],
})
export class UsersModule {}
```

# controller

```ts
@Controller('users')
// وقتی از یه گارد استفاده میکنیم یوزر رو میتونیم از طریق دکوراتور بگیریم
// rolesGuard بعد jwtGuard ترتیب گارد ها مهمه اول
@UseGuards(JwtGuard, RolesGuard)
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  @Roles(Role.Admin, Role.Owner)
  getUsers(@CurrentUser('role') role: Role, @Query('email', ParseEmailPipe) email?: string) {
    return this.usersService.getVisibleUsers(role, email);
  }

  @Post()
  @Roles(Role.Owner)
  addUser(@Body() userData: CreateUserDto) {
    return this.usersService.addUser(userData);
  }

  // خیلی مهمه که این روت قبل داینامیک روت باشه مگرنه به عنوان داینامیک روت شناخته میشه
  @Get('me')
  getMe(@CurrentUser('id') id: string) {
    return this.usersService.getById(id);
  }

  @Patch('me')
  updateMe(@CurrentUser('id') id: string, @Body() userData: UpdateUserDto) {
    return this.usersService.updateUserInformation(id, userData);
  }

  @Delete('me')
  deleteMe(@CurrentUser('id') id: string) {
    return this.usersService.removeUser(id);
  }

  @Get(':id')
  @Roles(Role.Admin, Role.Owner)
  getOne(@CurrentUser() actor: AuthenticatedUser, @Param('id', ParseUUIDPipe) id: string) {
    return this.usersService.getVisibleById(actor, id);
  }
  // روش خام برای گرفتن کاربر از ریکوئست
  getOne(@Req() req: Request) {
    const user = req.user;
  }

  @Delete(':id')
  @Roles(Role.Owner)
  removeUser(@Param('id', ParseUUIDPipe) id: string) {
    return this.usersService.removeUser(id);
  }

  @Patch(':id/role')
  @Roles(Role.Owner)
  updateRole(@Param('id', ParseUUIDPipe) id: string, @Body() { role }: UpdateRoleDto) {
    return this.usersService.updateRole(id, role);
  }
}
```

# service

```ts
@Injectable()
export class UsersService {
  constructor(
    private readonly sessionService: SessionService // تزریق یه سرویس - از قبل باید ماژول اون سرویس داخل ماژول یوزر ثبت شده باشه
  ) {}

  // methods ...
}
```
