# 📚 مجموعه یادداشت‌ های فنی مهندسی نرم افزار

> این فایل‌ها در اصل برای **مرور و یادآوری شخصی** من نوشته شده‌اند، اما از آنجا که ممکن است برای دیگران هم مفید باشند، این مخزن را عمومی کرده‌ام.  
> لطفاً در نظر داشته باشید که برخی یادداشت‌ها صرفاً خلاصه‌ی کدها و مفاهیم هستند و توضیحات گسترده ندارند.

---

issues در zod

session storage - local storage

gregorianToJalali - jalaliToGregorianw

update calender

```tsx
export type DateRange = {
  checkIn: string | null;
  checkOut: string | null;
};

type CalendarProps = {
  dateRange: DateRange;
  setDateRange: Dispatch<SetStateAction<DateRange>>;
};
```
