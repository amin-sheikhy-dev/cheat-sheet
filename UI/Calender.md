# پیش نیاز ها: Tailwind-css & Next.js or React & date-fns-jalali

```bash
npm i date-fns-jalali
```

#### مهم ترین استیت کامپوننت - استیتی که تاریخ های انتخاب شده توسط کاربر در آن ذخیره میشود

روز های انتخاب شده داخل این استیت ذخیره میشوند - در نهایت هرکاری که با روز های انتخاب شده میخواهید انجام دهید باید اضافه کنید

`const [selectedDates, setSelectedDates] = useState<string[]>([])`

و همچنین دقت شود این کامپوننت دیزاین جزئی دارد - خودتان باید با توجه به نیازتان به ان استایل بدهید

---

```tsx
'use client'; // در صورت استفاده از نکست

import { useState } from 'react';
import { format, addMonths, subMonths, startOfMonth, getDaysInMonth, addDays, parse } from 'date-fns-jalali';

const persianMonths = ['فروردین', 'اردیبهشت', 'خرداد', 'تیر', 'مرداد', 'شهریور', 'مهر', 'آبان', 'آذر', 'دی', 'بهمن', 'اسفند'];

const weekDays = ['ش', 'ی', 'د', 'س', 'چ', 'پ', 'ج'];

const toPersianDigits = (str: string): string => str.replace(/[0-9]/g, (d) => '۰۱۲۳۴۵۶۷۸۹'[+d]);

export default function Calendar() {
  const today = new Date();
  const [currentMonthStart, setCurrentMonthStart] = useState(startOfMonth(today));

  // مهم ترین استیت کامپوننت - استیتی که تاریخ های انتخاب شده توسط کاربر در آن ذخیره میشود
  const [selectedDates, setSelectedDates] = useState<string[]>([]);

  const persianYear = Number(format(currentMonthStart, 'yyyy'));
  const persianMonth = Number(format(currentMonthStart, 'M')) - 1;

  const daysInMonth = getDaysInMonth(currentMonthStart);

  const formattedMonthDate = format(currentMonthStart, 'yyyy/MM/dd');
  const gregorianDate = parse(formattedMonthDate, 'yyyy/MM/dd', new Date());
  const startDayOfWeek = (gregorianDate.getDay() + 1) % 7;

  const todayFormatted = format(today, 'yyyy/MM/dd');

  const daysArray: (Date | null)[] = [];
  for (let i = 0; i < startDayOfWeek; i++) daysArray.push(null);
  for (let d = 0; d < daysInMonth; d++) daysArray.push(addDays(currentMonthStart, d));

  function goToPrevMonth() {
    setCurrentMonthStart(subMonths(currentMonthStart, 1));
  }

  function goToNextMonth() {
    setCurrentMonthStart(addMonths(currentMonthStart, 1));
  }

  function toggleDate(date: Date) {
    const formatted = format(date, 'yyyy/MM/dd');
    setSelectedDates((prev) => (prev.includes(formatted) ? prev.filter((x) => x !== formatted) : [...prev, formatted]));
  }

  function removeDate(dateStr: string) {
    setSelectedDates((prev) => prev.filter((d) => d !== dateStr));
  }

  return (
    <div dir="rtl" className="mx-auto w-80 select-none rounded-lg bg-white p-4 shadow">
      {/* هدر */}
      <div className="mb-4 flex items-center justify-between">
        <button onClick={goToNextMonth} className="rounded px-2 py-1 text-xl transition hover:bg-gray-100">
          ▶️
        </button>

        {/* ماه */}
        <h2 className="text-lg font-semibold dark:text-black">
          {persianMonths[persianMonth]} {toPersianDigits(String(persianYear))}
        </h2>

        <button onClick={goToPrevMonth} className="rounded px-2 py-1 text-xl transition hover:bg-gray-100">
          ◀️
        </button>
      </div>

      {/* روز های هفته مثلا شنبه یکشنبه و غیره */}
      <div className="mb-2 grid grid-cols-7 gap-1 text-center text-sm font-medium text-gray-500">
        {weekDays.map((day, idx) => (
          <div key={idx}>{day}</div>
        ))}
      </div>

      {/* نمایش تاریخ روز ها */}
      <div className="mb-3 grid grid-cols-7 gap-1 border-y border-black pb-3">
        {daysArray.map((date, idx) => {
          if (date === null) return <div key={`empty-${idx}`} className="h-10" />;

          const formatted = format(date, 'yyyy/MM/dd');
          const isSelected = selectedDates.includes(formatted);
          const isToday = formatted === todayFormatted;

          return (
            <button
              key={formatted}
              onClick={() => toggleDate(date)}
              className={`relative flex h-10 items-center justify-center rounded text-sm transition ${
                isSelected ? 'bg-blue-500 font-bold text-white' : 'text-gray-700 hover:bg-gray-100'
              }`}
            >
              {toPersianDigits(format(date, 'd'))}

              {/* نشان‌گر دایره‌ای برای امروز */}
              {isToday && !isSelected && <span className="absolute bottom-1 h-1.5 w-1.5 rounded-full bg-blue-500" />}

              {isToday && isSelected && <span className="absolute bottom-1 h-1.5 w-1.5 rounded-full bg-white" />}
            </button>
          );
        })}
      </div>

      {/* نمایش تاریخ‌ های انتخاب شده */}
      {selectedDates.length > 0 && (
        <div className="space-y-2 text-right text-sm text-gray-600">
          <div className="font-medium">🗓️ تاریخ‌ های انتخاب‌ شده</div>

          <div className="flex flex-wrap gap-1">
            {selectedDates.map((d) => (
              <span key={d} className="inline-flex items-center gap-1 rounded-full bg-[#bebec0] px-2 py-1 text-xs font-bold text-blue-800">
                <button onClick={() => removeDate(d)} className="hover:text-red-600">
                  {toPersianDigits(d)}

                  <span>❌</span>
                </button>
              </span>
            ))}
          </div>
        </div>
      )}
    </div>
  );
}
```

```tsx
import Calendar from '@/components/Calendar';

export default function Page() {
  return <Calendar />;
}
```
