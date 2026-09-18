# پیش نیاز ها: Tailwind-css & Next.js or React

### کد های زیر در کامپوننت parent هستن

```tsx
import { StaticImageData } from 'next/image';

// ایمپورت عکس ها
import img1 from '@/src';
import img2 from '@/src';
import img3 from '@/src';
import img4 from '@/src';

// مشخص کردن تایپ ابجکت - در صورتی که از تایپ اسکریپت استفاده میکنید
interface ImagesTypes {
  image: StaticImageData;
  alt: string;
}

// ارایه ای که به عنوان پراپ باید بدی به کامپوننت
const images: ImagesTypes[] = [
  { image: Img1, alt: 'img1' },
  { image: Img2, alt: 'img2' },
  { image: Img3, alt: 'img3' },
  { image: Img4, alt: 'img4' },
];

// در نهایت کامپوننت شما باید به این شکل باشد
<ImageSlider images={images} />;
```

---

### خود کامپوننت اصلی image slider

```tsx
'use client'; // در صورتی که از نکست استفاده میکنید

import { useEffect, useState } from 'react';
import Image from 'next/image';
import { ImagesTypes } from '../sections/project-section';

export default function ImageSlider({ images }: { images: ImagesTypes[] }) {
  const [currentImageIndex, setCurrentImageIndex] = useState(0);

  useEffect(() => {
    if (!images.length) return;
    const interval = setInterval(() => {
      setCurrentImageIndex((prevIndex) => (prevIndex + 1) % images.length);
    }, 3 * 1000); // هر چند ثانیه عکس تغییر کند

    return () => clearInterval(interval);
  }, [images.length]);

  if (!images.length) return null;

  return (
    <div className="relative h-full w-full rounded-xl">
      {images.map((image, index) => (
        <Image
          key={index}
          src={image.image}
          fill
          priority={index === 0}
          sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
          className={`rounded-xl object-cover transition-[transform,opacity] duration-500 ease-in-out will-change-transform ${
            index === currentImageIndex ? 'z-[1] translate-x-0 rotate-0 scale-100 opacity-100' : '-rotate-5 -translate-x-4 scale-110 opacity-0'
          }`}
          alt={image.alt}
        />
      ))}
    </div>
  );
}
```
