# Framer Motion

کتابخانه انیمیشن برای React - انیمیشن با props به جای CSS keyframe و کلاس‌ها.

> از نسخه ۱۱ به بعد کتابخانه به `motion` تغییر نام داده - پکیج `motion` و ایمپورت از `motion/react`. پکیج `framer-motion` هنوز کار میکند و API هر دو یکسانه.

## نصب

```bash
npm i framer-motion
```

```tsx
import { motion } from 'framer-motion';
```

## کامپوننت motion

به جای تگ‌های html از نسخه motion آن‌ها استفاده می‌کنیم - `motion.div` ، `motion.button` و...

سه prop اصلی:

- `initial` = حالت اولیه قبل از شروع انیمیشن
- `animate` = حالتی که المان به آن می‌رود
- `transition` = چجوری و چقدر طول میکشد

```tsx
import { motion } from 'framer-motion';

export default function App() {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }} // موقع mount از این حالت شروع میشه
      animate={{ opacity: 1, y: 0 }} // به این حالت انیمیت میشه
      transition={{ duration: 0.5 }}
    >
      Hello
    </motion.div>
  );
}
```

هر وقت `animate` عوض بشه، المان به حالت جدید انیمیت میشه - بدون کد اضافه:

```tsx
const [isOpen, setIsOpen] = useState(false);

<motion.div animate={{ width: isOpen ? 240 : 64 }} transition={{ duration: 0.3 }} />;
```

## transition - نوع حرکت

دو نوع حرکت داریم:

- `tween` = حرکت با زمان مشخص و یکنواخت - پیش‌فرض برای opacity و رنگ و...
- `spring` = حرکت فیزیکی مثل فنر - پیش‌فرض برای transform ها

```tsx
// spring - سه تنظیم اصلی
<motion.div
  animate={{ scale: 1.2 }}
  transition={{
    type: 'spring',
    stiffness: 500, // سفتی فنر - بیشتر = تندتر
    damping: 20, // مقاومت - کمتر = بیشتر لرزیدن
    mass: 1, // سنگینی المان
  }}
/>

// tween - با ease
<motion.div
  animate={{ x: 100 }}
  transition={{
    type: 'tween',
    duration: 0.5, // ثانیه
    ease: 'easeInOut', // easeIn | easeOut | easeInOut | linear | cubic-bezier
    delay: 0.2,
  }}
/>
```

تکرار انیمیشن:

```tsx
<motion.div
  animate={{ rotate: 360 }}
  transition={{
    duration: 2,
    repeat: Infinity, // بینهایت - یا عدد دلخواه
    repeatType: 'loop', // loop | reverse | mirror
    repeatDelay: 0.5, // تاخیر بین هر تکرار
  }}
/>
```

## Keyframes - انیمیشن چند مرحله‌ای

به جای یک مقدار، آرایه بده - المان از روی اعضای آرایه رد میشه:

```tsx
<motion.div
  animate={{
    scale: [1, 1.5, 1.5, 1, 1],
    rotate: [0, 0, 180, 180, 0],
    borderRadius: ['0%', '0%', '50%', '50%', '0%'],
  }}
  transition={{
    duration: 2,
    ease: 'easeInOut',
    times: [0, 0.2, 0.5, 0.8, 1], // زمان هر مرحله از کل انیمیشن - بین 0 تا 1 - طولش باید با آرایه ها برابر باشه
    repeat: Infinity,
    repeatDelay: 1,
  }}
/>
```

## whileHover - whileTap - whileFocus - whileInView

انیمیشن‌های تعاملی - وقتی اتفاق افتادند به حالت while می‌روند و بعد برمی‌گردند:

```tsx
<motion.button
  whileHover={{ scale: 1.05 }} // وقتی موس روشه
  whileTap={{ scale: 0.95 }} // وقتی کلیک/لمس شده
  transition={{ type: 'spring', stiffness: 500 }}
>
  Submit
</motion.button>
```

```tsx
<motion.input whileFocus={{ scale: 1.02, borderColor: '#2563eb' }} />
```

