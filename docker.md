## نصب Docker روی Ubuntu

مراحل رسمی نصب از مخزن Docker (به جای نسخه‌ی apt):

```bash
sudo apt update

sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
 "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
 noble stable" | \
 sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

> به جای `noble` کدنام نسخه اوبونتو خودت را بگذار - `lsb_release -cs`

بررسی اینکه درست اجرا می‌شود:

```bash
sudo systemctl status docker
```

```bash
sudo docker run hello-world
```

اجرای دستورات docker بدون sudo:

```bash
sudo usermod -aG docker $USER
```

بعد باید از حساب خارج و دوباره وارد بشی یا سیستم را ری‌استارت کنی.

## دستورات اصلی Container

```bash
docker ps          # کانتینرهای در حال اجرا
docker ps -a       # همه کانتینرها حتی متوقف شده‌ها
```

```bash
docker run nginx                     # اجرا - اگه ایمیج نباشه دانلودش میکنه
docker run -d nginx                  # detached - در بک‌گراند
docker run -p 8080:80 nginx          # مپ پورت - پورت 8080 سیستم به 80 کانتینر
docker run --name my-nginx nginx     # با اسم دلخواه
docker run -it ubuntu bash           # ورود به ترمینال کانتینر
docker run --rm nginx                # بعد از توقف کانتینر حذفش کن
```

```bash
docker stop my-nginx      # توقف
docker start my-nginx     # اجرای دوباره
docker restart my-nginx
docker rm my-nginx        # حذف کانتینر
docker rm -f my-nginx     # حذف اجباری حتی در حال اجرا
docker logs my-nginx      # دیدن لاگ‌ها
docker logs -f my-nginx   # دنبال کردن لاگ‌ها مثل tail -f
```

ورود به ترمینال کانتینر در حال اجرا:

```bash
docker exec -it my-nginx bash
```

## ایمیج‌ها

```bash
docker images                 # لیست ایمیج‌های موجود
docker pull postgres          # دانلود ایمیج
docker rmi nginx              # حذف ایمیج
```

ساخت ایمیج از Dockerfile:

```bash
docker build -t my-app .
docker build -t my-app:2.0 .  # با تگ نسخه
```

## اجرای دیتابیس‌ها

```bash
docker run -d --name postgres-db \
  -e POSTGRES_USER=admin \
  -e POSTGRES_PASSWORD=secret \
  -e POSTGRES_DB=mydb \
  -p 5432:5432 \
  postgres
```

```bash
docker run -d --name mongo-db \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=secret \
  -p 27017:27017 \
  mongo
```

```bash
docker run -d --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=secret \
  -p 3306:3306 \
  mysql
```

## Volume

برای اینکه با حذف کانتینر داده‌ها پاک نشوند:

```bash
docker volume create mydata

docker run -d --name postgres-db \
  -e POSTGRES_PASSWORD=secret \
  -v mydata:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres
```

```bash
docker volume ls
docker volume rm mydata
```

## Dockerfile

فایلی به اسم `Dockerfile` در ریشه پروژه که می‌گوید ایمیج چطور ساخته شود:

```dockerfile
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["node", "dist/main.js"]
```

```text
FROM        ایمیج پایه
WORKDIR     مسیر کاری داخل کانتینر
COPY        کپی فایل از سیستم به کانتینر
RUN         دستوری که موقع build اجرا میشه
EXPOSE      اعلام پورت
CMD         دستوری که موقع اجرای کانتینر اجرا میشه
```

نکته: اول `package*.json` کپی و نصب می‌شود تا با هر تغییر کد، `npm ci` دوباره اجرا نشود - از کش استفاده می‌شود.

## .dockerignore

مثل gitignore برای ایمیج - فایل‌های اضافه را از build حذف می‌کند:

```text
node_modules
dist
.git
.env
```

## Docker Compose

اجرای چند سرویس با هم - فایل `docker-compose.yml`:

```yaml
services:
  app:
    build: .
    ports:
      - '3000:3000'
    environment:
      - DATABASE_URL=postgresql://admin:secret@db:5432/mydb
    depends_on:
      - db

  db:
    image: postgres
    restart: always
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: mydb
    ports:
      - '5432:5432'
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

```bash
docker compose up          # اجرای همه سرویس‌ها
docker compose up -d       # در بک‌گراند
docker compose up --build  # با بیلد دوباره
docker compose down        # توقف و حذف همه
docker compose logs -f     # لاگ همه سرویس‌ها
docker compose ps
```

> داخل فایل compose بین سرویس‌ها به جای localhost از اسم سرویس استفاده می‌شود - مثل `db:5432`

## پاکسازی

```bash
docker system df              # میزان فضای اشغالی
docker system prune           # حذف کانتینرهای متوقف، شبکه‌های بلااستفاده
docker system prune -a        # همه چیز حتی ایمیج‌های بلااستفاده
```
