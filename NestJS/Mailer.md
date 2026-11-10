# Mail Module

> module

```ts
import { Module } from '@nestjs/common';
import { MailerModule } from '@nestjs-modules/mailer';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { HandlebarsAdapter } from '@nestjs-modules/mailer/dist/adapters/handlebars.adapter.js';
import { join } from 'path';
import { MailService } from './mail.service.js';

@Module({
  imports: [
    MailerModule.forRootAsync({
      imports: [ConfigModule],

      inject: [ConfigService],

      useFactory: (config: ConfigService) => ({
        transport: {
          service: 'gmail', // به نست میگه از چه سرویسی استفاده کن

          // اطلاعات احراز هویت جیمیل
          auth: {
            user: config.getOrThrow<string>('SMTP_USER'),
            pass: config.getOrThrow<string>('SMTP_PASSWORD'),
          },
        },

        // مقدار پیش فرض برای فرستنده ایمیل - یعنی لازم نیست هی بنویسیم از طرف کی فرستاده میشه
        defaults: { from: config.getOrThrow<string>('SMTP_USER') },
      }),
    }),
  ],
  providers: [MailService],
  exports: [MailService],
})
export class MailModule {}
```

> service

```ts
import { Injectable } from '@nestjs/common';
import { MailerService } from '@nestjs-modules/mailer';
import { ConfigService } from '@nestjs/config';
import { passwordResetTemplate } from './templates/password-reset.template.js';
import { otpTemplate } from './templates/otp.template.js';

@Injectable()
export class MailService {
  constructor(
    private readonly mailer: MailerService,
    private readonly config: ConfigService
  ) {}

  async sendOtp(to: string, code: string): Promise<void> {
    const expiresInMinutes = Number(this.config.getOrThrow('OTP_TTL_MINUTES'));

    await this.mailer.sendMail({
      to,
      subject: 'Your verification code',
      text: `Your verification code is ${code}. It expires in ${expiresInMinutes} minutes.`,
      html: otpTemplate(code, expiresInMinutes),
    });
  }

  async sendPasswordReset(to: string, link: string): Promise<void> {
    const expiresInMinutes = Number(this.config.getOrThrow('PASSWORD_RESET_TTL_MINUTES'));

    await this.mailer.sendMail({
      to,
      subject: 'Reset your password',
      text: `Click the link to reset your password: ${link}\nThis link expires in ${expiresInMinutes} minutes.`,
      html: passwordResetTemplate(link, expiresInMinutes),
    });
  }
}
```

# Native

```ts
import { Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { createTransport, Transporter } from 'nodemailer';
import { dirname, join } from 'path';
import { fileURLToPath } from 'url';
import ejs from 'ejs';

const TEMPLATES_DIR = join(dirname(fileURLToPath(import.meta.url)), 'templates');

@Injectable()
export class MailService {
  private readonly transporter: Transporter;

  constructor(private readonly config: ConfigService) {
    this.transporter = createTransport(
      {
        service: 'gmail',
        auth: {
          user: config.getOrThrow<string>('SMTP_USER'),
          pass: config.getOrThrow<string>('SMTP_PASSWORD'),
        },
      },
      { from: config.getOrThrow<string>('SMTP_USER') }
    );
  }

  async sendOtp(to: string, code: string): Promise<void> {
    const expiresInMinutes = Number(this.config.getOrThrow('OTP_TTL_MINUTES'));

    await this.transporter.sendMail({
      to,
      subject: 'Your verification code',
      text: `Your verification code is ${code}. It expires in ${expiresInMinutes} minutes.`,
      html: await ejs.renderFile(join(TEMPLATES_DIR, 'otp.ejs'), {
        code,
        expiresInMinutes: expiresInMinutes,
      }),
    });
  }

  async sendPasswordReset(to: string, link: string): Promise<void> {
    const passwordResetTtlMinutes = Number(this.config.getOrThrow('PASSWORD_RESET_TTL_MINUTES'));

    await this.transporter.sendMail({
      to,
      subject: 'Reset your password',
      text: `Click the link to reset your password: ${link}\nThis link expires in ${passwordResetTtlMinutes} minutes. If you did not request this, ignore this email.`,
      html: await ejs.renderFile(join(TEMPLATES_DIR, 'password-reset.ejs'), {
        link,
        expiresInMinutes: passwordResetTtlMinutes,
      }),
    });
  }
}
```
