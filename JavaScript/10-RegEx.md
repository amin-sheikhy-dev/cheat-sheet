```js
const text = 'hello world';
const regex = /hello/;

regex.test(text); // true - این متد تست فقط بولین میده

const regex2 = /cat/;

regex2.test('my cat is cute'); // true
regex2.test('concatenate'); // true
regex2.test('Cat'); // false - حساس به حروف بزرگ و کوچیک

// . هر کاراکتر تکی

/c.t/.test('cat'); // true
/c.t/.test('c9t'); // true
/c.t/.test('c#t'); // true
/c.t/.test('c t'); // true

/c.t/.test('ct'); // false - چون نقطه باید دقیقاً یک کاراکتر را پوشش دهد

// [] وقتی از براکت استفاده می‌کنیم یعنی یکی از کاراکترهای داخل براکت

/[abc]/.test('hello a'); // true - چون داخلش یه آ هست

/[abcdef]/;
// بالایی و پایینی با هم فرقی ندارن
/[a-f]/; // از آ تا اف

/[a-z]/; // حروف کوچک انگلیسی
/[A-Z]/; // حروف بزرگ
/[0-9]/; // اعداد

/[a-zA-Z]/; // همه حروف انگلیسی کوچک و بزرگ

/[a-zA-Z]/.test('7'); // false
/[a-zA-Z]/.test('A'); // true

/[a-zA-Z0-9]/; // حرف کوچک - حرف بزرگ - عدد

// ^ نقیض
/[^0-9]/; // هر چیزی به جز عدد

/[^0-9]/.test('a'); // true
/[^0-9]/.test('%'); // true
/[^0-9]/.test(' '); // true
/[^0-9]/.test('2'); // false

// Quantifiers (تعیین تعداد تکرار)

// + یک بار یا بیشتر

/a+/.test('a'); // true
/a+/.test('aaaa'); // true
/a+/.test('aaaaaaaa'); // true
/a+/.test('34'); // false
/a+/.test(' '); // false

//` * صفر بار یا بیشتر

// وجود نداشته باشد a ممکن است اصلاً
/a*/.test(''); // true
/a*/.test('a'); // true
/a*/.test('aaaa'); // true
/a*/.test('aaaaaaaa'); // true
/a*/.test('34'); // true
/a*/.test(' '); // true

//` ? صفر یا یک بار

// اختیاری است u یعنی
/colou?r/.test('color');
/colou?r/.test('colour');

// {n} بار n دقیقاً

// دقیقا 3 بار
/a{3}/.test('a'); // false
/a{3}/.test('aaa'); // true
/a{3}/.test('aaaaa'); // false

// حداقل 2 بار و حداکثر 4 بار
/a{2,4}/.test('a'); // false
/a{2,4}/.test('aa'); // true
/a{2,4}/.test('aaaa'); // true
/a{2,4}/.test('aaaaaaa'); // false

// حداقل 3 بار
/a{3,}/.test('aa'); // false
/a{3,}/.test('aaa'); // true
/a{3,}/.test('aaaaa'); // true
/a{3,}/.test('aaaaaaaa'); // true

// سه رقم عدد
/[0-9]{3}/.test(352); // true
/[0-9]{3}/.test('352'); // true
// پنج حرف انگلیسی
/[a-zA-Z]{5}/.test('React'); // true

// یک یا چند رقم
/[0-9]+/.test(0); // true
/[0-9]+/.test('0'); // true
/[0-9]+/.test(231212); // true

// (Character Classes) شورت‌کات‌های پرکاربرد
// تمام اعداد
/[0-9]/;
// پایینی معادل بالاییه
/\d/;
// d = digit مخفف

/\d/.test(5); // true
/\d/.test('5'); // true
/\d/.test('a'); // false

/\d+/.test('2323'); // true

// یعنی هر چیزی غیر از عدد
/[^0-9]/;
// پایینی معادل بالاییه
/\D/;

// حروف انگلیسی - اعداد - آندرلاین
/[a-zA-Z0-9_]/;
// پایینی معادل بالاییه
/\w/;
// w = Word مخفف

/\w/.test('A'); // true
/\w/.test('_'); // true
/\w/.test('@'); // false

// غیر از حروف انگلیسی - اعداد - آندرلاین
/[^a-zA-Z0-9_]/;
// پایینی معادل بالاییه
/\W/;
/\W/.test('@'); // true

// معادل هر اسپیسی
// space tab newline carriage
/\s/.test(' '); // true
/\s/.test('\n'); // true

// هر چیزی غیر از فاصله
/\S/.test('a'); // true
/\S/.test(' '); // false

// Anchor
// ^ شروع رشته
/^hello/.test('hello world'); // true
/^hello/.test('say hello'); // false

// $ پایان رشته
/world$/.test('hello world'); // true
/world$/.test('world hello'); // false

// ترکیب ^$

// باشد hello رشته باید دقیقاً
/^hello$/.test('hello'); // true
/^hello$/.test('hello world'); // false

// فقط اعداد
/^\d+$/.test('2'); // true
/^\d+$/.test('245678790'); // true
/^\d+$/.test('abc56789'); // false

// فقط اعداد 4 رقمی
/^\d{4}$/.test('6789'); // true
/^\d{4}$/.test('36789'); // false

// یه حرف حداقل 3 رقم و حداکثر 10 رقمی - این مثال در نام کاربری استفاده میشه
/^\w{3,10}$/.test('user_123'); // true
/^\w{3,10}$/.test('user-123'); // false

