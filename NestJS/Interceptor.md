in car app

```ts
import { Exclude } from "class-transformer";

export class User {
  id: number;

  email: string;

  @Exclude()
  password: string;

  // important
  constructor(partial: Partial<User>) {
    Object.assign(this, partial);
  }
}

//

import { ClassSerializerInterceptor, UseInterceptors } from "@nestjs/common";

@Controller("users")
@UseInterceptors(ClassSerializerInterceptor)
export class UsersController {
  @Get(":id")
  findOne() {
    return new User({
      id: "...",
      email: "user@example.com",
      password: "12345678",
    });
  }

  @Get()
  otherMethods() {}
}
```

```json
{
  "id": 12,
  "email": "user@example.com"
}
```

---

```ts
import { Expose } from "class-transformer";

export class UserResponseDto {
  @Expose()
  id: number;

  @Expose()
  email: string;
}
```

```ts
import {
  ClassSerializerInterceptor,
  SerializeOptions,
  UseInterceptors,
} from '@nestjs/common';

@UseInterceptors(ClassSerializerInterceptor)
@SerializeOptions({ excludeExtraneousValues: true })
@Get(':id')
findOne() {
  return new UserResponseDto({
    id: 12,
    email: 'user@example.com',
    password: '12345678',
  });
}
```

```ts
import { ClassSerializerInterceptor } from "@nestjs/common";
import { Reflector } from "@nestjs/core";

const app = await NestFactory.create(AppModule);

app.useGlobalInterceptors(new ClassSerializerInterceptor(app.get(Reflector)));

await app.listen(3000);
```