انیمیشن موقع اسکرول و ورود المان به دید:

```tsx
<motion.div
  initial={{ opacity: 0, y: 40 }}
  whileInView={{ opacity: 1, y: 0 }} // وقتی وارد صفحه دید شد
  viewport={{ once: true, amount: 0.5 }} // once = فقط بار اول - amount = چند درصدش دیده بشه
  transition={{ duration: 0.6 }}
>
  Card
</motion.div>
```

## drag

```tsx
import { useRef } from 'react';

export default function SnapBackDrag() {
  const constraintsRef = useRef(null);

  return (
    <div ref={constraintsRef} className="relative w-96 h-96 border">
      <motion.div
        drag // فعال کردن درگ در دو محور
        // drag="x" فقط افقی
        // drag="y" فقط عمودی

        dragConstraints={constraintsRef} // محدوده حرکت = داخل المان والد
        // یا به صورت عددی:
        // dragConstraints={{ left: -100, right: 100, top: -100, bottom: 100 }}

        dragElastic={0.2} // شل بودن محدوده - 0 = سفت و خشک
        dragSnapToOrigin // بعد از رها کردن به نقطه اول برگردد
        whileDrag={{ scale: 0.95, cursor: 'grabbing' }}
        onDragEnd={(_, info) => {
          // info.point = مختصات صفحه - info.offset = جابجایی از شروع درگ
          console.log(info.offset.x, info.offset.y);
        }}
      />
    </div>
  );
}
```

## variants - تعریف چند حالت انیمیشن

به جای تکرار initial و animate، حالت‌ها را با اسم ذخیره می‌کنیم:

```tsx
const modalVariants = {
  hidden: { opacity: 0, scale: 0 },
  visible: { opacity: 1, scale: 1, transition: { type: 'spring', stiffness: 70 } },
};

<motion.dialog
  variants={modalVariants}
  initial="hidden"
  animate="visible"
  exit="hidden" // حالت خروج - با AnimatePresence کار میکنه
/>;
```

**propagation**: variant ها به فرزندان هم منتقل می‌شوند - فقط کافیست فرزند خودش variants داشته باشد و والد initial و animate را تعریف کند:

```tsx
const container = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: {
      staggerChildren: 0.2, // فرزندان پشت سر هم با تاخیر انیمیت میشن
      delayChildren: 0.3, // تاخیر قبل از شروع
    },
  },
};

const item = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0 },
};

<motion.ul variants={container} initial="hidden" animate="visible">
  {/* هر li خودش به ترتیب انیمیت میشه */}
  <motion.li variants={item}>Item 1</motion.li>
  <motion.li variants={item}>Item 2</motion.li>
  <motion.li variants={item}>Item 3</motion.li>
</motion.ul>;
```

اگر شرط داریم که هر فرزند انیمیشن متفاوتی داشته باشد، از `custom` استفاده می‌کنیم:

```tsx
const item = {
  hidden: (i: number) => ({ opacity: 0, x: -50 * i }),
  visible: (i: number) => ({ opacity: 1, x: 0, transition: { delay: i * 0.1 } }),
};

{
  items.map((label, i) => (
    <motion.div key={label} custom={i} variants={item} initial="hidden" animate="visible">
      {label}
    </motion.div>
  ));
}
```

## AnimatePresence - انیمیشن خروج

مشکل: وقتی کامپوننت از DOM حذف می‌شود، React فوراً پاکش می‌کند و فرصت انیمیشن خروج (`exit`) نیست.

راه حل: رندر شرطی را داخل `AnimatePresence` می‌گذاریم تا بعد از تمام شدن انیمیشن exit، از DOM حذف شود.

دو قانون مهم:

- المان داخلش باید **شرطی رندر** شود یا `key` داشته باشد
- کامپوننت حذف‌شونده باید `motion` باشد و prop `exit` داشته باشد

