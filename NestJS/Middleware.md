```ts
@Module({
  imports: [AuthModule, UsersModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer.apply(LoggerMiddleware).forRoutes('*'); // * یعنی تمام روت ها

    consumer.apply(LoggerMiddleware).forRoutes('users'); // فقط روت یوزرز

    consumer.apply(LoggerMiddleware).exclude('product').forRoutes('*'); // همه روت ها بجز پروداکت
  }
}
```

```ts
import { Injectable, Logger, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

// اگه بخای از بلاک ایپی استفاده کنی یا بگی یه یوزر بیشتر از 100 درخواست نتونه بزنه با اینه

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  private logger = new Logger('HTTP');

  use(req: Request, res: Response, next: NextFunction) {
    const { ip, method, baseUrl } = req;

    const userAgent = req.get('user-agent') || '';

    const startAt = process.hrtime();

    res.on('finish', () => {
      const { statusCode } = res;

      const contentLength = res.get('content-length');

      const dif = process.hrtime(startAt);

      const responseTime = dif[0] * 1e3 + dif[1] * 1e-6;

      this.logger.log(`${method} ${baseUrl} ${statusCode} ${contentLength} - ${responseTime.toFixed(2)}ms ${userAgent} ${ip}`);
    });

    next();
  }
}
```
