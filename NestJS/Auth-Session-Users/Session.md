# Entity

```ts
// این جدول فقط برای نگهداری توکن نیست در واقع سیستم مدیریت نشست‌ های کاربر محسوب می‌شود
// قابلیت هایی مثل خروج از یک دستگاه خروج از تمام دستگاه‌ ها مشاهده نشست‌ های فعال را پیاده‌سازی کرذ
@Entity('sessions')
export class Session {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  // هر کاربر ممکنه با لپتاپ یا موبایلش وارد شه
  // هر دستگاه یه رفرش توکن جداگونه داره پس ممکنه چند رکورد برای یه کاربر باشه
  // با این ستون میتونیم نشست های یه کاربر رو پیدا کنیم و باطل کنیم
  @Index()
  @Column({ name: 'user_id' })
  userId: string;

  // هش رفرش توکن اصلی
  @Column({ name: 'token_hash', unique: true })
  tokenHash: string;

  // می‌گوید این توکن مال کدام نشست است - روی کدام دستگاه و کدام ورود؟
  @Index()
  @Column({ name: 'family_id' })
  familyId: string;

  // زمان انقضای توکن - میتونیم بفهمیم رفرش توکن هنوز اعتبار داره یا نه
  // انقضای کوکی توی مرورگر به تنهایی کافی نیست سرور هم باید اعتبار توکن رو بررسی کنه
  @Column({ name: 'expires_at' })
  expiresAt: Date;

  // تاریحی که رفرش توکن باطل شده ممکنه باطل شده باشه ممکنه نشده باشه که اگه نباشه نال میزاریم
  @Column({ name: 'revoked_at', type: 'timestamp', nullable: true })
  revokedAt: Date | null; // اگه نال باشه یعنی هنوز باطل نشده - ولی اگه تاریح باشه تاریخ باطل شدنشو میگه
  // چرا بجای حذف مستقیم توکن این ستونو داریم؟
  // حذف رکورد باعث میشه سابقه توکن را از دست بدیم
  // ولی این باعث میشه بفهمیم توکن قبلا معتبر بوده؟ چه زمانی باطل شده؟ کسی خاسته از باطل شدش استفاده کنه یا نه؟

  // تاریخ ساخت توکن - این ستون خودش ایجاد میشه
  @CreateDateColumn({ name: 'created_at' })
  createdAt: Date;
}
```

# Service

```ts
import { createHash, randomBytes, randomUUID } from 'crypto';

@Injectable()
export class SessionService {
  constructor(
    @InjectRepository(Session) private readonly sessionRepo: Repository<Session>,
    private readonly config: ConfigService
  ) {}

  private getToken() {
    return randomBytes(32).toString('hex');
  }

  private hashToken(token: string): string {
    return createHash('sha256').update(token).digest('hex');
  }

  async getSessionByToken(refreshToken: string) {
    const session = await this.sessionRepo.findOneBy({ tokenHash: this.hashToken(refreshToken) });

    if (!session) {
      throw new UnauthorizedException('invalid refresh token');
    }

    // اگه این توکن قبلا باطل شده و کسی داره ازش استفاده می‌کنه یعنی احتمالا توکن دزدیده شده
    if (session.revokedAt) {
      // پس کل توکن های اون خانواده رو باطل کن و کاربر مجبور به لاگین مجدد بشه
      await this.revokeSessionByFamilyId(session.familyId);
      throw new UnauthorizedException('invalid refresh token');
    }

    if (session.expiresAt < new Date()) {
      throw new UnauthorizedException('refresh token expired');
    }

    return session;
  }

  revokeAllSessionsByUserId(userId: string) {
    return this.sessionRepo.update({ userId: userId, revokedAt: IsNull() }, { revokedAt: new Date() });
  }

  revokeSessionById(id: string) {
    return this.sessionRepo.update({ id: id, revokedAt: IsNull() }, { revokedAt: new Date() });
  }

  revokeSessionByFamilyId(familyId: string) {
    return this.sessionRepo.update({ familyId: familyId, revokedAt: IsNull() }, { revokedAt: new Date() });
  }

  revokeSessionByToken(token: string) {
    return this.sessionRepo.update({ tokenHash: this.hashToken(token), revokedAt: IsNull() }, { revokedAt: new Date() });
  }

  async createToken(userId: string, familyId?: string) {
    const TTL = Number(this.config.getOrThrow('REFRESH_TOKEN_TTL_DAYS')) * 24 * 60 * 60 * 1000;

    const refreshToken = this.getToken();

    await this.sessionRepo.save({
      userId: userId,
      familyId: familyId || randomUUID(),
      tokenHash: this.hashToken(refreshToken),
      expiresAt: new Date(Date.now() + TTL),
      revokedAt: null,
    });

    return refreshToken;
  }
}
```

# Module

```ts
@Module({
  imports: [TypeOrmModule.forFeature([Session])],
  providers: [SessionService],
  exports: [SessionService],
})
export class SessionModule {}
```
