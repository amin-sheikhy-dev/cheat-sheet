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

//

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

export const VipOnly = () => {
  return SetMetadata(VIP_KEY, true);
};

//

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

//

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
