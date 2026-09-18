اول یه مثال کلی

```js
const text = 'react & next.js';

const encoded = encodeURIComponent(text);
console.log(encoded); // react%20%26%20next.js

const decoded = decodeURIComponent(encoded);
console.log(decoded); // react & next.js
```

### encodeURIComponent()

فرض کن یک سایت خبری داریم و کاربر در Search Box این را وارد می‌کند

مثلا `react & next.js`

```js
const query = 'react & next.js';

const url = `/search?query=${query}`;

console.log(url);
```

خروجی `/search?query=react & next.js`

بعضی کاراکترها در URL معنی خاصی دارند و حالا اگر خود متن کاربر شامل آن ها باشد ممکن است ساختار URL را خراب کند

```js
const query = 'react & next.js';

const encodedQuery = encodeURIComponent(query);

console.log(encodedQuery);
```

خروجی `react%20%26%20next.js`

```js
const url = `/search?query=${encodedQuery}`;

console.log(url);
```

میشود `/search?query=react%20%26%20next.js`

### decodeURIComponent()

مقداری که از URL می‌گیریم ممکن است به شکل encoded باشد پس باید decode کنیم

```js
const encodedQuery = 'react%20%26%20next.js';

const query = decodeURIComponent(encodedQuery);

console.log(query);
```

---

خود URLSearchParams مرورگر encoding را انجام می‌دهد

```js
const params = new URLSearchParams();

params.set('query', 'react & next.js');

console.log(params.toString());
```

```js
params.set('query', encodeURIComponent(query)); // غلط

params.set('query', query); // درست
```
