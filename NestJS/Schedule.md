توی اپ ماژول باید اول راه اندازی بشه

```ts
import { ScheduleModule } from '@nestjs/schedule';

@Module({
  imports: [ScheduleModule.forRoot()],
})
export class AppModule {}
```

توی ماژول مورد نظر

```ts
import { Cron, CronExpression } from '@nestjs/schedule';
import { LessThan, Repository } from 'typeorm';

@Injectable()
export class TasksService {
  private readonly logger = new Logger(TasksService.name);

  constructor(
    @InjectRepository(PasswordReset) private readonly resetRepo: Repository<PasswordReset>,
    @InjectRepository(Session) private readonly sessionRepo: Repository<Session>,
    @InjectRepository(Otp) private readonly otpRepo: Repository<Otp>,
    private schedulerRegistry: SchedulerRegistry
  ) {}

  // هر روز ساعت 2 بامداد
  @Cron('0 0 2 * * *')
  cleanupLogs() {}

  // هر 10 دقیقه
  @Cron(CronExpression.EVERY_10_MINUTES)
  syncData() {}

  // هر 30 ثانیه
  @Interval(30_000)
  checkServerHealth() {}

  // مثال
  @Timeout(5_000)
  sendStartupNotification() {}

  // مثلاً وقتی کاربر سابسکریپشن داره استپ می‌کنه
  pauseUserSync(userId: string) {
    const job = this.schedulerRegistry.getCronJob(`sync-${userId}`);
    job.stop();
  }

  // وقتی سابسکریپشن دوباره فعال شد
  resumeUserSync(userId: string) {
    const job = this.schedulerRegistry.getCronJob(`sync-${userId}`);
    job.start();
  }

  @Cron(CronExpression.EVERY_10_MINUTES)
  async cleanupExpiredAuthRecords() {
    const now = new Date();

    const [otpResult, resetResult, sessionResult] = await Promise.all([
      this.otpRepo.delete({ expiresAt: LessThan(now) }),
      this.resetRepo.delete({ expiresAt: LessThan(now) }),
      this.sessionRepo.delete({ expiresAt: LessThan(now) }),
    ]);

    if (otpResult.affected || resetResult.affected || sessionResult.affected) {
      this.logger.log(
        `Cleaned up ${otpResult.affected ?? 0} expired OTPs, ` +
          `${resetResult.affected ?? 0} expired password resets, and ` +
          `${sessionResult.affected ?? 0} expired sessions`
      );
    }
  }
}
```
