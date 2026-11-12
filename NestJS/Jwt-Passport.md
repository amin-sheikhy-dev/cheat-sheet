# JWT Strategy

اینجا مشخص میکنی توکن رو از کجا بخونه مثلا کوکی یا هدر یا بقیه جاها

```ts
//o امضا شده است access token داخل decode چیزی که بعد از
interface JwtAccessPayload {
  sub: string;
  email: string;
  role: string;
  iat?: number;
  exp?: number;
}

// می‌گیره CurrentUser برمی‌گردونه و validate چیزی که
export interface AuthenticatedUser {
  id: string;
  email: string;
  role: Role;
}

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor(private readonly config: ConfigService) {
    super({
      // برمی‌دارد Authorization: Bearer <token> توکن را از
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),

      // تاریخ انقضا را چک می‌کند - یعنی ایگنور نمیکنه
      ignoreExpiration: false,

      // امضا را تایید می‌کند secret با این
      secretOrKey: config.getOrThrow('JWT_SECRET'),
    });
  }

  // گزاشته میشه req.user به عنوان req این چیزیه که داخل
  validate(payload: JwtAccessPayload): AuthenticatedUser {
    return { id: payload.sub, email: payload.email, role: payload.role as Role };
  }
}
```

# Auth Guard

برای اینه که به کاربرانی که احراز هویت نشدن اجازه ورود نده

```ts
import { Injectable, UnauthorizedException } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class JwtGuard extends AuthGuard('jwt') {
  // برمی‌گرداند TokenExpiredError خطاها را با پیام خودش مثلا passport
  // می‌کنیم override را handleRequest برای پیام خطای یکنواخت و بدون لو دادن جزئیات
  handleRequest<TUser = unknown>(err: unknown, user: false | TUser | undefined): TUser {
    if (err || !user) {
      throw new UnauthorizedException('invalid or expired access token');
    }

    return user;
  }
}
```

# Module

توی ماژولی که بخای استفاده کنی

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

    TypeOrmModule.forFeature([Otp, PasswordReset]),

    PassportModule, // فعال بشه Nest برای اینکه پاسپورت توی
  ],
  controllers: [TestModule],
  providers: [TestModule, JwtStrategy],
})
export class TestModule {}
```

بعد حالا هرجا خاستی استفاده کنی

```ts
private getJwtToken({ id, email, role }: AuthenticatedUser) {
  if (!id || !role || !email) {
    throw new Error('getJwtToken: user object is missing required fields (id, email, role)');
  }

  return this.jwtService.sign({ sub: id, email: email, role: role });
}
```
