## بررسی نسخه Git

```bash
git --version
```

---

# تنظیمات اولیه

## تنظیم Username و Email

```bash
git config --global user.name "amin.sheikhy"
git config --global user.email "aminsheikhy59@gmail.com"
```

## مشاهده تنظیمات فعلی

```bash
git config --global user.name
git config --global user.email
```

---

# ایجاد Repository

## مقداردهی اولیه Git

```bash
git init
```

---

# وضعیت فایل‌ها

## مفهوم وضعیت‌ها

### U = Untracked

فایل جدیدی که Git هنوز آن را نمی‌شناسد.

### M = Modified

فایلی که قبلاً Commit شده و اکنون تغییر کرده است.

## مشاهده وضعیت پروژه

```bash
git status
```

نمایش:

- فایل‌های Untracked
- فایل‌های Modified
- فایل‌های Stage شده

---

# Stage کردن فایل‌ها

## اضافه کردن یک فایل

```bash
git add index.html
```

## اضافه کردن همه فایل‌ها

```bash
git add .
```

---

# Commit

## ثبت تغییرات

```bash
git commit -m "create basic structure of my project"
```

---

# مشاهده تاریخچه Commit ها

## نمایش کامل

```bash
git log
```

## نمایش خلاصه

```bash
git log --oneline
```

---

# جابجایی بین Commit ها

## رفتن به یک Commit

```bash
git checkout 928ed23
```

### روش پیشنهادی

```bash
git switch --detach 928ed23
```

## بازگشت به شاخه اصلی

```bash
git switch main
```

یا

```bash
git checkout main
```

یا

```bash
git checkout master
```

---

# Clone کردن Repository

## دانلود Repository

```bash
git clone https://github.com/amin-dev3232/test.git
```

## دانلود با نام دلخواه

```bash
git clone https://github.com/amin-dev3232/test.git my-project
```

---

# Remote Repository

## اتصال Repository به GitHub

```bash
git remote add origin https://github.com/amin-dev3232/test.git
```

## مشاهده Remote ها

```bash
git remote
```

## چک کردن ریموت فعلی

```bash
git remote -v
```

## عوض کردن ریموت

```bash
git remote set-url origin https://github.com/amin-sheikhy-dev/paradise.git
```

---

# Push

## اولین Push

```bash
git push -u origin main
```

یا

```bash
git push -u origin master
```

## Push های بعدی

```bash
git push
```

## Push به Branch جدید

```bash
git push --set-upstream origin res-branch
```

---

# Pull

## دریافت آخرین تغییرات

```bash
git pull origin main
```

یا

```bash
git pull origin master
```

---

# Branch

## مشاهده Branch ها

```bash
git branch
```

## ساخت Branch جدید

```bash
git branch amin-branch
```

## تغییر نام Branch

```bash
git branch -M ali-branch
```

### تغییر نام Branch اصلی به Main

```bash
git branch -M main
```

---

# جابجایی بین Branch ها

## رفتن به Branch دیگر

```bash
git switch amin-branch
```

```bash
git switch master
```

## ساخت Branch و رفتن روی آن

```bash
git switch -c amin-branch
```

---

# Merge

## ادغام Branch با Branch فعلی

ابتدا روی Branch مقصد برو:

```bash
git switch main
```

سپس:

```bash
git merge amin-branch
```

### Merge Conflict

اگر دو نفر یک بخش مشترک از کد را تغییر داده باشند، Git تعارض (Conflict) ایجاد می‌کند و از شما می‌خواهد مشخص کنید کدام تغییر حفظ شود.

---

# README

تقریباً هر Repository باید فایل زیر را داشته باشد:

```text
README.md
```

برای مشاهده بهتر در VS Code:

- روی فایل راست کلیک کن
- Open Preview را انتخاب کن

---

# .gitignore

برای نادیده گرفتن فایل‌ها و پوشه‌ها:

```text
.gitignore
```

## مثال‌ها

### نادیده گرفتن یک فایل

```text
style.css
```

### فقط فایل داخل روت پروژه

```text
/style.css
```

### نادیده گرفتن یک پوشه

```text
assets/
```

### نادیده گرفتن مسیر مشخص

```text
styles/fonts/
```

### نادیده گرفتن تمام فایل‌های CSS

```text
*.css
```

### مستثنی کردن یک فایل

```text
!main.css
```

### فقط فایل‌های CSS روت پروژه

```text
/*.css
```
