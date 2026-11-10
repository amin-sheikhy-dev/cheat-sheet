```ts
@Module({
  imports: [
    // یعنی 15 تا درخواست در 60 ثانیه
    // این به صورت سراسری هست و روی تمام روت ها اعمال میشه
    // بعضی روت های حساس باید محدودیت بیشتری داشته باشن که داخل خود اون روت ها اعمال میکنیم
    ThrottlerModule.forRootAsync({
      inject: [ConfigService],

      useFactory: (config: ConfigService) => ({
        throttlers: [{ ttl: 60_000, limit: 15 }], // توی 60 ثانیه فقط 15 بار میشه به هر روت ریکوئست زد
        skipIf: () => config.get('NODE_ENV') === 'development', // اگه تو حالت توسعه بودیم بیخیالش شو
      }),
    }),
  ],

  providers: [{ provide: APP_GUARD, useClass: ThrottlerGuard }],
})
export class AppModule implements NestModule {}
```

```ts
// فقط روی یه روت
@UseGuards(ThrottlerGuard)
@Get()
someRoute() {}

// این گارد روی همه روت ها اعمال میشه بدون اینکه همه جا از روش پایین استفاده کنیم
providers: [{ provide: APP_GUARD, useClass: ThrottlerGuard }]
```

برای روت های حساس رو خودمون میتونیم تنظیم کنیم

```ts
@Post('signup')
@Throttle({ default: { ttl: 60_000, limit: 3 } })
signup(@Body() userData: SignUpDto) {
  return this.authService.signup(userData);
}
```

اگه یه روت بخوای کلاً از throttle معاف بشه

```ts
@SkipThrottle()
@Get('health')
healthCheck() {}
```
