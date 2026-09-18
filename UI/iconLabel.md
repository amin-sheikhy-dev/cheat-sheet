```tsx
type IconLabelProps = {
  children: React.ReactNode;
  icon: React.ReactNode;
  iconDir: 'l' | 'r';
};

export function IconLabel({ children, icon, iconDir }: IconLabelProps) {
  return (
    <div className="flex items-center gap-1">
      {iconDir === 'l' && icon}
      <span>{children}</span>
      {iconDir === 'r' && icon}
    </div>
  );
}
```
