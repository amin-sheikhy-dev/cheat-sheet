ایجاد User

```ts
const user = this.userRepo.create({ name: 'Amin', email: 'amin@example.com' });

await userRepository.save(user);
```

گرفتن همه Userها

```ts
const users = await userRepository.find();
```

گرفتن یک User

```ts
const user = await userRepository.findOne({
  where: { id: 1 },
});
```

چند شرط

```ts
const user = await userRepository.findOne({
  where: {
    name: 'Amin',
    email: 'amin@example.com',
  },
});
```

---

### Update

```ts
await userRepository.update(1, {
  name: 'Amin Sheikhy',
});
```

---

### Delete & remove()

```ts
await userRepository.delete(1);
```

```ts
const user = await userRepository.findOne({
  where: { id: 1 },
});

if (user) {
  await userRepository.remove(user);
}
```

---

### جداول و تنظیم حرفه‌ای Entity

### Relation

```ts
@Entity('users') // اسم جدول
export class User {
  @PrimaryGeneratedColumn('uuid') // یعنی از یو یو آیدی استفاده کن
  id: number;

  @Column({ type: 'varchar', length: 100 })
  name: string;

  @Column({ type: 'varchar', length: 255, unique: true }) // email باید منحصر‌به‌فرد باشد
  email: string;

  @Column({ type: 'int', nullable: true }) // nullable اگر بخواهیم اختیاری باشد
  age: number | null;

  @Column({ type: 'text' })
  bio: string;

  @Column({ type: 'boolean', default: true })
  isActive: boolean;

  // ستونی نمیسازه ولی برای گرفتن دیتا خوبه
  @OneToMany(() => Reservation, (reservation) => reservation.user)
  reservations: Relation<Reservation[]>;

  @CreateDateColumn() // موقع ایجاد یوزر همشون زمان ایجاد رو نشون میده - کلا ثابته و اپدیت نمیشه
  createdAt: Date;

  @UpdateDateColumn() // موقع ایجاد یوزر همشون زمان ایجاد رو نشون میده - موقع اپدیت تغییر میکنه
  updatedAt: Date;
}
```

```ts
@Entity('rooms')
export class Room {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column('numeric', {
    name: 'price_per_night',
    precision: 10,
    scale: 2,
    transformer: {
      to: (value: number) => value, // موقع درج کردن داخل دیتابیس
      from: (value: string) => Number(value), // موقع خوندن از دیتابیس
    },
  })
  pricePerNight: number;

  @Column('text')
  description: string;

  @Column('text', { array: true })
  amenities: string[];

  // ستونی نمیسازه ولی برای گرفتن دیتا خوبه
  @OneToMany(() => Reservation, (reservation) => reservation.room)
  reservations: Relation<Reservation[]>;
}
```

```ts
@Entity('reservations')
export class Reservation {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ name: 'check_in', type: 'date' })
  checkIn: string;

  @Column({ name: 'check_out', type: 'date' })
  checkOut: string;

  @Column()
  guests: number;

  // معنی نداره و تنها نمیشه نوشتش @ManyToOne بدون @OneToMany
  @ManyToOne(() => User, (user) => user.reservations, { nullable: false, onDelete: 'CASCADE' }) // اگه یوزر حذف شد رزور هاشم حذف شه
  @JoinColumn({ name: 'user_id' }) // اگه این رو ندیم خودش اسم رو میسازه
  user: Relation<User>;
  // این دوتا واقعا ستون میسازن
  @ManyToOne(() => Room, (room) => room.reservations, { nullable: false, onDelete: 'SET NULL' }) // اگه اتاق حذف شد رزور هاش نال بشه
  @JoinColumn({ name: 'room_id' }) // اگه این رو ندیم خودش اسم رو میسازه
  room: Relation<Room>;
}
```

گرفتن یوزر با رزور هاش

```ts
const user = await this.userRepository.findOne({
  where: {
    email: 'ali@gmail.com',
  },
  relations: {
    reservations: true,
  },
});
```

```json
{
  "id": "...",
  "name": "Ali",
  "email": "ali@gmail.com",
  "age": 27,
  "bio": "...",
  "isActive": true,
  "reservations": [
    {
      "id": "...",
      "checkIn": "2026-10-10",
      "checkOut": "2026-10-15",
      "guests": 2
    },
    {
      "id": "...",
      "checkIn": "2026-11-01",
      "checkOut": "2026-11-05",
      "guests": 3
    }
  ]
}
```

گرفتن یوزر با اتاق هایی که رزور کرده البته به واسطه رزور ها اتاق رو نشون میده

```ts
const user = await this.userRepository.findOne({
  where: {
    email: 'ali@gmail.com',
  },
  relations: {
    reservations: {
      room: true,
    },
  },
});
```

```json
{
  "id": "...",
  "name": "Ali",
  "email": "ali@gmail.com",

  "reservations": [
    {
      "id": "...",
      "checkIn": "2026-10-10",
      "checkOut": "2026-10-15",
      "guests": 2,

      "room": {
        "id": "...",
        "pricePerNight": 150,
        "description": "Luxury room",
        "amenities": ["WiFi", "TV"]
      }
    }
  ]
}
```

گرفتن اتاق ها و روز های رزور شده

