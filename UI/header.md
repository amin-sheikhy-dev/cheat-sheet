```tsx
import Link from 'next/link';

const items = [
  { name: 'Link 1', icon: '🔘', href: '/link1' },
  { name: 'Link 2', icon: '🔘', href: '/link2' },
  { name: 'Link 3', icon: '🔘', href: '/link3' },
  { name: 'Link 4', icon: '🔘', href: '/link4' },
];

export default function Header() {
  return (
    <>
      <header className="sticky top-0 z-10 border-b border-white/15 bg-white/0 text-white backdrop-blur-sm">
        <nav className="hidden py-4 md:block">
          <ul className="flex justify-center gap-12">
            {items.map((i) => (
              <li key={i.name}>
                <Link href={i.href} className="relative pb-1 text-sm hover:text-zinc-900">
                  {i.name}
                </Link>
              </li>
            ))}
          </ul>
        </nav>
      </header>

      <div className="fixed bottom-0 left-0 right-0 z-10 text-white md:hidden">
        <nav className="mx-2 mb-2 rounded-full border border-white/15 bg-white/5 backdrop-blur-[5px]">
          <ul className="flex justify-around px-3 py-1">
            {items.map((i) => (
              <li key={i.name} className="flex items-center justify-between">
                <Link href={i.href} className="flex flex-col items-center justify-between gap-[6px] text-xs">
                  {i.icon}

                  <span className="font-black">{i.name}</span>
                </Link>
              </li>
            ))}
          </ul>
        </nav>
      </div>
    </>
  );
}
```
