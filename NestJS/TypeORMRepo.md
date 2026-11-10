تنظیم main.ts در app.module

```ts
@Module({
  imports: [
    TypeOrmModule.forRootAsync({
      inject: [ConfigService],

      useFactory: (config: ConfigService) => ({
        type: 'postgres',

        host: config.getOrThrow('HOST_DB'),

        port: Number(config.getOrThrow('PORT_DB')),

        username: config.getOrThrow('USERNAME_DB'),

        password: config.getOrThrow('PASSWORD_DB'),

        database: config.getOrThrow('DATABASE_DB'),

        entities: [import.meta.dirname + '/**/*.entity{.ts,.js}'],

        synchronize: config.get('NODE_ENV') === 'development',
      }),
    }),
  ],
})
export class AppModule implements NestModule {}
```

داخل ماژولی که بخای ازش استفاده کنی اول باید اون جدول رو ایمپورت کنی

```ts
@Module({
  imports: [TypeOrmModule.forFeature([Table1, Table2])],

  controllers: [AuthController],

  providers: [AuthService, JwtStrategy],
})
export class AuthModule {}
```

داخل سرویس بخای استفاده کنی باید اول تزریق کنی ریپو رو

```ts
@Injectable()
export class AuthService {
  constructor(
    @InjectRepository(Table1) private readonly table1Repo: Repository<Table1>,
    @InjectRepository(Table2) private readonly table2Repo: Repository<Table2>
  ) {}

  // methods...
}
```
