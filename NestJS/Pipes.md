custom pipes

nest pipes

```ts
import { ApiProperty } from '@nestjs/swagger';
import { Transform } from 'class-transformer';
import { IsEmail, IsNotEmpty, IsString, MaxLength, MinLength } from 'class-validator';

export class CreateUserDto {
  @ApiProperty({ description: 'User email address', example: 'user@example.com' })
  @Transform(({ value }) => (typeof value === 'string' ? value.trim().toLowerCase() : value))
  @IsEmail()
  email: string;

  @ApiProperty({ description: 'User password', example: '12345678', minLength: 8, maxLength: 72 })
  @IsString()
  @MinLength(8)
  @MaxLength(72)
  @IsNotEmpty()
  password: string;
}

//

import { BadRequestException, Injectable, PipeTransform } from '@nestjs/common';
import { PickType } from '@nestjs/swagger';
import { plainToInstance } from 'class-transformer';
import { validate } from 'class-validator';
import { CreateUserDto } from '../../users/dto/create-user.dto.js';

class EmailDto extends PickType(CreateUserDto, ['email']) {}

@Injectable()
export class ParseEmailPipe implements PipeTransform {
  async transform(value: unknown) {
    const obj = plainToInstance(EmailDto, { email: value });
    const errors = await validate(obj);

    if (errors.length > 0) {
      throw new BadRequestException(errors);
    }

    return obj.email;
  }
}
```

```ts
import { BadRequestException, Injectable, PipeTransform } from '@nestjs/common';
import { Role } from '../../users/entities/user.entity.js';

@Injectable()
export class ParseRolePipe implements PipeTransform {
  private readonly allowedRoles = [Role.Admin, Role.User];

  transform(value: unknown): Role.Admin | Role.User {
    if (typeof value !== 'string') {
      throw new BadRequestException('role must be a string');
    }

    if (!this.allowedRoles.includes(value as Role)) {
      throw new BadRequestException(`role must be one of: ${this.allowedRoles.join(', ')}`);
    }

    return value as Role.Admin | Role.User;
  }
}
```