```tsx
import { motion, AnimatePresence } from 'framer-motion';
import { useState } from 'react';

function ToggleButton() {
  const [isVisible, setIsVisible] = useState(true);

  return (
    <div>
      <button onClick={() => setIsVisible(!isVisible)}>Toggle</button>

      <AnimatePresence>
        {isVisible && (
          <motion.div
            key="box"
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -20 }} // انیمیشن خروج
            transition={{ duration: 0.5 }}
          >
            Hello
          </motion.div>
        )}
      </AnimatePresence>
    </div>
  );
}
```

نکات:

```tsx
// mode="wait" - اول خروج قبلی کامل بشه بعد ورود جدید شروع بشه - برای تب ها و اسلایدر
<AnimatePresence mode="wait">
  <motion.div key={activeTab} exit={{ opacity: 0 }} animate={{ opacity: 1 }}>
    {activeTab}
  </motion.div>
</AnimatePresence>

// initial={false} - موقع اولین رندر انیمیشن ورود اجرا نشه
<AnimatePresence initial={false}>{/* ... */}</AnimatePresence>
```

مثال واقعی - مودال با portal:

```tsx
<AnimatePresence>{isCreatingNewChallenge && <NewChallenge onDone={handleDone} />}</AnimatePresence>;
// در کامپوننت والد

// در خود کامپوننت مودال
return createPortal(
  <>
    <div className="backdrop" onClick={onClose} />

    <motion.dialog className="modal" open variants={modalVariants} initial="hidden" animate="visible" exit="hidden">
      <h2>{title}</h2>
      {children}
    </motion.dialog>
  </>,
  document.body
);
```

## useScroll و useTransform - انیمیشن مبتنی بر اسکرول

`useScroll` مقدار اسکرول صفحه را به صورت motion value می‌دهد.

`useTransform` یک motion value را به مقدار انیمیشنی دیگر تبدیل می‌کند - آرگومان اول هوک، آرگومان دوم ورودی‌ها و آرگومان سوم خروجی‌ها:

```tsx
import { motion, useScroll, useTransform } from 'framer-motion';

export default function WelcomePage() {
  const { scrollY } = useScroll(); // مقدار اسکرول عمودی صفحه - پیکسل

  // وقتی اسکرول به 200 میرسه opacity نصف و به 500 میرسه صفر میشه
  const opacityCity = useTransform(scrollY, [0, 200, 300, 500], [1, 0.5, 0.4, 0]);

  // عکس با اسکرول بالا میره - پارالاکس
  const yCity = useTransform(scrollY, [0, 200], [0, -100]);

  const opacityHero = useTransform(scrollY, [0, 300, 500], [1, 1, 0]);
  const scaleText = useTransform(scrollY, [0, 300], [1, 1.5]);

  return (
    <header id="welcome-header">
      <motion.div style={{ scale: scaleText }} id="welcome-header-content">
        <h1>Ready for a challenge?</h1>
      </motion.div>

      {/* motion value ها را در style می‌دهیم - نه در animate */}
      <motion.img style={{ opacity: opacityCity, y: yCity }} src={cityImg} alt="city" id="city-image" />
      <motion.img style={{ opacity: opacityHero }} src={heroImg} alt="hero" id="hero-image" />
    </header>
  );
}
```

اسکرول نسبت به یک المان مشخص - مثلا نوار پیشرفت:

```tsx
const containerRef = useRef(null);

const { scrollYProgress } = useScroll({
  target: containerRef, // اسکرول داخل این المان چک بشه
  offset: ['start end', 'end start'], // از جایی که المان وارد دید میشه تا خارج میشه
});

<motion.div style={{ scaleX: scrollYProgress }} className="progress-bar" />;
```

## useMotionValue و useSpring و useMotionValueEvent

`useMotionValue` یک مقدار متحرک است - تغییرش باعث re-render نمیشود (برعکس useState):

