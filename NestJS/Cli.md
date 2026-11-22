# Nest CLI & Scripts

## نصب و ساخت پروژه

```bash
npm i -g @nestjs/cli        # CLI نصب
nest --version              # نمایش ورژن نست - یا مطمئن شدن از نصب
nest new <name>             # پروژه جدید
nest info                   # اطلاعات پروژه و نسخه‌ها
nest upgrade                # آپگرید به آخرین نسخه نست
nest add <library>          # اضافه کردن کتابخونه خارجی
nest deploy                 # cloud به deploy
```

## Generate — `nest g <schematic> <name>`

```bash
# ─── basic ───────────────────────────────
nest g module        mo   # ماژول
nest g controller    co   # کنترلر
nest g service       s    # سرویس
nest g resource      res  # full CRUD — module + controller + service + dto

# ─── auth & logic ────────────────────────
nest g guard         gu   # گارد
nest g interceptor   itc  # اینترسپتور
nest g pipe          pi   # پایپ
nest g filter        f    # فیلتر
nest g middleware     mi   # میدلور
nest g decorator     d    # دکوراتور کاستوم

# ─── others ──────────────────────────────
nest g class         cl   # کلاس ساده
nest g interface     itf  # اینترفیس
nest g provider      pr   # پروایدر
nest g resolver      r    # GraphQL resolver
nest g gateway       ga   # WebSocket gateway
nest g library       lib  # monorepo کتابخونه داخل
nest g sub-app       app  # monorepo اپ جدید داخل
nest g configuration      # CLI فایل کانفیگ
```

### آپشن‌های مهم

```bash
--flat        # بدون پوشه جداگانه
--no-spec     # بدون فایل تست
--dry-run     # فقط نشون بده چی میسازه نسازه

# مثال
nest g service users                    # src/users/users.service.ts
nest g service users/profile            # src/users/profile/profile.service.ts
nest g service common/services/logger   # src/common/services/logger.service.ts

# با آپشن
nest g service users --no-spec --flat   # src/users/users.service.ts بدون پوشه جداگانه
```

---

## NPM Scripts

```bash
# ─── run ────────────────────────────────
npm run start           # اجرای معمولی
npm run start:dev       # watch mode — ری‌استارت خودکار
npm run start:debug     # debug mode
npm run start:prod      # اجرای build شده

# ─── Build ───────────────────────────────
npm run build           # کامپایل TypeScript به dist/

# ─── Test ────────────────────────────────
npm run test            # unit tests
npm run test:watch      # watch mode
npm run test:cov        # coverage
npm run test:e2e        # end-to-end tests
```
