# Entity

```ts
import { Column, CreateDateColumn, Entity, Index, OneToMany, PrimaryGeneratedColumn, UpdateDateColumn } from 'typeorm';
import { Reservation } from '../../reservations/entities/reservation.entity.js';
import type { Relation } from 'typeorm';

// از اینام ها استفاده میکنیم چون که در زمان کامپایل هم وجود دارن
export enum Role {
  User = 'user',
  Admin = 'admin',
  Owner = 'owner',
}

@Entity('users')
@Index('only_one_owner', ['role'], { unique: true, where: `"role" = 'owner'` }) // فقط یک کاربر می‌تونه رول صاحب داشته باشه
export class User {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  // nullable: false پیشفرضه - یعنی نمیتونه خالی باشه
  @Column({ unique: true })
  email: string;

  @Column({ select: false })
  password: string;

  @Column({ type: 'enum', enum: Role })
  role: Role;

  // @CreateDateColumn({ name: 'created_at' })
  // createdAt: Date;
  // هم تاریخ هم زمان
  // @UpdateDateColumn({ name: 'updated_at' })
  // updatedAt: Date;

  @CreateDateColumn({ name: 'created_at', type: 'date' })
  createdAt: string;
  // فقط تاریخ
  @UpdateDateColumn({ name: 'updated_at', type: 'date' })
  updatedAt: string;

  // ستونی نمیسازه ولی برای گرفتن دیتا خوبه
  @OneToMany(() => Reservation, (reservation) => reservation.user)
  reservations: Relation<Reservation[]>;
}
```

# Module

```ts
@Module({
  imports: [TypeOrmModule.forFeature([User]), SessionModule],
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService],
})
export class UsersModule {}
```

# Controller

```ts
@ApiTags('Users')
@Controller('users')
@ApiBearerAuth()
// وقتی از یه گارد استفاده میکنیم یوزر رو میتونیم از طریق دکوراتور بگیریم
// rolesGuard بعد jwtGuard ترتیب گارد ها مهمه اول
@UseGuards(JwtGuard, RolesGuard)
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  @Roles(Role.Admin, Role.Owner)
  @ApiQuery({ name: 'email', required: false, example: 'user@example.com' })
  getUsers(@CurrentUser('role') role: Role, @Query('email', ParseEmailPipe) email?: string) {
    return this.usersService.getVisibleUsers(role, email);
  }

  @Post()
  @Roles(Role.Owner)
  addUser(@Body() userData: CreateUserDto) {
    return this.usersService.addUser(userData);
  }

  @Get('me')
  getMe(@CurrentUser('id') id: string) {
    return this.usersService.getById(id);
  }

  @Patch('me')
  updateMe(@CurrentUser('id') id: string, @Body() userData: UpdateUserDto) {
    return this.usersService.updateUserInformation(id, userData);
  }

  @Delete('me')
  deleteMe(@CurrentUser('id') id: string) {
    return this.usersService.removeUser(id);
  }

  @Get(':id')
  @Roles(Role.Admin, Role.Owner)
  getOne(@CurrentUser() actor: AuthenticatedUser, @Param('id', ParseUUIDPipe) id: string) {
    return this.usersService.getVisibleById(actor, id);
  }

  @Delete(':id')
  @Roles(Role.Owner)
  removeUser(@Param('id', ParseUUIDPipe) id: string) {
    return this.usersService.removeUser(id);
  }

  @Patch(':id/role')
  @Roles(Role.Owner)
  updateRole(@Param('id', ParseUUIDPipe) id: string, @Body() { role }: UpdateRoleDto) {
    return this.usersService.updateRole(id, role);
  }
}
```

# Service

```ts
import { ILike, In, Repository } from 'typeorm';
import * as bcrypt from 'bcrypt';
import { type AuthenticatedUser } from '../common/strategies/jwt.strategy.js';

@Injectable()
export class UsersService {
  constructor(
    @InjectRepository(User) private readonly userRepo: Repository<User>,
    private readonly sessionService: SessionService
  ) {}

  findById(id: string) {
    return this.userRepo.findOne({
      where: { id },
    });
  }

  findByIdWithPassword(id: string) {
    return this.userRepo.findOne({
      where: { id: id },
      select: { id: true, email: true, role: true, password: true },
    });
  }

  findByEmail(email: string) {
    return this.userRepo.findOneBy({ email });
  }

  findByEmailWithPassword(email: string) {
    return this.userRepo.findOne({
      where: { email: email },
      select: { id: true, email: true, role: true, password: true },
    });
  }

  existsByEmail(email: string) {
    return this.userRepo.existsBy({ email });
  }

  async getById(id: string) {
    const user = await this.findById(id);
    if (!user) throw new NotFoundException('user not found');
    return user;
  }

  async updateUserInformation(id: string, { email, password }: UpdateUserDto) {
    if (!email && !password) {
      throw new BadRequestException('No fields provided for update');
    }

    const user = await this.findByIdWithPassword(id);

    if (!user) {
      throw new NotFoundException('user not found');
    }

    if (email && email !== user.email) {
      if (await this.existsByEmail(email)) {
        throw new ConflictException('email already in use');
      }

      user.email = email;
    }

    if (password) {
      user.password = await bcrypt.hash(password, 10);
      await this.sessionService.revokeAllSessionsByUserId(id);
    }

    await this.userRepo.save(user);

    return { id: user.id, email: user.email };
  }

  updateUserHashedPassword(id: string, passwordHash: string) {
    return this.userRepo.update(id, { password: passwordHash });
  }

  createVerified(email: string, passwordHash: string) {
    const user = this.userRepo.create({
      email: email,
      password: passwordHash,
      role: Role.User,
    });

    return this.userRepo.save(user);
  }

  async addUser({ email, password }: CreateUserDto) {
    if (await this.existsByEmail(email)) {
      throw new ConflictException('Email already exists');
    }

    const user = this.userRepo.create({
      email: email,
      password: await bcrypt.hash(password, 10),
      role: Role.User,
    });

    const saved = await this.userRepo.save(user);

    return { id: saved.id, email: saved.email };
  }

  async getVisibleById(actor: AuthenticatedUser, id: string) {
    const target = await this.getById(id);

    if (actor.role === Role.Admin && target.role === Role.User) {
      return target;
    }

    if (actor.role === Role.Owner) {
      return target;
    }

    throw new NotFoundException('user not found');
  }

  async getVisibleUsers(role: Role, email?: string) {
    const visibleRoles: Role[] = role === Role.Owner ? [Role.Admin, Role.User] : [Role.User];

    return email
      ? this.userRepo.find({
          where: {
            email: ILike(`%${email}%`),
            role: In(visibleRoles),
          },
        })
      : this.userRepo.find({
          where: { role: In(visibleRoles) },
        });
  }

  async removeUser(id: string) {
    const target = await this.getById(id);

    if (target.role === Role.Owner) {
      throw new ForbiddenException('owner cannot be deleted');
    }

    await this.sessionService.revokeAllSessionsByUserId(id); // بهتره که اول سشن لغو شه بعدش کاربر حذف شه
    await this.userRepo.remove(target);

    return { message: 'user deleted' };
  }

  async updateRole(id: string, role: Role.Admin | Role.User) {
    const target = await this.getById(id);

    if (target.role === Role.Owner) {
      throw new ForbiddenException('owner role cannot be changed');
    }

    target.role = role;

    await this.userRepo.save(target);

    // همه نشست هارو باطل کن موقع تغییر رول
    await this.sessionService.revokeAllSessionsByUserId(id);

    return { id: target.id, role: target.role, email: target.email };
  }
}
```
