```tsx
import Image from 'next/image';
import bg from '@/assets/background-img.jpg';

export default function BackgroundImg() {
  return (
    <div className="pointer-events-none fixed inset-0 -z-10 overflow-hidden">
      <Image src={bg} alt="background" fill priority className="object-cover" />
    </div>
  );
}
```
