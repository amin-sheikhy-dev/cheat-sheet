# module

```ts
import { PassportModule } from '@nestjs/passport';
import { JwtModule } from '@nestjs/jwt';

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

    PassportModule,
    MailModule,

    SessionModule,
    UsersModule,
  ],
  controllers: [AuthController],
  providers: [AuthService, JwtStrategy],
})
export class AuthModule {}
```

# controller

```ts
import { Controller, Post, Body, Res, Req, HttpCode } from '@nestjs/common';
import { Cookies, UnauthorizedException, CookieSerializeOptions } from '@nestjs/common';
import { ApiCookieAuth, ApiTags } from '@nestjs/swagger';
import { ConfigService } from '@nestjs/config';
import type { CookieOptions, Request, Response } from 'express';
import { HttpAdapterHost } from '@nestjs/core';

const REFRESH_COOKIE = 'refresh_token';

@ApiTags('Auth')
@Controller('auth')
export class AuthController {
  constructor(
    private readonly adapterHost: HttpAdapterHost,
    private readonly authService: AuthService,
    private readonly config: ConfigService
  ) {}

  // این متد تنظیمات کوکی رو مشخص میکنه
  private get refreshCookieOptions(): CookieSerializeOptions {
    return {
      httpOnly: true, // جاوااسکریپت مرورگر نتونه این مقدار رو بخونه
      secure: this.config.get('NODE_ENV') === 'production',
      sameSite: 'strict',
      path: '/auth',
      // وقتی از دکوراتور های داخلی نست میخای استفاده کنی maxAge به صورت ثانیه حساب میشه نه میلی ثانیه
      maxAge: Number(this.config.getOrThrow('REFRESH_TOKEN_TTL_DAYS')) * 24 * 60 * 60,

      // برای وقتیه که تایپ کوکی رو از اکسپرس بگیری
      maxAge: Number(this.config.getOrThrow('REFRESH_TOKEN_TTL_DAYS')) * 24 * 60 * 60 * 1000,
    };
  }

  @Post('signup')
  @Throttle({ default: { ttl: 60_000, limit: 3 } })
  signup(@Body() userData: SignUpDto) {
    return this.authService.signup(userData);
  }

  @Post('verify-email')
  @Throttle({ default: { ttl: 60_000, limit: 5 } })
  @HttpCode(200)
  @ApiCookieAuth()
  async verifyEmail(@Body() data: VerifyEmailDto, @Res({ passthrough: true }) res: unknown) {
    const { accessToken, refreshToken } = await this.authService.verifyEmail(data);

    res.cookie(REFRESH_COOKIE, refreshToken, this.refreshCookieOptions);
    // روش بالا در صورتی که از ماژول های خود اکسپرس استفاه میکنی و پایین ماژول نیتیو نست
    this.adapterHost.httpAdapter.setCookie(res, REFRESH_COOKIE, refreshToken, this.refreshCookieOptions);

    return { accessToken };
  }

  @Post('signin')
  @Throttle({ default: { ttl: 60_000, limit: 5 } })
  @HttpCode(200)
  @ApiCookieAuth()
  async signin(@Body() userData: SignInDto, @Res({ passthrough: true }) res: unknown) {
    const { accessToken, refreshToken } = await this.authService.signin(userData);

    res.cookie(REFRESH_COOKIE, refreshToken, this.refreshCookieOptions);
    // روش بالا در صورتی که از ماژول های خود اکسپرس استفاه میکنی و پایین ماژول نیتیو نست
    this.adapterHost.httpAdapter.setCookie(res, REFRESH_COOKIE, refreshToken, this.refreshCookieOptions);

    return { accessToken };
  }

  @Post('refresh')
  @Throttle({ default: { ttl: 60_000, limit: 10 } })
  @HttpCode(200)
  @ApiCookieAuth()
  async refresh(@Cookies(REFRESH_COOKIE) token: string | undefined, @Req() req: Request, @Res({ passthrough: true }) res: unknown) {
    const token = req.cookies?.[REFRESH_COOKIE]; // ماژول نیتیو اکسپرس

    if (!token) throw new UnauthorizedException('missing refresh token');

    const { accessToken, refreshToken } = await this.authService.refresh(token);

    res.cookie(REFRESH_COOKIE, refreshToken, this.refreshCookieOptions); // ماژول نیتیو اکسپرس
    this.adapterHost.httpAdapter.setCookie(res, REFRESH_COOKIE, refreshToken, this.refreshCookieOptions);

    return { accessToken };
  }

  @Post('signout')
  @Throttle({ default: { ttl: 60_000, limit: 10 } })
  @HttpCode(200)
  async signout(@Cookies(REFRESH_COOKIE) token: string | undefined, @Req() req: Request, @Res({ passthrough: true }) res: unknown) {
    const token = req.cookies?.[REFRESH_COOKIE]; // ماژول نیتیو اکسپرس

    if (token) await this.authService.signout(token);

    const { maxAge: _maxAge, ...clearOptions } = this.refreshCookieOptions; // ماژول نیتیو اکسپرس

    res.clearCookie(REFRESH_COOKIE, clearOptions); // ماژول نیتیو اکسپرس
    this.adapterHost.httpAdapter.clearCookie(res, REFRESH_COOKIE, { path: '/auth' });

    return { message: 'logged out' };
  }

  @Post('forgot-password')
  @Throttle({ default: { ttl: 60_000, limit: 3 } })
  forgotPassword(@Body() forgotPasswordData: ForgotPasswordDto) {
    return this.authService.forgotPassword(forgotPasswordData.email);
  }

  @Post('reset-password')
  @Throttle({ default: { ttl: 60_000, limit: 3 } })
  resetPassword(@Body() resetPasswordData: ResetPasswordDto) {
    return this.authService.resetPassword(resetPasswordData.token, resetPasswordData.password);
  }
}
```

