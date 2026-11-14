```tsx
import React from 'react';

type IconLabelProps = {
  children: React.ReactNode;

  icon: React.ReactNode;

  iconDir: 'left' | 'right';

  gap?: number;
};

export function IconLabel({ children, icon, iconDir, gap = 1 }: IconLabelProps) {
  return (
    <div className={`flex items-center gap-${gap}`}>
      {iconDir === 'left' && icon}

      <span>{children}</span>

      {iconDir === 'right' && icon}
    </div>
  );
}

//

<IconLabel iconDir="left" icon={<SquaresIcon size={30} className="inline text-[#A78BFA]" />}>
  Categories
</IconLabel>;
```
