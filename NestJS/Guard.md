```ts
import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common';

@Injectable()
export class ApiKeyGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const request = context.switchToHttp().getRequest();

    const apiKey = request.headers['x-api-key'];

    return apiKey === 'my-secret-key';
  }
}

// استفاده

@Controller('users')
@UseGuards(ApiKeyGuard)
export class UsersController {
  @Get()
  getUsers() {
    return ['Ali', 'Reza', 'Amin'];
  }
}
```

---

```ts
import { SetMetadata } from '@nestjs/common';

export const VIP_KEY = 'isVip';
// ست کردن متا دیتا
export const VipOnly = () => {
  return SetMetadata(VIP_KEY, true);
};

// استفاده در گارد
import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common';
import { Reflector } from '@nestjs/core';
import { VIP_KEY } from './vip.decorator.js';

@Injectable()
export class VipGuard implements CanActivate {
  constructor(private readonly reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const isVip = this.reflector.get<boolean>(VIP_KEY, context.getHandler());

    if (!isVip) {
      return true;
    }

    const request = context.switchToHttp().getRequest();

    return request.user?.isVip === true;
  }
}

// استفاده در کنترلر

@Controller('products')
@UseGuards(VipGuard)
export class ProductsController {
  @Get()
  getProducts() {
    return 'All products';
  }

  @Get('vip')
  @VipOnly()
  getVipProducts() {
    return 'VIP products';
  }
}
```

---

```ts
import { SetMetadata } from '@nestjs/common';

export const ROLES_KEY = 'roles';

export enum Role {
  User = 'user',
  Admin = 'admin',
  Owner = 'owner',
}

export function Roles(...roles: Role[]) {
  // SetMetadata برای چسبوندن مقدار اضافی به یه متد یا کلاس استفاده میشه
  return SetMetadata(ROLES_KEY, roles); // ورودی اول میشه کلید و ورودی دوم میشه مقدار اون کلید
}

// استفاده در گارد

import { Reflector } from '@nestjs/core';
import { ROLES_KEY } from '../decorators/roles.decorator.js';

@Injectable()
// رو داشته باشیم CanActivate برای ساخت یه گارد باید حتما
export class RolesGuard implements CanActivate {
  // استفاده می‌شود Metadata است که برای خواندن NestJS یک سرویس Reflector
  constructor(private readonly reflector: Reflector) {}

  // ExecutionContext یک ابجکته که نست به اینترسپتور و گارد و بقیه بخش ها میده تا بفهمیم داریم چیو اجرا میکنیم
  canActivate(context: ExecutionContext): boolean {
    // context.getClass() | context.getHandler() | مثلا از چه کلاس یا متدی داریم اینو میگیریم

    // ما میتونیم روی کلاس روی متد متادیتا بزاریم
    // یعنی نزدیک ترین متادیتا رو برسون به ما یعنی اگه هم کلاس داشت هم متد به ما متد رو بده getAllAndOverride
    const roles = this.reflector.getAllAndOverride(ROLES_KEY, [context.getHandler(), context.getClass()]);
    // [context.getHandler(), context.getClass()] همون حرف بالا یعنی اول متد اگه نبود از کنترلر بگیر

    // مشخص را می‌خواند target معمولاً یک get
    // const *** = this.reflector.get('key', context.getHandler())

    if (!roles || roles.length === 0) return true;

    // این دقیقا میشه ریکوئست و بعدش میتونی بادی و هدر و پارام رو داشته باشی
    const request = context.switchToHttp().getRequest();

    if (!request.user) throw new UnauthorizedException();

    if (!roles.includes(request.user.role)) {
      throw new ForbiddenException('insufficient role');
    }

    return true;
  }
}
```