// کل رشته باید فقط عدد باشد
/\d+/.test('abc123xyz'); // true
/^\d+$/.test('abc123xyz'); // false

/^[A-Z]{2}\d{4}$/; // دوتا حروف بزرگ و چهار تا عدد

// کاربر فقط اجازه داره حروف فارسی و فاصله تایپ کنه
/^[آابپتثجچحخدذرزژسشصضطظعغفقکگلمنوهئی‌]+$/.test('امین');

// | همان یا است

// cat یا dog
/cat|dog/.test('cute cat'); // true
/cat|dog/.test('dog'); // true
/cat|dog/.test('my dog'); // true
/cat|dog/.test('bird'); // false

// () برای کنترل اولویت از پرانتز استفاده می‌کنیم
// باشد dog یا cat کل رشته باید دقیقاً
/^(cat|dog)$/.test('cat'); // true
/^(cat|dog)$/.test('dog'); // true

/(cat|dog)/.test('cat123'); // true
/^(cat|dog)$/.test('cat123'); // false

/(cat|dog)/.test('my dog'); // true
/^(cat|dog)$/.test('my dog'); // false

// را گروه‌بندی کنیم Regex می‌توانیم بخشی از
// ha یک یا چند بار
/(ha)+/.test('ha'); // true
/(ha)+/.test('haha'); // true

/(ha)+/.test('haha#93'); // true
/^(ha)+$/.test('haha#93'); // false

/(ha)+/.test('ha1'); // true
/(ha)+/.test('he'); // false

// فقط 3 بار گو تکرار شه
/(go){3}/.test('gogogo'); // true

/(go){3}/.test('gogogo-'); // true
/^(go){3}$/.test('gogogo-'); // false

/(go){3}/.test('go'); // false
/(go){3}/.test('gog'); // false

//.test('');

// هم می‌کند Capture پرانتز فقط برای گروه‌ بندی نیست به صورت پیش‌فرض

const result1 = 'Ali-25'.match(/(\w+)-(\d+)/);
['Ali-25', 'Ali', '25']; // اینو میده

result1[0]; // Ali-25
result1[1]; // Ali
result1[2]; // 25

const result1_2 = 'Ali-25'.match(/\w+-\d+/);
['Ali-25']; // اینو میده

const result2 = '2026-05-31'.match(/(\d{4})-(\d{2})-(\d{2})/);
['2026-05-31', '2026', '05', '31'];

const result3 = 'Ali Ahmadi'.match(/(\w+) (\w+)/);
['Ali Ahmadi', 'Ali', 'Ahmadi'];

const result4 = 'Reza Mohammady'.match(/(\w+)\s(\w+)/);
['Reza Mohammady', 'Reza', 'Mohammady'];

const result5 = 'email: ali@gmail.com'.match(/email: (.+)/);
['email: ali@gmail.com', 'ali@gmail.com'];

const result6 = 'email: ali@gmail.com'.match(/(email:) (.+)/);
['email: ali@gmail.com', 'email:', 'ali@gmail.com'];

// Flags = را تغییر بده Regex رفتار

// i = ignore Case
// به حروف بزرگ کوچیک حساس نیست

/cat/i.test('CaT'); // true
/cat/i.test('CAT'); // true

// g = global
// بدون فلگ گلوبال فقط اولین نتیجه رو نمایش میده
'cat dog cat bird'.match(/cat/);
['cat'];

'cat dog cat bird cat'.match(/cat/g);
['cat', 'cat', 'cat'];

'1 22 333'.match(/\d+/g);
['1', '22', '333'];

// m = multiline
// برای متن‌های چندخطی است Flag این

const multiline = `hello
world`;

multiline.match(/^world/); // پیدا نمیشه چون فقط اولین خطو میبینه
multiline.match(/^world/m); // الان درسته چون ابتدای هر خط را بررسی می‌کند

// ترکیب فلگ ها

'Cat cat CAT'.match(/cat/gi);
['Cat', 'cat', 'CAT'];

// چند تا مثال کاربردی تا اینجا

/^\w{3,20}$/; // نام کاربری

/^[^\s@]+@[^\s@]+\.[^\s@]+$/; // ایمیل

/^(?=.*[A-Z])(?=.*\d).+$/; // پسورد

// replace(regex, replacement)
'1-2-3'.replace(/-/g, '/'); // 1/2/3

// تبدیل فاصله‌ های متعدد به یک فاصله
'hello     world'.replace(/\s+/g, ' '); // hello world

'+98 (912) 123-45-67'.replace(/\D/g, ''); // 989121234567
// حذف همه چیز غیر از عدد

'Ali Ahmadi'.replace(/(\w+)\s(\w+)/, 'lastName:$2 | firstName:$1'); // lastName:Ahmadi | firstName:Ali

'2026-05-31'.replace(/(\d{4})-(\d{2})-(\d{2})/, '$3/$2/$1'); // 31/05/2026

'9027290733'.replace(/(\d{3})(\d{3})(\d{4})/, '+98 $1-$2-$3'); // +98 902-729-0733

// Lookahead - Lookbehind
// چیزی را بررسی میکنند ولی جزو متن مچ‌ شده حساب نمیشوند
// Lookahead → بررسی میکند بعد از موقعیت فعلی چه چیزی وجود دارد
// Lookbehind → بررسی میکند قبل از موقعیت فعلی چه چیزی وجود دارد
```
