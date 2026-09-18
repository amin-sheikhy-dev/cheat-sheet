```js
const str = [
  'https://tasteoftheplace.com/kenyan-beef-curry/',
  'http://allrecipes.co.uk/recipe/29578/chicken-katsu-curry.aspx',
  'https://www.bbcgoodfood.com/recipes/11753/nutty-chicken-curry',
];

for (const s of str) {
  const url = new URL(s);

  console.log('URL:', s);
  console.log('Protocol:', url.protocol);
  console.log('Hostname:', url.hostname);
  console.log('Host:', url.host);
  console.log('Port:', url.port);
  console.log('Pathname:', url.pathname);
  console.log('Origin:', url.origin);
}
```

خروجی

URL: https://tasteoftheplace.com/kenyan-beef-curry/
Protocol: https:
Hostname: tasteoftheplace.com
Host: tasteoftheplace.com
Port:
Pathname: /kenyan-beef-curry/
Origin: https://tasteoftheplace.com

---

URL: http://allrecipes.co.uk/recipe/29578/chicken-katsu-curry.aspx
Protocol: http:
Hostname: allrecipes.co.uk
Host: allrecipes.co.uk
Port:
Pathname: /recipe/29578/chicken-katsu-curry.aspx
Origin: http://allrecipes.co.uk

---

URL: https://www.bbcgoodfood.com/recipes/11753/nutty-chicken-curry
Protocol: https:
Hostname: www.bbcgoodfood.com
Host: www.bbcgoodfood.com
Port:
Pathname: /recipes/11753/nutty-chicken-curry
Origin: https://www.bbcgoodfood.com

---
