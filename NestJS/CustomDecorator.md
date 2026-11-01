```ts
import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export const ClientIp = createParamDecorator((data: unknown, context: ExecutionContext) => {
  const request = context.switchToHttp().getRequest();

  return request.ip;
});

//

@Get()
getSomething(@ClientIp() ip: string) {
  console.log(ip);

  return { ip };
}
```
