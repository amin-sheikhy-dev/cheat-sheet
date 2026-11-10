```ts
export class UserResponseDto {
  @Expose()
  id: string;

  @Expose() //  @Exclude() برعکس همدیگه ان
  email: string;
}
```

`next.handle()` برمی‌گردونه رو به من بده Controller رو اجرا کن و نتیجه‌ای که Controller برو

`.pipe()` یک کاری انجام بدیم Controller حالا می‌خوایم روی نتیجه‌ی

```json
data =
{
  "id": "...",
  "email": "amin@gmail.com",
  "password": "1234"
}
```

`plainToInstance` UserResponseDto رو بگیر و تبدیلش کن به data یعنی

`excludeExtraneousValues: true` دارن Expose() هایی رو نگه دار که property فقط

```ts
import { CallHandler, ExecutionContext, NestInterceptor } from '@nestjs/common';
import { plainToInstance } from 'class-transformer';
import { map } from 'rxjs/operators';
import { Observable } from 'rxjs';

export class UserResponseInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(
      map((data) => {
        return plainToInstance(UserResponseDto, data, { excludeExtraneousValues: true });
      })
    );
  }
}
```

مثال

```ts
import { CallHandler, ExecutionContext, NestInterceptor } from '@nestjs/common';
import { map } from 'rxjs/operators';
import { Observable } from 'rxjs';

export class UserSerializerInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(
      map((data) => {
        const { password, ...user } = data.user;

        return { ...data, user };
      })
    );
  }
}
```

`context.switchToHttp()` را به من بده HTTP است اطلاعات HTTP این درخواست

`getRequest()` را بده request خود

```ts
import { Observable } from 'rxjs';
import { UsersService } from '../users/users.service.js';
import { map } from 'rxjs/operators';

@Injectable()
export class CurrentUserInterceptor implements NestInterceptor {
  constructor(private usersService: UsersService) {}

  async intercept(context: ExecutionContext, next: CallHandler): Promise<Observable<any>> {
    const request = context.switchToHttp().getRequest();

    const { userId } = request.session;

    if (userId) {
      const user = await this.usersService.findById(userId);
      request.currentUser = user;
    }

    // اگه سشن وجود نداشت که خب نتیجه خود کنترلر رو ریترن کن رو ریترن کن
    return next.handle();
  }
}

export class SetSessionInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const request = context.switchToHttp().getRequest();

    return next.handle().pipe(
      // چیزی که مپ ریترن میکنه چیزی هست که کنترلر برای ما ریترن میکنه
      map((user) => {
        request.session.userId = user.id;

        return user;
      })
    );
  }
}
```

---

```ts
export class UserEntity {
  id: number;
  email: string;
  firstName: string;
  lastName: string;

  @Exclude()
  password: string;

  constructor(partial: Partial<UserEntity>) {
    Object.assign(this, partial);
  }
}

//

@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @UseInterceptors(ClassSerializerInterceptor)
  @Get(':id')
  async findOne(@Param('id') id: string): Promise<UserEntity> {
    const user = await this.usersService.findOne(id);
    return new UserEntity(user);
  }
}
```

---

به جای تکرار @UseInterceptors روی هر controller می‌توانید آن را سراسری کنید

`main.ts`

```ts
import { ClassSerializerInterceptor } from '@nestjs/common';
import { NestFactory, Reflector } from '@nestjs/core';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalInterceptors(new ClassSerializerInterceptor(app.get(Reflector)));
  await app.listen(3000);
}
bootstrap();
```

یا روش دوم: در ماژول با تزریق وابستگی

`app.module.ts`

```ts
import { ClassSerializerInterceptor, Module } from '@nestjs/common';
import { APP_INTERCEPTOR } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_INTERCEPTOR,
      useClass: ClassSerializerInterceptor,
    },
  ],
})
export class AppModule {}
```

---

```ts
import { Exclude, Expose } from 'class-transformer';

export class UserResponseDto {
  id: number;
  email: string;
  firstName: string;
  lastName: string;

  @Exclude()
  password: string;

  @Expose()
  get fullName(): string {
    return `${this.firstName} ${this.lastName}`;
  }

  constructor(partial: Partial<UserResponseDto>) {
    Object.assign(this, partial);
  }
}
```

خروجی ORM های مختلف شکل متفاوتی دارد اما همگی را می‌توان با plainToInstance به DTO تبدیل کرد

```ts
import { plainToInstance } from 'class-transformer';

// TypeORM
plainToInstance(UserResponseDto, user);

// Prisma
plainToInstance(UserResponseDto, user);

// Mongoose
plainToInstance(UserResponseDto, user.toJSON());
```

تبدیل در لایه Service یا Controller

```ts
@Controller('users')
@UseInterceptors(ClassSerializerInterceptor)
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get(':id')
  async findOne(@Param('id') id: number): Promise<UserResponseDto> {
    const user = await this.usersService.findOne(id);

    return plainToInstance(UserResponseDto, user);
  }

  @Get()
  async findAll(): Promise<UserResponseDto[]> {
    const users = await this.usersService.findAll();

    return plainToInstance(UserResponseDto, users); // آرایه هم پشتیبانی می‌شود
  }
}
```

---

تکرار plainToInstance توی همه جا سخت میشه

```ts
// decorators/serialize.decorator.ts
import { SetMetadata } from '@nestjs/common';
import { ClassConstructor } from 'class-transformer';

export const SERIALIZE_DTO = 'serialize_dto';
export const Serialize = (dto: ClassConstructor<unknown>) => SetMetadata(SERIALIZE_DTO, dto);
```

```ts
// interceptors/serialize.interceptor.ts
import { CallHandler, ExecutionContext, Injectable, NestInterceptor } from '@nestjs/common';
import { Reflector } from '@nestjs/core';
import { plainToInstance } from 'class-transformer';
import { map, Observable } from 'rxjs';
import { SERIALIZE_DTO } from '../decorators/serialize.decorator';

@Injectable()
export class SerializeInterceptor implements NestInterceptor {
  constructor(private readonly reflector: Reflector) {}

  intercept(context: ExecutionContext, next: CallHandler): Observable<unknown> {
    const dto = this.reflector.getAllAndOverride(SERIALIZE_DTO, [context.getHandler(), context.getClass()]);

    return next.handle().pipe(
      map((data) => {
        if (!dto) return data;

        return plainToInstance(dto, data, { excludeExtraneousValues: true });
      })
    );
  }
}
```

```ts
// app.module.ts
@Module({
  providers: [{ provide: APP_INTERCEPTOR, useClass: SerializeInterceptor }],
})
export class AppModule {}
```

```ts
// dto/user-response.dto.ts
import { Expose } from 'class-transformer';

export class UserResponseDto {
  @Expose()
  id: number;

  @Expose()
  email: string;

  @Expose()
  firstName: string;

  @Expose()
  lastName: string;
}
```

```ts
@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get(':id')
  @Serialize(UserResponseDto)
  findOne(@Param('id') id: string) {
    return this.usersService.findOne(id); // مستقیم خروجی ORM
  }

  @Get()
  @Serialize(UserResponseDto)
  findAll() {
    return this.usersService.findAll();
  }
}
```

```ts
import { Expose, Type } from 'class-transformer';

export class RoleResponseDto {
  @Expose()
  id: number;

  @Expose()
  name: string;
}

export class UserResponseDto {
  @Expose()
  id: number;

  @Expose()
  email: string;

  @Expose()
  @Type(() => RoleResponseDto)
  role: RoleResponseDto;
}
```