```tsx
import { motion, useMotionValue, useSpring, useMotionValueEvent, useTransform } from 'framer-motion';

export default function App() {
  const x = useMotionValue(0);

  // نسخه نرم شده - حرکت رو فنری و طبیعی میکنه
  const springX = useSpring(x, { stiffness: 300, damping: 30 });

  // گوش دادن به تغییر - به جای useEffect
  useMotionValueEvent(x, 'change', (latest) => {
    console.log(latest);
  });

  // تبدیل مشتق شده - با هر تغییر x خودکار آپدیت میشه
  const background = useTransform(x, [-100, 100], ['#ff6b6b', '#4ecdc4']);

  return (
    <>
      <motion.div style={{ x: springX, background }} />
      <button onClick={() => x.set(x.get() + 50)}>Move</button>
    </>
  );
}
```

## layout و layoutId - انیمیشن خودکار چیدمان

`layout` = وقتی چیدمان المان عوض میشه (سایز/جابجایی)، خودکار به موقعیت جدید انیمیت میشه:

```tsx
const [isExpanded, setIsExpanded] = useState(false);

<motion.div layout onClick={() => setIsExpanded(!isExpanded)} className={isExpanded ? 'big' : 'small'}>
  Click me
</motion.div>;
```

`layoutId` = المان‌های مختلف با یک id مشترک، مثل یک چیز واحد انیمیت می‌شوند - برای تب اندرلاین و expand کردن کارت:

اندرلاین تب ها - در هر لحظه فقط یکی از این ها رندر میشه

ولی چون layoutId مشترک دارن، بین تب ها کش میاد

```tsx
{
  tabs.map((tab) => (
    <button key={tab.id} onClick={() => setActive(tab.id)}>
      {tab.label}

      {active === tab.id && <motion.div layoutId="underline" className="underline" />}
    </button>
  ));
}
```

expand کردن کارت - در لیست و در حالت باز layoutId یکی است

```tsx
{
  cards.map((card) => (
    <motion.div key={card.id} layoutId={card.id} onClick={() => setSelectedId(card.id)}>
      {card.title}
    </motion.div>
  ));
}

<AnimatePresence>
  {selectedId && (
    <motion.div className="expanded-card" layoutId={selectedId}>
      {content}
    </motion.div>
  )}
</AnimatePresence>;
```

> شرط layoutId: در هر لحظه فقط **یک** المان با آن id روی صفحه باشد.

## جمع‌بندی props

| prop / hook           | کار                                              |
| --------------------- | ------------------------------------------------ |
| `initial`             | حالت شروع                                        |
| `animate`             | حالت هدف - با تغییرش انیمیت میشه                 |
| `exit`                | حالت خروج - فقط داخل AnimatePresence کار میکنه   |
| `transition`          | نوع و مدت حرکت - spring / tween / times / repeat |
| `variants`            | تعریف حالت‌های اسم‌دار - به فرزندان منتقل میشه   |
| `whileHover`          | موقع هاور                                        |
| `whileTap`            | موقع کلیک                                        |
| `whileDrag`           | حین درگ                                          |
| `whileFocus`          | موقع فوکوس                                       |
| `whileInView`         | موقع ورود به دید - با viewport                   |
| `drag`                | فعال‌سازی درگ                                    |
| `layout` / `layoutId` | انیمیشن خودکار چیدمان                            |
| `useScroll`           | مقدار اسکرول صفحه یا یک المان                    |
| `useTransform`        | تبدیل motion value به مقدار انیمیشنی             |
| `useSpring`           | نرم و فنری کردن حرکت                             |
| `useMotionValue`      | مقدار متحرک بدون re-render                       |
| `useMotionValueEvent` | گوش دادن به تغییر motion value                   |
| `AnimatePresence`     | اجرای انیمیشن خروج برای رندر شرطی                |

## نکات ریز

- مدت زمان‌ها **ثانیه** هستند نه میلی‌ثانیه
- طول آرایه `times` باید با تعداد keyframe ها برابر باشد
- motion value ها در `style` داده می‌شوند نه در `animate`
- تغییر motion value باعث re-render نمیشود - مزیت بزرگ نسبت به state
- `exit` بدون `AnimatePresence` هیچ کاری نمیکند
- برای انیمیشن متوالی چند المان `staggerChildren` در transition والد
- درگ با `dragConstraints={ref}` به محدوده والد محدود میشود
