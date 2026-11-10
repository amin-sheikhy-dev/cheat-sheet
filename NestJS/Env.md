## main.ts

```ts
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  const config = app.get(ConfigService); // مقدارشو از اپ میگیری

  await app.listen(Number(config.getOrThrow('PORT')));
}

await bootstrap();
```

### module

```ts
@Module({
  imports: [
    // می‌ریزه process.env رو می‌خونه و مقدارهاش رو توی .env ماژول کانفیگ نست رو راه‌اندازی می‌کنه فایل
    ConfigModule.forRoot({ isGlobal: true }),
    // isGlobal: true اگه این رو نزاری باید توی هر ماژول که بخای ازش استفاده کنی توی اون ماژول ایمپورت کنی

    // داخل ماژول بخای ازش استفاده کنی باید هم تزریقش کنی هم از تابع یوز فکتوری استفاده کنی
    TypeOrmModule.forRootAsync({
      inject: [ConfigService],

      useFactory: (config: ConfigService) => ({
        type: 'postgres',
        host: config.getOrThrow('HOST_DB'),
        port: Number(config.getOrThrow('PORT_DB')),
        username: config.getOrThrow('USERNAME_DB'),
        password: config.getOrThrow('PASSWORD_DB'),
        database: config.getOrThrow('DATABASE_DB'), // getOrThrow اگه نبود ارور بده چون واجبه
        entities: [import.meta.dirname + '/**/*.entity{.ts,.js}'],
        synchronize: config.get('NODE_ENV') === 'development',
      }),
    }),
  ],
})
export class AppModule implements NestModule {}
```

```ts
@Module({
  imports: [
    JwtModule.registerAsync({
      inject: [ConfigService],

      useFactory: (config: ConfigService) => ({
        secret: config.getOrThrow('JWT_SECRET'),
        signOptions: { expiresIn: `${Number(config.getOrThrow('JWT_ACCESS_TTL_MINUTES'))}m` },
      }),
    }),
  ],
})
export class AuthModule {}
```

### class

```ts
@Injectable()
export class AuthService {
  // توی کلس بخاد استفاده بشه باید تزریق شه
  constructor(private readonly config: ConfigService) {}

  test(email, password) {
    this.config.getOrThrow('NODE_ENV') === 'development'
      ? console.log(`--------${code},${email}--------`)
      : await this.mailService.sendOtp(email, code);

    return { message: 'verification code sent' };
  }
}
```
