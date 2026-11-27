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

---

```tsx
'use client';

import { useState } from 'react';
import { format, addMonths, subMonths, startOfMonth, getDaysInMonth, addDays, parse, isAfter, isBefore, isEqual } from 'date-fns-jalali';
import { BiLeftArrow, BiRightArrow } from 'react-icons/bi';
import { LuCalendar } from 'react-icons/lu';
import { IconLabel } from '../ui/icon-label';
import { RiCloseLargeLine } from 'react-icons/ri';
import { FaArrowLeftLong } from 'react-icons/fa6';
import { toPersianDigits } from '@/lib/utils/to-persian-digits';

const persianMonths = ['فروردین', 'اردیبهشت', 'خرداد', 'تیر', 'مرداد', 'شهریور', 'مهر', 'آبان', 'آذر', 'دی', 'بهمن', 'اسفند'];
const weekDays = ['ش', 'ی', 'د', 'س', 'چ', 'پ', 'ج'];

interface DateRange {
  checkIn: string | null;
  checkOut: string | null;
}

export default function Calendar() {
  const today = new Date();
  const [currentMonthStart, setCurrentMonthStart] = useState(startOfMonth(today));
  const [dateRange, setDateRange] = useState<DateRange>({ checkIn: null, checkOut: null });
  // console.log(dateRange) | {checkIn: '1405/09/09', checkOut: '1405/09/23'}
  const [hoverDate, setHoverDate] = useState<string | null>(null);

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

  // Parse a yyyy/MM/dd jalali string to Date for comparison
  function parseJalali(str: string): Date {
    return parse(str, 'yyyy/MM/dd', new Date());
  }

  function isInRange(formatted: string): boolean {
    const { checkIn, checkOut } = dateRange;
    const end = checkOut ?? hoverDate;
    if (!checkIn || !end) return false;

    const d = parseJalali(formatted);
    const start = parseJalali(checkIn);
    const endDate = parseJalali(end);

    if (isEqual(start, endDate)) return false;

    const [from, to] = isAfter(start, endDate) ? [endDate, start] : [start, endDate];
    return isAfter(d, from) && isBefore(d, to);
  }

  function isRangeStart(formatted: string): boolean {
    const { checkIn, checkOut } = dateRange;
    const end = checkOut ?? hoverDate;
    if (!checkIn) return false;
    if (!end) return formatted === checkIn;

    const start = parseJalali(checkIn);
    const endDate = parseJalali(end);
    const [from] = isAfter(start, endDate) ? [endDate, start] : [start, endDate];
    return isEqual(parseJalali(formatted), from);
  }

  function isRangeEnd(formatted: string): boolean {
    const { checkIn, checkOut } = dateRange;
    const end = checkOut ?? hoverDate;
    if (!checkIn || !end) return false;

    const start = parseJalali(checkIn);
    const endDate = parseJalali(end);
    if (isEqual(start, endDate)) return false;
    const [, to] = isAfter(start, endDate) ? [endDate, start] : [start, endDate];
    return isEqual(parseJalali(formatted), to);
  }

  function handleDayClick(date: Date) {
    const formatted = format(date, 'yyyy/MM/dd');
    const { checkIn, checkOut } = dateRange;

    if (!checkIn || checkOut || formatted === checkIn) {
      setDateRange({ checkIn: formatted, checkOut: null });
      return;
    }

    const start = parseJalali(checkIn);
    const clicked = parseJalali(formatted);

    if (isBefore(clicked, start)) {
      setDateRange({ checkIn: formatted, checkOut: checkIn });
    } else {
      setDateRange({ checkIn, checkOut: formatted });
    }
  }

  function clearRange() {
    setDateRange({ checkIn: null, checkOut: null });
  }

  return (
    <div
      dir="rtl"
      className="border-primary-900/10 dark:bg-primary-900/80 dark:border-primary-700/40 shadow-primary-900/5 mx-auto w-80 rounded-2xl border bg-white/90 p-4 shadow-lg backdrop-blur-sm select-none dark:shadow-black/20"
    >
      <div className="mb-4 flex items-center justify-between">
        <button
          onClick={() => setCurrentMonthStart(addMonths(currentMonthStart, 1))}
          className="text-primary-600 hover:bg-accent-500/10 hover:text-accent-600 dark:text-primary-300 dark:hover:bg-accent-500/15 dark:hover:text-accent-400 rounded-lg px-2 py-1 text-xl transition"
        >
          <BiRightArrow className="dark:text-accent-400 text-accent-600" size={26} />
        </button>

        <h2 className="text-primary-950 text-lg font-bold dark:text-white">
          {persianMonths[persianMonth]} {toPersianDigits(String(persianYear))}
        </h2>

        <button
          onClick={() => setCurrentMonthStart(subMonths(currentMonthStart, 1))}
          className="text-primary-600 hover:bg-accent-500/10 hover:text-accent-600 dark:text-primary-300 dark:hover:bg-accent-500/15 dark:hover:text-accent-400 rounded-lg px-2 py-1 text-xl transition"
        >
          <BiLeftArrow className="dark:text-accent-400 text-accent-600" size={26} />
        </button>
      </div>

      <div className="text-primary-900/50 dark:text-primary-300/60 mb-2 grid grid-cols-7 gap-1 text-center text-sm font-bold">
        {weekDays.map((day, idx) => (
          <div key={idx}>{day}</div>
        ))}
      </div>

      <div className="border-primary-900/10 dark:border-primary-700/40 mb-3 grid grid-cols-7 gap-y-1 border-y pb-3">
        {daysArray.map((date, idx) => {
          if (date === null) return <div key={`empty-${idx}`} className="h-10" />;

          const formatted = format(date, 'yyyy/MM/dd');
          const isToday = formatted === todayFormatted;
          const rangeStart = isRangeStart(formatted);
          const rangeEnd = isRangeEnd(formatted);
          const inRange = isInRange(formatted);
          const isSelected = rangeStart || rangeEnd;

          return (
            <div
              key={formatted}
              className={`relative flex h-10 items-center justify-center ${
                inRange ? 'bg-accent-500/15 dark:bg-accent-500/20' : ''
              } ${rangeStart ? 'rounded-r-lg' : ''} ${rangeEnd ? 'rounded-l-lg' : ''} ${!rangeStart && !rangeEnd && inRange ? 'rounded-none' : ''}`}
              onMouseEnter={() => !dateRange.checkOut && setHoverDate(formatted)}
              onMouseLeave={() => setHoverDate(null)}
            >
              <button
                onClick={() => handleDayClick(date)}
                className={`relative flex h-9 w-9 items-center justify-center rounded-full text-sm transition ${
                  isSelected
                    ? 'bg-accent-500 text-primary-950 shadow-accent-500/25 font-bold shadow-md'
                    : inRange
                      ? 'text-accent-700 dark:text-accent-300 hover:bg-accent-500/20'
                      : 'text-primary-800 hover:bg-accent-500/10 hover:text-accent-700 dark:text-primary-200 dark:hover:bg-accent-500/15 dark:hover:text-accent-400'
                }`}
              >
                {toPersianDigits(format(date, 'd'))}

                {isToday && !isSelected && <span className="bg-accent-500 absolute bottom-1 h-1.5 w-1.5 rounded-full" />}
                {isToday && isSelected && <span className="bg-primary-950 absolute bottom-1 h-1.5 w-1.5 rounded-full" />}
              </button>
            </div>
          );
        })}
      </div>

      {(dateRange.checkIn || dateRange.checkOut) && (
        <div className="text-primary-900/70 dark:text-primary-200/70 space-y-2 text-right text-sm">
          <div className="text-primary-950 font-medium dark:text-white">
            <IconLabel icon={<LuCalendar className="text-accent-600 dark:text-accent-400" />} iconDir="left">
              بازه انتخاب‌ شده
            </IconLabel>
          </div>

          <div className="flex items-center justify-between gap-2">
            <div className="bg-accent-500/15 dark:bg-accent-500/20 flex flex-1 flex-col rounded-xl px-3 py-2">
              <span className="text-primary-500 dark:text-primary-400 text-[10px]">ورود</span>
              <span className="text-accent-700 dark:text-accent-300 text-xs font-bold">
                {dateRange.checkIn ? toPersianDigits(dateRange.checkIn) : '-'}
              </span>
            </div>

            <FaArrowLeftLong className="text-primary-400 dark:text-primary-500" size={15} />

            <div className="bg-accent-500/15 dark:bg-accent-500/20 flex flex-1 flex-col rounded-xl px-3 py-2">
              <span className="text-primary-500 dark:text-primary-400 text-[10px]">خروج</span>
              <span className="text-accent-700 dark:text-accent-300 text-xs font-bold">
                {dateRange.checkOut ? toPersianDigits(dateRange.checkOut) : '-'}
              </span>
            </div>

            <button onClick={clearRange} className="rounded-lg p-1.5 transition hover:bg-red-500/10" title="پاک کردن">
              <RiCloseLargeLine className="text-red-500" size={15} />
            </button>
          </div>
        </div>
      )}
    </div>
  );
}
```