# service

```ts
import { BadRequestException, Injectable, UnauthorizedException } from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';
import * as bcrypt from 'bcrypt';
import { createHash, randomBytes, randomInt } from 'crypto';
import { InjectRepository } from '@nestjs/typeorm';
import { ConfigService } from '@nestjs/config';

@Injectable()
export class AuthService {
  constructor(
    @InjectRepository(PasswordReset) private readonly resetRepo: Repository<PasswordReset>,
    @InjectRepository(Otp) private readonly otpRepo: Repository<Otp>,
    private readonly sessionService: SessionService,
    private readonly usersService: UsersService,
    private readonly mailService: MailService,
    private readonly jwtService: JwtService,
    private readonly config: ConfigService
  ) {}

  private hashToken(token: string) {
    return createHash('sha256').update(token).digest('hex');
  }

  private getJwtToken({ id, email, role }: AuthenticatedUser) {
    if (!id || !role || !email) {
      throw new Error('getJwtToken: user object is missing required fields (id, email, role)');
    }

    return this.jwtService.sign({ sub: id, email: email, role: role });
  }

  async signup({ email, password }: SignUpDto) {
    if (await this.usersService.existsByEmail(email)) {
      throw new BadRequestException('email already exists');
    }

    const code: string = String(randomInt(1_000, 10_000));

    await this.otpRepo.upsert(
      {
        email: email,
        passwordHash: await bcrypt.hash(password, 10),
        codeHash: await bcrypt.hash(code, 10),
        expiresAt: new Date(Date.now() + Number(this.config.getOrThrow('OTP_TTL_MINUTES')) * 60 * 1000),
        attempts: 0,
      },
      ['email']
    );

    this.config.getOrThrow('NODE_ENV') === 'development'
      ? console.log(`--------${code},${email}--------`)
      : await this.mailService.sendOtp(email, code);

    return { message: 'verification code sent' };
  }

  async verifyEmail({ email, code }: VerifyEmailDto) {
    const otp = await this.otpRepo.findOneBy({ email });

    if (!otp || otp.expiresAt < new Date() || otp.attempts >= Number(this.config.getOrThrow('OTP_MAX_ATTEMPTS'))) {
      throw new BadRequestException('invalid or expired code');
    }

    if (!(await bcrypt.compare(code, otp.codeHash))) {
      await this.otpRepo.increment({ email }, 'attempts', 1);

      throw new BadRequestException('invalid or expired code');
    }

    const user = await this.usersService.createVerified(email, otp.passwordHash);
    await this.otpRepo.delete({ email });

    return {
      accessToken: this.getJwtToken(user),
      refreshToken: await this.sessionService.createToken(user.id),
    };
  }

  async signin({ email, password }: SignInDto) {
    const user = await this.usersService.findByEmailWithPassword(email);

    const dummyHash = '$2b$10$OT.JE3FQJtDAYAS0N/Qk5e818jhdU3swDc8cNVnfWP5yGH5rNSDam';
    const passwordMatches: boolean = await bcrypt.compare(password, user?.password ?? dummyHash);

    if (!user || !passwordMatches) {
      throw new UnauthorizedException('invalid credentials');
    }

    return {
      accessToken: this.getJwtToken(user),
      refreshToken: await this.sessionService.createToken(user.id),
    };
  }

  async refresh(refreshToken: string) {
    const session = await this.sessionService.getSessionByToken(refreshToken);

    const user = await this.usersService.findById(session.userId);

    if (!user) {
      throw new UnauthorizedException('invalid refresh token');
    }

    const revoked = await this.sessionService.revokeSessionById(session.id);
    if (revoked.affected !== 1) throw new UnauthorizedException('invalid refresh token');

    return {
      accessToken: this.getJwtToken(user),
      refreshToken: await this.sessionService.createToken(user.id, session.familyId),
    };
  }

  async signout(refreshToken: string) {
    await this.sessionService.revokeSessionByToken(refreshToken);

    return { message: 'signout successfully' };
  }

  async forgotPassword(email: string) {
    const message = { message: 'if this email is registered, a reset link has been sent' };

    if (!(await this.usersService.existsByEmail(email))) {
      return message;
    }

    const resetToken = randomBytes(32).toString('hex');

    await this.resetRepo.upsert(
      {
        email,
        tokenHash: this.hashToken(resetToken),
        expiresAt: new Date(Date.now() + Number(this.config.getOrThrow('PASSWORD_RESET_TTL_MINUTES')) * 60 * 1000),
      },
      ['email']
    );

    const link = `${this.config.getOrThrow('FRONTEND_URL')}${this.config.getOrThrow('PASSWORD_RESET_PATH')}?token=${resetToken}`;

    this.config.getOrThrow('NODE_ENV') === 'development'
      ? console.log(`--------Password reset link: ${link}--------`)
      : await this.mailService.sendPasswordReset(email, link);

    return message;
  }

  async resetPassword(resetToken: string, newPassword: string) {
    const passwordReset = await this.resetRepo.findOneBy({ tokenHash: this.hashToken(resetToken) });

    if (!passwordReset || passwordReset.expiresAt < new Date()) {
      throw new BadRequestException('invalid or expired link');
    }

    const user = await this.usersService.findByEmail(passwordReset.email);

    if (!user) {
      throw new BadRequestException('invalid or expired link');
    }

    await this.usersService.updateUserHashedPassword(user.id, await bcrypt.hash(newPassword, 10));

    await this.sessionService.revokeAllSessionsByUserId(user.id);

    await this.resetRepo.delete({ id: passwordReset.id });

    return { message: 'password updated' };
  }
}
```

# main.ts

```ts
import cookieParser from 'cookie-parser';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  app.use(cookieParser()); // لازمه

  await app.listen(Number(config.getOrThrow('PORT')));
}
await bootstrap();
```