```ts
const room = await this.roomRepository.findOne({
  where: {
    id: roomId,
  },
  relations: {
    reservations: true,
  },
});
```

```json
{
  "id": "...",
  "pricePerNight": 150,
  "description": "Luxury room",

  "reservations": [
    {
      "id": "...",
      "checkIn": "2026-10-10",
      "checkOut": "2026-10-15",
      "guests": 2
    }
  ]
}
```

گرفتن رزور ها با یوزر ها و اتاق ها

```ts
const reservation = await this.reservationRepository.findOne({
  where: {
    id: reservationId,
  },
  relations: {
    user: true,
    room: true,
  },
});
```

```json
{
  "id": "...",

  "checkIn": "2026-10-10",
  "checkOut": "2026-10-15",
  "guests": 2,

  "user": {
    "id": "...",
    "name": "Ali",
    "email": "ali@gmail.com"
  },

  "room": {
    "id": "...",
    "pricePerNight": 150,
    "description": "Luxury room"
  }
}
```

---

```ts
@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  name: string;

  @OneToOne(() => Profile, (profile) => profile.user)
  profile: Relation<Profile>;
}
```

```ts
@Entity()
export class Profile {
  @PrimaryGeneratedColumn()
  id: number;

  @Column({ nullable: true })
  bio: string | null;

  @OneToOne(() => User, (user) => user.profile)
  @JoinColumn({ name: 'user_id' })
  user: Relation<User>;
}
```

---

### where-Like-In-Between

```ts
const users = await userRepository.find({
  where: {
    isActive: true,
    age: 27,
  },
});

// برای جستجوی متنی
const users = await userRepository.find({
  where: {
    name: Like('%Amin%'),
  },
});

// مشخص را بگیریم ID اگر بخواهیم چند
const users = await userRepository.find({
  where: {
    id: In([1, 3, 7]),
  },
});

// هایی که سنشان بین 20 و 30 است User مثلاً
const users = await userRepository.find({
  where: {
    age: Between(20, 30),
  },
});

// بیشتر
const users = await userRepository.find({
  where: {
    age: MoreThan(25),
  },
});

// کمتر
const users = await userRepository.find({
  where: {
    age: LessThan(30),
  },
});

// جدید ترین یوزر ها
const users = await userRepository.find({
  order: {
    createdAt: 'DESC',
  },
});

const users = await userRepository.find({
  skip: 10, // اون 10 تای اول رو رد کن
  take: 10, // فقط 10 تا بده
});

const page = 2;
const limit = 10;

const users = await userRepository.find({
  skip: (page - 1) * limit,
  take: limit,
});

// اگر هم داده‌ها را بخواهیم و هم تعداد کل رکوردها
const [users, total] = await userRepository.findAndCount({
  skip: 0,
  take: 10,
});

users = [
  // 10 users
];
total = 57;

// یه کوئری واقعی
const [users, total] = await userRepository.findAndCount({
  where: {
    age: Between(20, 30),
    isActive: true,
  },

  order: {
    createdAt: 'DESC',
  },

  skip: 0,
  take: 10,
});
```

---

### QueryBuilder

بهت اجازه می‌دهد Query را مرحله‌به‌مرحله بسازی.

```ts
const users = await userRepository
  .createQueryBuilder('user') //
  .where('user.age > :age', { age: 20 })
  .orderBy('user.createdAt', 'DESC')
  .getMany(); // بیشتر از یک رکورد میده
```

```ts
const user = await userRepository
  .createQueryBuilder('user')
  .where('user.email = :email', {
    email: 'amin@example.com',
  })
  .getOne(); // اگر فقط یک رکورد می‌خواهیم
```

AND

```ts
const users = await userRepository
  .createQueryBuilder('user')
  .where('user.age > :age', {
    age: 20,
  })
  .andWhere('user.isActive = :active', {
    active: true,
  })
  .getMany();
```

OR

```ts
const users = await userRepository
  .createQueryBuilder('user')
  .where('user.name = :name1', {
    name1: 'Amin',
  })
  .orWhere('user.name = :name2', {
    name2: 'Ali',
  })
  .getMany();
```

orderBy

```ts
const users = await userRepository
  .createQueryBuilder('user') //
  .orderBy('user.createdAt', 'DESC')
  .getMany();
```

limit و offset

```ts
const users = await userRepository
  .createQueryBuilder('user') //
  .orderBy('user.createdAt', 'DESC')
  .skip(10)
  .take(10)
  .getMany();
```

```ts
const users = await userRepository
  .createQueryBuilder('user') //
  .leftJoinAndSelect('user.posts', 'post')
  .getMany();

  // قرار نمی‌گیرند Entity ها لزوماً در خروجی Post انجام می‌شود ولی Join
  .leftJoin("user.posts", "post")

  // می‌کند Select را Relation می‌کند و هم داده‌ی Join هم
  .leftJoinAndSelect()
```

```ts
const query = userRepository.createQueryBuilder('user');

if (minAge) {
  query.andWhere('user.age >= :minAge', {
    minAge,
  });
}

if (isActive !== undefined) {
  query.andWhere('user.isActive = :isActive', {
    isActive,
  });
}

if (search) {
  query.andWhere('user.name ILIKE :search', {
    search: `%${search}%`,
  });
}

const users = await query.orderBy('user.createdAt', 'DESC').getMany();
```
