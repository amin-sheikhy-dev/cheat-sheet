یک هوک از react-dom هست که وضعیت ارسال فرم (Pending) رو نشون میده.
pending: وقتی فرم در حال ارسال هست، مقدار true میشه و دکمه غیرفعال (Disabled) میشه و متن "Submitting..." نمایش داده میشه.

فقط داخل کامپوننتی کار می‌کنه که child یک <form> باشه — نه خود کامپوننت فرم.

پس باید دکمه رو به یک کامپوننت جداگانه تبدیل کنی:

```tsx
"use client";

import { useFormStatus } from "react-dom";

export function SubmitButton({ label, loadingLabel }: Record<string, string>) {
  const { pending } = useFormStatus();

  return (
    <button type="submit" disabled={pending}>
      {pending ? loadingLabel : label}
    </button>
  );
}
```

```tsx
import { SubmitButton } from "@/components/SubmitButton";

<form onSubmit={handleSubmit}>
  {/* ... inputs */}

  <SubmitButton label="ثبت رمز عبور جدید" loadingLabel="در حال ثبت..." />
</form>;
```
