# Nest Exceptions & Status Codes

✅ 2xx — موفق
───────────────

- `200 OK` ➡️ درخواست موفق
- `201 Created` ➡️ ایجاد شد
- `204 No Content` ➡️ موفق ولی چیزی برنمیگرده

🔀 3xx — ریدایرکت
───────────────────

- `301 Moved Permanently` ➡️ منتقل شده دائمی
- `302 Found` ➡️ منتقل شده موقت
- `304 Not Modified` ➡️ کش معتبره

⚠️ 4xx — خطای کلاینت
──────────────────────────────────────────────────────────────────

- `400 Bad Request` ➡️ ورودی اشتباه ولیدیشن خطا

  > BadRequestException()

- `401 Unauthorized` ➡️ توکن نداره یا نامعتبره

  > UnauthorizedException()

- `403 Forbidden` ➡️ توکن داره ولی دسترسی نداره

  > ForbiddenException()

- `404 Not Found` ➡️ رکورد یا روت پیدا نشد

  > NotFoundException()

- `405 Method Not Allowed` ➡️ متد HTTP اشتباه

  > MethodNotAllowedException()

- `408 Request Timeout` ➡️ timeout شد

  > RequestTimeoutException()

- `409 Conflict` ➡️ تکراری مثلاً ایمیل

  > ConflictException()

- `413 Payload Too Large` ➡️ سایز ریکوئست زیاده

  > PayloadTooLargeException()

- `415 Unsupported Media` ➡️ Content-Type اشتباه

  > UnsupportedMediaTypeException()

- `422 Unprocessable` ➡️ ساختار درسته ولی داده نامعتبره

  > UnprocessableEntityException()

- `429 Too Many Requests` ➡️ throttle زیاد ریکوئست زده
  > TooManyRequestsException()

🔥 5xx — خطای سرور
──────────────────────────────────────────────────────────────────

- `500 Internal Server Error` ➡️ خطای سرور

  > InternalServerErrorException()

- `501 Not Implemented` ➡️ هنوز پیاده‌سازی نشده

  > NotImplementedException()

- `502 Bad Gateway` ➡️ سرویس upstream خطا داد

  > BadGatewayException()

- `503 Service Unavailable` ➡️ سرور موقتاً در دسترس نیست

  > ServiceUnavailableException()

- `504 Gateway Timeout` ➡️ upstream timeout شد

  > GatewayTimeoutException()

---

کاستوم

```ts
throw new BadRequestException({
  message: 'ورودی نامعتبر',
  error: 'INVALID_INPUT',
  statusCode: 400,
});
```
