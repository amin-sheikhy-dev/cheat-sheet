# پیش نیاز ها: Tailwind-css & Next.js or React

### کد های زیر در کامپوننت parent هستن

```tsx
import { StaticImageData } from 'next/image';

// ایمپورت عکس ها
import img1 from '@/src';
import img2 from '@/src';
import img3 from '@/src';
import img4 from '@/src';

// تایپ عکس ها - در صورتی که از تایپ اسکریپت استفاده میکنید
export interface ImagesTypes {
  image: StaticImageData;
  alt: string;
}

// اکسپورت ارایه
export const images: ImagesTypes[] = [
  { image: Img1, alt: 'img1' },
  { image: Img2, alt: 'img2' },
  { image: Img3, alt: 'img3' },
  { image: Img4, alt: 'img4' },
];

// فرستادن عکس ها به عنوان پراپ کامپوننت
<ImageSwiper images={images} />;
```

### خود کامپوننت اصلی

```tsx
'use client'; // در صورتی که از نکست استفاده میکنید

import { useState } from 'react';
import { ImagesTypes } from '../sections/project-section';
import Image from 'next/image';

export default function ImageSwiper({ images }: { images: ImagesTypes[] }) {
  const [currentIndex, setCurrentIndex] = useState<number>(0);

  function goToPrev(): void {
    setCurrentIndex((prev) => (prev === 0 ? images.length - 1 : prev - 1));
  }
  function goToNext(): void {
    setCurrentIndex((prev) => (prev === images.length - 1 ? 0 : prev + 1));
  }

  return (
    <div dir="ltr" className="relative w-full">
      <div className="w-full overflow-hidden">
        <div className="flex w-full duration-700 ease-out" style={{ transform: `translateX(-${currentIndex * 100}%)` }}>
          {images.map((image) => (
            <div key={image.alt} className="w-full flex-shrink-0">
              <Image src={image.image} alt={image.alt} className="rounded-2xl" />
            </div>
          ))}
        </div>
      </div>

      <button onClick={goToPrev} className="absolute left-2 top-1/2 -translate-y-3/4 rounded-full bg-white/20 p-2 text-2xl backdrop-blur-sm">
        ◀️
      </button>

      <button onClick={goToNext} className="absolute right-2 top-1/2 -translate-y-3/4 rounded-full bg-white/20 p-2 text-2xl backdrop-blur-sm">
        ▶️
      </button>

      <div className="mt-4 flex justify-center gap-2">
        {images.map((_, index) => (
          <button key={index} className={`h-2 rounded-full bg-black dark:bg-white ${currentIndex === index ? 'w-6' : 'w-2'}`} />
        ))}
      </div>
    </div>
  );
}
```
