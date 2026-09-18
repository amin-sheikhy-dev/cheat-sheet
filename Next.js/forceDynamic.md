این صفحه را dynamic در نظر بگیر و هنگام request دوباره اجرا کن بنابراین اگر صفحه را refresh کنی زمان جدیدی می‌بینی

اینجا اطلاعات profile می‌تواند در طول زمان تغییر کند بنابراین نمی‌خواهیم صفحه به شکل static از قبل ساخته شود.

```tsx
export const dynamic = 'force-dynamic';

export default async function ProfilePage() {
  const response = await fetch('https://example.com/api/profile');

  const user = await response.json();

  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}
```

اگر صفحه static باشد ممکن است محتوای تولیدشده قبلی دوباره استفاده شود و برای چیزی مثل آخرین اخبار مناسب نباشد

```tsx
export const dynamic = 'force-dynamic';

export default async function NewsPage() {
  const news = await getLatestNews();

  return (
    <div>
      {news.map((item) => (
        <p key={item.id}>{item.title}</p>
      ))}
    </div>
  );
}
```
