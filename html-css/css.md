```css
html {
  /* فونت سایز دیفالت صفحه 16 پیکسله ولی من کردمش 10 */
  /* برای استفاده از مقیاس رم هم کاربرد دارد */
  font-size: 10px;
}

/* از ستاره برای انتخاب کل صفحه استفاده میشه */
* {
  /* فونت تمامی متون سایت 30 پیکسل باشه */
  font-size: 3rem;
  /* مارجین همرو صفر میکنیم که خود گوگل به بعضی تگا مارجین نده */
  margin: 20px 10px;
  padding: 0px;

  /* یعنی همه باکسات دقیقا همون وید و هایتی ک میدی باشه */
  box-sizing: border-box;

  /* به این صورته ک اگه اولی اجرا نشد بره سراغ دومی */
  font-family: sans-serif, Verdana;
}
body {
  background-color: #ffffff;
}

/* اگه بخای نوار اسکرول بادی شکل خاصی پیدا کنه */
body::-webkit-scrollbar {
  width: 5px;
  background: transparent;
}

body::-webkit-scrollbar-thumb {
  background: rgba(0, 0, 0, 0.2);
  border-radius: 10px;
}

/*--------------------------------------------------------------------------------- FONT FACE */

@font-face {
  font-family: dana;
  font-style: normal;
  font-weight: 500;
  src:
    url('./test.woff2') format('woff2'),
    url('./test.woff') format('woff');
}

/*--------------------------------------------------------------------------------- SELECTOR */

b {
  background-color: #fafafa;
  box-shadow: 0 0 10px black;
  border-radius: 10px;
  padding: 4px;
}

h1 {
  color: aquamarine;
  background-color: blue;
  width: 500px;
  text-align: center;
}

/* برای انتخاب یک ایدی از # استفاده میکنیم */
#id {
  font-size: 20px;
}

/* برای انتخاب کلس از . استفاده میشه */
/* div هایی که توی تگ img توی این مثال یعنی همه */
div.img {
  color: #d50000;
}

/* برای انتخاب چنتا تگ از کاما استفاده میکنیم */
em,
i {
  color: #f75e06;
  width: 400px;
}

/* فقط دیو های داخل سلکتور انتخاب شدن */
.selector div {
  text-shadow: 0 0 10px #eb0000;
}

/* فقط دیو های اول داخل سلکتور انتخاب شدن یعنی فرزند مستقیشون */
.selector > div {
  color: #00ccff;
}

/* h2 بعد h4 فقط اولین تگ */
h2 + h4 {
  color: tomato;
}

/* h2 بعد از تگ h3 همه تگ های */
h2 ~ h3 {
  color: yellow;
}

/*--------------------------------------------------------------------------------- BACK-GROUND */

/** BACK-GROUND IMG */
.background {
  background-image: url('./files/creeper.jpg');

  /* یعنی صد در صد پرنتشو بگیره */
  background-size: 80%;

  /* یعنی کجای این عرضو ارتفاع قرار بگیره و در محور ایکس ها یا وای های */
  background-position-x: left;

  /* بک گراند ریپیت ینی تصویر بدون تکرار */
  background-repeat: no-repeat;

  border-radius: 20px;
  margin: 30px 0px;
  width: 900px;
  height: 400px;
  border: 2px dashed black;
}

/* background-attachment */
.bck-grd {
  background-attachment: fixed;
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  background-image: url('./files/minecraft.jpg');

  width: 80vw;
  height: 600px;
  box-shadow: var(--box-shadow-blk);
  margin: 20px auto;
  border-radius: 20px;
  border: 4px dashed black;
}

/* background color */
#aidi {
  background-color: rgb(207, 216, 220);

  width: 300px;
  height: 300px;
  border-radius: 50px;
}

/*--------------------------------------------------------------------------------- BORDER */

/*! بردر نان واسه حذف نوار دور اینپوت ها و باتن ها استفاده میشه */
.borders {
  border-style: dashed;
  border-color: blue;
  border-width: 4px;
  border-left-color: rgb(0, 0, 0);
  border-left-width: 10px;
  border-left-style: solid;
  /* برای گوشه گرد کردن بک گراند ها یا باکس ها و خلاصه چیزای زیادی استفاده میشه */
  border-radius: 1px;

  width: 400px;
}

.borders1,
.link {
  width: 500px;
  height: auto;
  border-style: solid;
  border-color: slategrey;
  border-width: 4px;
  margin: 50px 20px;
  border-radius: 30px;
  background-color: #014439;
  color: #4caf50;

  border-left-color: violet;
  border-left-width: 15px;
  border-left-style: double;

  border-right-color: violet;
  border-right-width: 15px;
  border-right-style: double;
}

/* کاری به این یه خط کد نداشته باش */
.hv {
  color: #4caf50;
}

.hv:hover {
  text-shadow: 0 0 10px #76ff03;
  color: black;
}

/*--------------------------------------------------------------------------------- MARGIN */

/* مارجین منفی هم داریم که میره روی نوشته ولی استفاده نکردم */
.margin {
  background-color: #00695c;
  /*! اعداد مارجین به ترتیب عقربه های ساعته اول بالا راست پایین چپ */
  margin: 10px 20px 30px 40px;
}

.margin1 {
  /*! اگه دومیو روی اوتو بزاریم عنصر خود به خود وسط قرار میگیره */
  /*! اولی برای بالا پایین و دومی برای چپو راست */
  margin: 50px 30px;

  background-color: navajowhite;
}

/*--------------------------------------------------------------------------------- PADDING */

/* دقیقا مثل مارجین طول عرض و ارتفاع میگیره فقط فرقش داخل محتواس */
/* اعداد پدینگ هم به ترتیب عقربه های ساعته اول بالا راست پایین چپ */
.margin {
  padding: 40px;
}

.margin1 {
  padding: 10px 20px 30px 40px;
}

.padding {
  padding: 0px 45%;

  margin: 30px 0px;
  background-color: tan;
}

/*--------------------------------------------------------------------------------- WIDTH HEIGHT */

/* واحد های اندازه گیری */
.darsad {
  width: 700px;
  height: 400px;

  background-color: #455a64;
}

/* % درصد */
/*! باید یه پرنتی داشته باشه ک نسبت بهش درصد بگیره */
.darsad1 {
  width: 50%;
  height: 50%;

  background-color: #5d4037;
}

/* rem */
/* یعنی هر یدونش مساوی با پیکسل اچ تی ام ال که دیفالت روی 16 عه ولی من گزاشتم رو 10 */
.rem {
  width: 90rem;
  height: 10rem;
  margin: 4rem 1rem;
  padding: 3rem 45% 0px 45%;

  background-color: #5c6bc0;
}

/* تگ ای ام هم داریم ک عدد میتونی بهش بدی و مثل درصد باید تگ پرنت داشته باشه و نسبت با توجه به رم میدی بهش */
/* ویو پورت یه درصدی بهش میدی با توجه به صفحت اندازه میگیره */
.viewport {
  /* یعنی 50 درصد صفحت چه صفحه کوچیک چ بزرگ */
  width: 50vw;
  height: 15vw;
  font-size: 4vw;
  margin: 30px 0px;

  background-color: #ffab91;
}

/*--------------------------------------------------------------------------------- MIN WIDTH-HEIGHT */

/* بهتر از همش اینه که از تابع کلمپ استفاده کنیم و بهینه تر میشه */
.min {
  /* یعنی طبق صفحه کوچیک شو ولی نه کمتر از 900 پیکسل اگه صفحه کوچیک تر شد اسکرول میخوره */
  min-width: 900px;
  min-height: 200px;
}

/*--------------------------------------------------------------------------------- MAX WIDTH-HEIGHT */

.max {
  /*! یعنی حداکثرت باید صددرصد ویو پورت رو بگیره و عرضت طبق صفحه کوچیک شه */
  max-width: 100vw;
  max-height: 15vw;
}

/*--------------------------------------------------------------------------------- TEXT ALIGN */

/** TEXT TRANSFORM */
.aligncenter {
  /* همه متون وسط قرار بگیرن */
  text-align: center;

  /* همه حروف متن با حروف بزرگ */
  /* توی فارسی کاربرد نداره */
  text-transform: uppercase;

  background-color: #bcaaa4;
  width: 1300px;
  border-radius: 40px;
  padding: 20px 100px;
  text-shadow: 2px 2px 1px white;
}

.alignjustify {
  /* جاستیفای اینجوره که بین کلمات اسپیس میزاره تا منظم شن */
  text-align: justify;

  /* همه متن با حروف کوچیک */
  /* توی فارسی کاربرد نداره */
  text-transform: lowercase;

  background-color: #a5d6a7;
  border-radius: 40px;
  width: 400px;
  padding: 0px 20px;
}

.direction {
  /*! دایرکشن رایت تو لفت بهش میدم مستقل از سایت جهت بگیره */
  direction: rtl;

  /* حرف اول کلمه رو بزرگ میکنه */
  /* توی فارسی کاربرد نداره */
  text-transform: capitalize;

  background-color: #e1bee7;
  border-radius: 40px;
  width: 1400px;
  padding: 0px 20px;
}

/*--------------------------------------------------------------------------------- DECORATION */

/* مثل همون تگ دل عمل میکنه ولی انعطاف پذیریش بیشتره */
/* بیشتر با بردر باتم به متنا خط میدن بخاطر انعطاف پذیری بالاشون */

.D1 {
  /* خط وسط متن */
  text-decoration: line-through;
  margin: 30px;
}

.D2 {
  /* خط بالای متن */
  text-decoration: overline;
  margin: 30px;
}

.D3 {
  /* خط زیر متن */
  text-decoration: underline;
  margin: 30px;
}

.D4 {
  /*! بیشترین استفادش اینه بدی به تگای لینک ک خط زیرشون نره */
  text-decoration: none;
  margin: 30px;
}

/*--------------------------------------------------------------------------------- TEXT SPACING */

.indent {
  /* به اول کلمه فاصله میده */
  text-indent: 50px;

  background-color: #a5d6a7;
  width: 80rem;
  border-radius: 40px;
}

.lineheight {
  /* ارتفاع بین کلمات جمله */
  line-height: 90px;

  background-color: #ffee58;
  width: 80rem;
  border-radius: 40px;
}

.letter {
  /* بین حروف کلمات فاصله میندازه و واسه متون فارسی کاربرد نداره */
  letter-spacing: 7px;

  background-color: #9575cd;
  width: 80rem;
  border-radius: 40px;
}

.world {
  /* بین خود کلمات فاصله میندازه */
  word-spacing: 30px;

  background-color: #f8bbd0;
  width: 80rem;
  border-radius: 40px;
}

.white {
  /* یعنی خط نشکنه و حالت دیفالتش روی رپ یعنی بشکنه هست */
  white-space: nowrap;

  background-color: #80cbc4;
  width: 80rem;
  border-radius: 40px;
}

/*--------------------------------------------------------------------------------- TEXT SHADOW */

.text-shadow {
  /* برای سایه دادن به متن استفاده میشه */
  /* اولی برای ایکس دومی ایگرگ و سومی مقدار بلورش و چهارمی رنگش */
  text-shadow: 2px 2px 1px #ecef50;
}

/*--------------------------------------------------------------------------------- BOX SHADOW */

.box-shadow {
  /* اینست بهش یه حالت تو رفتگی میده */
  box-shadow: 0px 0px 20px #000000 inset;
  width: 900px;
  height: 200px;
  line-height: 200px;
  font-weight: bold;
  margin: 0 auto;
  background-color: white;
  text-align: center;
  border-radius: 30px;
}

/*--------------------------------------------------------------------------------- FOMT-WEIGHT */

/** FONT-STYLE */
.Font-Weight {
  /* مثل همون بولد کردنه فقط دستمون باز تره و خودمون عدد میدیم */
  /* بولد و نرمال هم داره اکثرا تگ های اچ هارو بهشون نرمال میدن */
  font-weight: 900;

  /* مثل همون تگ آی و ای ام هستنش */
  font-style: italic;

  background-color: thistle;
  border-radius: 30px;
  width: 600px;
  margin: 10px auto;
}

/*--------------------------------------------------------------------------------- PSEUDO CLASSES */

/** HOVER */
.Classes:hover {
  /* موس رو ببری روش هاور میشه */
  color: #ff0000;
}

.Classes:active {
  /* وقتی کلیک کنی روش رنگش عوض میشه */
  background-color: yellow;
}

.input {
  /*! وقتی روی اینپوت کلیک میکنیم اوت لاین کالر رنگ عوض میکنه */
  outline-color: #ff0000;
}

/*--------------------------------------------------------------------------------- LIST STYLE */

.list {
  /* میتونیم ایکون رو تغییر بدیم */
  list-style-image: url('./files/icon.png');

  /* لیست استایل پوزیشن اینساید خیلی کاربردیه باعث میشه ایکون نزنه بیرون */
  list-style-position: inside;

  /* این استایل تایپ نان واسه وقتیه ک لازمش نداریم ایکونارو بیشترین استفاده رو هم داره */
  /* list-style: none; */
}

/*--------------------------------------------------------------------------------- DISPLAY */

.block {
  /* به عنصر قابلیت بلاک میدیم که بشه بهش مارجین و پدینگ داد */
  display: block;
}

.inlineblock {
  /* ترکیبی از اینلاین و بلاکه ینی هر عرضی یا طولی میخای میتونی بهش بدی و همچنان هیچیم ندی عرضو و طول خودشو میگیره */
  display: inline-block;
}

.none {
  /* واسه اینه که عنصر کلا حذف میشه از صفحه */
  display: none;
}

/*--------------------------------------------------------------------------------- POSITION */

/* پوزیشن تمام عناصر ها به صورت دیفالت روی استاتیک قرار دارن */

/*? relative */
.box1 {
  /* یعنی از بالا 140 تا فاصله بگیر در کل پوزیشن ینی اینکه بدون توجه به بقیه المنت ها فاصله بگیر و حتی بیا روشون */
  position: relative;
  top: 170px;

  background-color: #fe9900;
  height: 100px;
  width: 70%;
}

.box2 {
  background-color: #8bc34a;
  height: 300px;
  width: 90%;
}

/*? fixed */
.box3 {
  /* پوزیشن فیکس نسبت به ویو پورت اندازه میگیره و توی اون موقعیت میمونه */
  /* تا بهش جهت ندی کجای صفحه بره هیچ جا نمیره */
  position: fixed;
  right: 0;
  top: 0;

  background-color: #9e9e9e;
  border-radius: 10px;
  text-shadow: 2px 1px 3px #fafafa;
  z-index: 9;
  opacity: 0.3;
}

/*? absolute */
.box4 {
  /* باید یه پرنتی داشته باشه که اون پرنت هم پوزیشن رلتیو داشته باشه ک نسبت به اون پوزیشن بگیره مگرنه نسبت به بادی پوزیشن میگیره */
  position: relative;

  background: #9575cd;
  height: 200px;
}

.box5 {
  position: absolute;
  bottom: 0;
  left: 0;

  background-color: #9c27b0;
  height: 50px;
}

/*? sticky */
.box6 {
  /* اگه صفحه اسکرول بخوره یا تکون بخوره اینم باهاش میاد اگه دقت کنی توی هدر های سایت دیدیش */
  position: sticky;
  /* بهش باتم صفر میدی ینی هروقت انقدی صفحه رفت بالا ک باتمش صفر شد اینم باهاش بیاد */
  /* بیشترین کاربرد هم تاپ صفر داره واسه هدر های سایت که همزمان با اسکرول کردن صفحه اینم بیاد پایین */
  bottom: 0;

  background-color: #80cbc4;
  display: inline-block;
  opacity: 0.3;
  z-index: 9;
}

/*--------------------------------------------------------------------------------- Z INDEX */

/* باید از فلوی نرمال خارج شده باشن تا بشه بهشون زد ایندکس داد و ترتیب روی هم قرار گرفتنو بهشون بدیم */
/* وقتی بهشون منفی بدیم میره زیر تمامی عناصر قرار میگیره */
/* به هدر های سایت معمولا بیشترین زد ایندکس رو میدن که روی هر عنصری قرار بگیرن */
.z {
  position: relative;
  color: #ffffff;
  margin: 0;
  background-color: rgb(11, 11, 179);
  height: 200px;
  border: 10px solid #5c6bc0;
}

.z2 {
  margin: 0;
  background-color: #81c2f7;
  width: 200px;
  height: 50px;
  position: absolute;
  left: 500px;
  text-align: center;
  border: 6px solid #1565c0;
  /* الان به صورت پیش فرض عدد 2 باید زیر 3 بره چون ایندکس کوچک تری داره */
  z-index: 4;
}

.z3 {
  margin: 0;
  background-color: #4db6ac;
  width: 200px;
  height: 50px;
  position: absolute;
  right: 700px;
  text-align: center;
  border: 6px solid #00695c;

  z-index: 3;
}

/*--------------------------------------------------------------------------------- OVERFLOW */

/** SCROLL */
/* اگر سر ریز بشه اتفاقاتی میفته */
/* همه عناصر دیفالت روی ویزیبل هستن */
/* هیدن یعنی اون اور شده رو کلا مخفی کن */
/* اور فلو ممکنه واسه باکس هام باشه مثلا اگه یه باکسی طولش یا عرضش از پرنتش بیشتر باشه اورفلو میشه */
.scroll {
  /* در هر صورت اسکرول داره */
  /* میتونی بهش بدی اسکرول افقی بخوره یا اسکرول عمودی */
  overflow: scroll;

  height: 100px;
  background-color: #a1887f;
}
.auto {
  /* اوتو یعنی اگه سر ریز شد اسکرول بخور */
  overflow: auto;

  background-color: #757575;
  height: 100px;

  /* میتونی  بنویسی اگه عمودی یا افقی سر ریز شد چه اتفاقی بیفته */
  /* overflow-x: auto; */
  /* overflow-y: auto; */
}

/*! اگه بخام اسکرول یک محتوا مخفی شه */

/* 
.container{
  overflow: auto;
  scrollbar-width: none;
}
  
.container::-webkit-scrollbar {
  display: none;
} 
*/

/*--------------------------------------------------------------------------------- FLEX BOX */

/* فلکس همیشه برای پرنت تگ ها استفاده میشه */
.container {
  display: flex;

  /* واسه اینه عناصر داخل پرنت چقد از هم فاصله داشته باشن */
  gap: 100px;

  /* حالت دیفالت روی نو رپ هستش و فلکس اجازه نمیده ایتما عرض بیشتری از اون چیز بگیرن */
  /* اگه روی رپ بزاریم میزاره عرض ایتما بیشتر بشه و بیان خط بعدی */
  /* flex-wrap: wrap; */

  /* باعث میشه عناصر کاملا منظم در گوشه های صفحه در محور ایکس ها */
  justify-content: space-between;

  /* باعث میشه عناصر کاملا منظم کنار هم قرار بگیرن در راستای ایگرگ ها */
  align-items: center;

  width: 1200px;
  height: 400px;
  margin-right: auto;
  margin-left: auto;
  background-color: #5e35b1;
  border: 10px solid #1a237e;
  border-radius: 50px;
}
.flex-item {
  background-color: #9fa8da;
  border: 7px solid #d1c4e9;
  text-align: center;
  line-height: 200px;

  /* بهشون وید 100 دادم ایتما منظم قرار بگیرن */
  /* width: 100%; */

  /* عدد 1200 کانتینرمو تقسیم 3 کردم ک هر ایتم باید 400 تا بگیره */
  /* width: 400px; */

  width: 200px;
  height: 200px;
  border-radius: 30px;
}

/*--------------------------------------------------------------------------------- PSEUDO ClASSES */

.ckck:checked + .on {
  /* checked */
  /* یعنی وقتی چک باکس فعال شد یه اتفاقی بیفته */
  display: block;
}
.on {
  display: none;
  font-size: 20px;
  font-weight: bold;
  font-style: italic;
  text-shadow: 2px 2px 6px #ffeb3b;
}
.b {
  display: flex;
  border: 5px solid #004d40;
  background-color: #b2dfdb;
  height: 64px;
  width: 200px;
  align-items: center;
  border-radius: 20px;
}

/* first-last-nth child */
.itemm:first-child {
  /* اولین بچه */
  color: #4caf50;
}

.itemm:last-child {
  /* اخرین بچه */
  color: #f4511e;
}
.itemm:nth-child(3) {
  /* خودت انتخاب میکنی */
  /* even-odd ایون ینی ایتمای زوج و اود ینی ایتمای فرد */

  color: #1e88e5;

  /* .div:empty یعنی تمامی دیو هایی که فرزند ندارن رو انتخاب کن */
  /* .div:not(amin) ینی همه ایتما بجز این که کاربردش نفهمیدم */
}

/*--------------------------------------------------------------------------------- AFTER BEFORE */

.af-bf::after {
  /* after */
  /* یه تکستی رو بعدش میزنی */
  content: ' khodafez bache ha ';
  color: #06b396;
}
.af-bf::before {
  /* before */
  /* یه تکستی رو قبلش میزنی */
  content: '';
  background-color: #1e88e5;
  width: 30px;
  height: 30px;
  border-radius: 100%;
  position: absolute;
  /* left: 10px; */
}

/* سلکشن یعنی یه عنصر رو هایلایت و خاستی انتخابش کنی */
.itemm::selection {
  background-color: #4caf50;
}
h5::selection {
  /* selection */
  /* سلکشن معمولا با تم وبسایت انجام میشه */
  /* فقط تگ های اچ5 من سلکت شدن فلان شه */
  background-color: #4caf50;
}
::selection {
  /* همه عناصر صفحه وقتی سلکت شدن */
  background-color: #f5efb6;
}

/*--------------------------------------------------------------------------------- OPACITY */

.opa-pa {
  /* کل محتوا رو کمرنگ میکنه */
  opacity: 0.8;

  background-image: url('./files/minecraft.jpg');
  width: 861px;
  height: 404px;
  margin: 0 auto;
  border-radius: 30px;
  padding: 100px 10%;
  font-size: 30px;
  color: #d500f9;
  text-shadow: 2px 2px 3px #00e5ff;
}

/*--------------------------------------------------------------------------------- ATTRIBUTE SELECTOR */

[href] {
  /*! تمامی تگ هایی که اتریبیوت اچرف دارن خط زیرشون نباشه */
  text-decoration: none;
}

[class='at-se'] {
  /* تمامی تگ هایی ک اتریبیوت کلسشون مساوی با اینه فلان شن */
  background: #cfd8dc;
}

/*

[href^="https://"] {
  تگ های اچرفی که اولشون با اچ تی تی پی اس بود سلکت شن
}

[href$=".ir"] {
  تگ های اچرفی که اخرشون با دات ای ار بود سلکت شن
} 

*/

/*--------------------------------------------------------------------------------- MATH FUNCTION */

.math1 {
  background-color: #1a237e;
  width: 1300px;
  height: 200px;
}

.math2 {
  /*! calc دقیقا مثل محاسبات ریاضیه */
  width: calc(100% - 200px);

  background-color: #1e88e5;
  height: 200px;
  margin: 0;
  line-height: 200px;
  text-align: center;
}

.clamp {
  /*! اولی مینه دومی نرمالش سومی ماکسش */
  width: clamp(350px, 50vw, 600px);
  /* با این پایینی فرقی نداره */
  /* min-width: 200px;
    width: 50%;
    min-width: 600px; */

  height: 200px;
  background-color: #827717;
  text-align: center;
  padding-top: 30px;
}

/*--------------------------------------------------------------------------------- BACKGROUND SIZE */

.bg-s {
  background-image: url('./files/creeper.jpg');
  background-repeat: no-repeat;

  /*! یعنی صد در صد عرض پرنتشو بگیره */
  /* background-size: contain; */

  /*! یعنی تصویرو هرجور شده پر کن */
  background-size: cover;

  /*! به بی جی سایز ربط داره و میگه اونجاییش که حذف میشه کجاش زوم شه */
  background-position: center;
  background-size: cover;

  width: 700px;
  height: 500px;
  border: 4px solid black;
  border-radius: 20px;

  /* یعنی شفاف و شیشه ای که بک گراند پرنتشو بگیره */
  /* background: transparent; */
}

/*--------------------------------------------------------------------------------- CURSOR */

.crs {
  /* مدل های مختلفی داره و این روی حالت دسته */
  cursor: pointer;
}

/*--------------------------------------------------------------------------------- GRADIENT */

.grd {
  /* از چپ به راست و از چپ به راست متمایل به پایین */
  /* to right, to right bottom */
  background-image: linear-gradient(to right bottom, #d500f9, #2962ff);

  /* درجه هم میشه داد */
  /* background-image: linear-gradient(45deg, #D500F9, #2962FF); */

  /* به صورت پیشفرض اگه بهش جهت ندی از بالا به پایین ساخته میشه */
  /* background-image: linear-gradient(#D500F9, #2962FF); */

  width: 800px;
  height: 200px;
  margin: 0 auto;
  border: 5px solid black;
  border-radius: 20px;
  text-align: center;
  padding: 80px 0;
}

.gr-im {
  /* ترکیب گردینت و ایمیج */
  background-image: linear-gradient(rgba(56, 11, 11, 0), rgba(255, 255, 255, 0.6)), url('./files/farsh.jpg');
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;

  width: 800px;
  height: 500px;
  margin: 0 auto;
  color: white;
  border: 5px solid black;
  border-radius: 20px;
  text-align: center;
  padding: 220px 0;
}
/* وقتی عکس هاور بشه چی میشه */
.parent-gr-im-hv {
  width: 800px;
  height: 450px;
  display: block;
  margin: 0 auto;
}
.gr-im-hv {
  background-image: url('./files/farsh.jpg');

  width: 800px;
  height: 450px;
  color: white;
  text-align: center;
  padding: 200px 0;
  transition: all 500ms;
}
.gr-im-hv:hover {
  background-image: linear-gradient(rgba(33, 33, 33, 0), rgba(250, 250, 250, 0.5)), url('./files/farsh.jpg');
  box-shadow: 0px 0px 20px #000000;
}

/*--------------------------------------------------------------------------------- TEXT EFFECCT */

.txt-ef {
  /* خط نشکنه */
  white-space: nowrap;
  overflow: hidden;
  /* ادامش سه نقطه بخوره */
  text-overflow: ellipsis;
}

.wrt_mod {
  /* متنو عمودی مینویسه */
  writing-mode: vertical-lr;
}

/*--------------------------------------------------------------------------------- TRANSITION */

/* ترنسیشن همیشه باید به قسمت اصلی المان بدیم نه به قسمت هاور چون وقتی که موسو برمیداریم دیگه ترنسیشن اجرا نمیشه پس حتما باید قسمت اصلی تگ بدیم */
/* واسه اینه پراپرتی به نرمی تغییر کنه مثلا هاور کردن که مقادیری ک میگیره رو زیر نوشتم */
/* property واسه چه پراپرتی اعمال شه و حالت آل هم داره- duration زمان افکت- timing function نوع افکت- delay چقد طول بکشه اعمال شه */
.trns {
  /* همه چی تغییر کنه توی 500میلی ثانیه */
  /* و 100 میلی ثانیه دیلی داشته باشه باعث میشه اگه کاربر هدفش اون نبوده باشه الکی هاور نشه */
  transition: all 500ms ease-in 100ms;
  /*
! transition: property , duration , timing function , delay
  */

  background-color: #1a237e;
  width: 900px;
  height: 200px;
  margin: 0 auto;
  color: white;
  text-align: center;
}

.trns:hover {
  background-color: #3ccdf1;
  color: black;
}

/* کصکلک */
.trns1 {
  display: flex;
  justify-content: space-between;
}
.trns-child {
  transition: all 500ms;
  background-image: linear-gradient(#455a64, #cfd8dc);
  width: 200px;
  height: 200px;
  margin: 60px auto;
  border-radius: 40px;
  text-align: center;
  line-height: 200px;

  border-top-left-radius: 0px;
  border-bottom-right-radius: 0px;
}
.trns-child:hover {
  box-shadow: 0px 0px 10px rgb(37, 37, 37);
  text-shadow: 1px 1px 10px white;
  transform: scale(1.05);
}

/*--------------------------------------------------------------------------------- TRASFORM */

/* باعث تغییر میشه مثلا میچرخه یا گنده میشه */
.p-frm {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.tr-frm1 {
  transition: all 500ms;
  background-color: #d1c4e9;
  width: 200px;
  height: 200px;
  margin: 0 auto;
  text-align: center;
  border-radius: 30px;
  line-height: 200px;
}
.tr-frm1:hover {
  /*! میچرخه */
  transform: rotate(45deg);
}
.tr-frm2 {
  transition: all 500ms;
  background-color: #ffccbc;
  width: 200px;
  height: 200px;
  margin: 0 auto;
  text-align: center;
  border-radius: 30px;
  line-height: 200px;
}
.tr-frm2:hover {
  /*! گنده یا کوچیک میشه */
  transform: scale(0.8);
}
.tr-frm3 {
  transition: all 500ms;
  background-color: #b2dfdb;
  width: 200px;
  height: 200px;
  margin: 0 auto;
  text-align: center;
  border-radius: 30px;
  line-height: 200px;
}
.tr-frm3:hover {
  /*! فلان درجه بچرخون که میشه بهش محور ایکس یا ایگرگ هم داد */
  transform: skew(10deg);
}
.tr-frm4 {
  transition: all 500ms;
  background-color: #fff9c4;
  width: 200px;
  height: 200px;
  margin: 0 auto;
  text-align: center;
  border-radius: 30px;
  line-height: 200px;
}

.tr-frm4:hover {
  /*! با توجه به عددی که میدیم به محور ایکس یا ایگرگ تغییر میکنه */
  transform: translateX(30px);
}

/*--------------------------------------------------------------------------------- OBJECT FIT */

/* مختص تگ های ایمیج و ویدیو */
.obj1 {
  width: 50vw;
  height: 350px;
  margin: 10px;
  padding: 0;
  box-shadow: 0 0 10px #000000;
  border-radius: 20px;
}

.obj-cnt {
  object-fit: contain;

  /* ماجین بدی که دیفالت از عناصر فاصله نگیره */
  margin: 0;
  /* یادت باشه اول بگی صد در صد عرض و ارتفاع پرنتشو بگیره */
  width: 100%;
  height: 100%;
  border-radius: 20px;
}

.obj-cvr {
  object-fit: cover;

  /* ماجین بدی که دیفالت از عناصر فاصله نگیره */
  margin: 0;
  /* یادت باشه اول بگی صد در صد عرض و ارتفاع پرنتشو بگیره */
  width: 100%;
  height: 100%;
  border-radius: 20px;
}

.obj,
.b-obj {
  display: flex;
  margin: 0;
  justify-content: space-between;
}

/*--------------------------------------------------------------------------------- VARIABLE */

/* اینارو باید اول سایت بزاری */
/* تقریبا میشه گفت یه عنصریه که خودمون دستی میسازیمش */
/* دو نوع لوکال و گلوبال داره که لوکال رو توی خود تگ میدی و همونجام استفاده میکنی ولی گلوبال توی روت میدیش و هرجایی میتونی استفادش کنی */
:root {
  /* root */
  /* دقیقا ینی به خود سند اصلی اچ تی ام ال اشاره میکنیم و ارجعیت بیشتری داره */
  --color-red: #b71c1c;
  --shadow-blk: 0 0 10px black;
  --shadow-red: 0 0 10px #d50000;
  --width: 900px;
  --height: 200px;
  --margin-center: 0 auto;
  --margin-30-center: 30px auto;
  --border-radius: 20px;
}

.vrbl {
  background-color: var(--color-red);
  box-shadow: var(--shadow-red);
  width: var(--width);
  height: var(--height);
  margin: var(--margin-center);
  border-radius: var(--border-radius);
}

/*--------------------------------------------------------------------------------- MEDIA QUERY */

/*! واسه اینه مثلا تو هر سایز صفحه چجور کدا اجرا شن و واسه این خوبه که سایتو واسه موبایل یا غیره تنظیمش کنی */
/* واسه ماکسیمم وید به ترتیب این شکلیه */
/* max-width 1300 */
/* max-width 900 */
/* max-width 768 */
/* max-width 400 */
/* واسه مینیمم وید برعکس ماکسیممه */
/* min-width 400 */
/* min-width 768 */
/* min-width 900 */
/* min-width 1300 */

.max-media {
  background-color: #06b396;
  max-width: 800px;
  height: 200px;
  margin: var(--margin-center);
  border-radius: 100%;
}

@media screen and (max-width: 350px) {
  .max-media {
    background-color: #014439;
    border-radius: 100%;
    border-top-left-radius: 0px;
    border-bottom-right-radius: 0px;
  }
}

/*--------------------------------------------------------------------------------- ANIMATION */

.anm {
  /* اسم انیمیشن */
  animation-name: loop;

  /* مدت زمانش تا پایان */
  animation-duration: 2s;

  /* تعداد پخش ان */
  animation-iteration-count: infinite;

  /* کاربر چقد وایسه تا پخش شه */
  animation-delay: 100ms;

  /* نوع پخش شدنش مث ترنسیشن */
  animation-timing-function: linear;

  /* shorthand */
  /*! animation: loop 3s infinite 100ms ease-in */
  /*! animation: name duration count delay function */

  width: 100px;
  height: 100px;
  background-color: #6200ea;
  border-radius: 100%;
}

@keyframes loop {
  /* شروع انیمیشن از 0 درصد هستش */
  0% {
    background-color: #64dd17;

    transform: translateX(0vw);
  }
  25% {
    background-color: #304ffe;

    transform: translateX(22vw);
  }
  50% {
    background-color: #ffff00;

    transform: translateX(44vw);
  }
  75% {
    background-color: #d50000;

    transform: translateX(66vw);
  }

  /* پایان انیمیشن از 100 درصد هستش */
  100% {
    background-color: #1de9b6;

    transform: translateX(90vw);
  }
}

/*--------------------------------------------------------------------------------- BLUR */

.kossher {
  /*! این دقیقا خود عنصر رو بلور میکنه بدون اینکه به بچه هاش دست بزنه */
  /* مثلا به بادی بدی بک گراند بادی بلور میشه ولی به عناصر روی ان دست نمیخوره */
  backdrop-filter: blur(8px);

  /*! این به خودش دست نمیزنه ولی بچه هاشو بلور میکنه */
  /* یعنی به بادی بدی عناصر بادی همشون بلور میشن */
  filter: blur(2px);
}
```
