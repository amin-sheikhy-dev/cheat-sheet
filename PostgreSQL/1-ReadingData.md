```sql
-- SELECT FROM ______________________________________________________________________________

-- ستون نیم از جدول یوزر
SELECT name
FROM users

-- ستون نیم و ایمیل از جدول یوزر
SELECT name,email
FROM users

-- همه ستون های یک جدول
SELECT *
FROM users

-- DISTINCT ______________________________________________________________________________
-- مقادیر تکراری حذف بشن و هر مقدار فقط یک بار در نتیجه نمایش داده بشه
SELECT DISTINCT city
FROM users

-- COUNT ______________________________________________________________________________
-- تعداد رو میگه

SELECT COUNT (city)
FROM users
-- بالایی پایینی فرقی با هم ندارن و پایینی رایج تره
SELECT COUNT (*)
FROM users

SELECT COUNT (DISTINCT city)
FROM users

-- WHERE ______________________________________________________________________________
-- ردیف هارو فیلتر میکنه

SELECT name,city
FROM users
WHERE city = 'Tehran'
-- نتیجه سرچ بالایی و پایینی یکیه
SELECT name
FROM users
WHERE city = 'Tehran'

SELECT *
FROM users
WHERE age > 30 AND city = 'Tehran'

SELECT *
FROM users
WHERE age > 40 OR age < 25

-- علامت <> با =! فرقی نداره
SELECT *
FROM users
WHERE city != 'Tehran'

-- ORDER BY ______________________________________________________________________________

-- بر اساس اعداد
SELECT *
FROM users
ORDER BY age

-- بر اسا حروف الفبا
SELECT *
FROM users
ORDER BY name ASC

SELECT *
FROM users
ORDER BY name DESC

-- اول بر اساس سیتی مرتب می‌کنه
-- بعد اگر چند نفر شهر یکسان داشته باشند بین آنها بر اساس سن مرتب می‌کند
SELECT name, city, age
FROM users
ORDER BY city, age

SELECT name, city, age
FROM users
ORDER BY city ASC, age DESC

-- LIMIT ______________________________________________________________________________

-- فقط 4 تا نتیجه برگردون
SELECT *
FROM users
ORDER BY age
LIMIT 4

-- BETWEEN ______________________________________________________________________________
-- دو طرف را شامل می‌شود
SELECT *
FROM users
WHERE age BETWEEN 25 AND 30

-- اگر بخواهیم بگوییم بین این دو مقدار نباشد
SELECT name, age
FROM users
WHERE age NOT BETWEEN 25 AND 30
ORDER BY age DESC

-- IN ______________________________________________________________________________
SELECT *
FROM users
WHERE age BETWEEN 20 AND 30
AND city IN ('Tehran', 'Shiraz', 'Mashhad')
ORDER BY age DESC
-- بالایی با پایینی یکیه ولی از این استفاده میکنیم که خلاصه تر بشه
SELECT *
FROM users
WHERE age BETWEEN 20 AND 30
AND (city = 'Tehran' OR city = 'Shiraz' OR city = 'Mashhad')
ORDER BY age DESC


SELECT *
FROM users
WHERE age BETWEEN 20 AND 30
AND city NOT IN ('Tehran')
ORDER BY age DESC

-- Aggregate Functions ______________________________________________________________________________

SELECT MIN(price)
FROM products

SELECT MAX(price)
FROM products

SELECT MIN(price), MAX(price)
FROM products

SELECT AVG(price)
FROM products

-- مقدار دومی که این تابع میگیره اینه تا چند رقم رند کنه
SELECT ROUND(AVG(price), 2)
FROM products

SELECT SUM(price)
FROM products

-- GROUP BY ______________________________________________________________________________
-- ردیف هارو بر اساس چیزی که تایین میکنیم داخل یه گروه قرار میده

SELECT city, COUNT(*)
FROM users
GROUP BY city

SELECT city, ROUND(AVG(age), 2)
FROM users
GROUP BY city
ORDER BY AVG(age) DESC

-- HAVING ______________________________________________________________________________
-- برای فیلتر کردن گروه ها استفاده میشه

SELECT city, COUNT(*)
FROM users
GROUP BY city
HAVING COUNT(*) > 70
ORDER BY COUNT(*) DESC

SELECT city, AVG(age)
FROM users
WHERE city NOT IN ('Shiraz', 'Arak', 'Yazd')
GROUP BY city
HAVING AVG(age) > 41
ORDER BY AVG(age) DESC

-- AS ______________________________________________________________________________
-- برای تغییر موقتی اسم هست

SELECT city, ROUND(AVG(age), 2) AS average_age
FROM users
GROUP BY city

SELECT * AS users_table
FROM users AS u

-- INNER JOIN ______________________________________________________________________________
-- دو جدول رو به هم وصل میکنه و فقط رکوردهایی رو برمی‌گردونه که در هر دو جدول تطابق داشته باشن

SELECT *
FROM users
INNER JOIN orders
ON users.id = orders.user_id

SELECT u.email, u.city, o.status
FROM users AS u
INNER JOIN orders AS o
ON u.id = o.user_id
WHERE status IN('pending')

-- FULL OUTER JOIN = FULL JOIN _____________________________________________________________
-- همه‌ی ردیف‌های هر دو جدول را برمی‌گرداند و مقادیر غیر مشترک را نال قرار میدهد

-- همه ردیف هارو بده
SELECT *
FROM users
FULL JOIN orders
ON users.id = orders.user_id

-- فقط قسمت های غیر مشترک رو بده
-- کاربرانی که هیچ سفارشی ندارن یا سفارش‌هایی که به کاربری متصل نیستن
SELECT *
FROM users
FULL JOIN orders
ON users.id = orders.user_id
WHERE users.id IS NULL
OR orders.user_id IS NULL

-- LEFT OUTER JOIN = LEFT JOIN _____________________________________________________________

-- همه کاربران رو بده حتی اگر سفارش نداشته باشند
SELECT u.name, o.id
FROM users AS u
LEFT JOIN orders AS o
ON u.id = o.user_id

-- حالا اینجا میگیم فقط کاربرانی که سفارش ندارن رو بده
SELECT u.name, o.id
FROM users AS u
LEFT JOIN orders AS o
ON u.id = o.user_id
WHERE o.id IS NULL

-- RIGHT OUTER JOIN = RIGHT JOIN _____________________________________________________________

-- همه سفارش‌ها را نگه دار حتی اگه کاربری نداشته باشن
SELECT u.name, o.id
FROM users AS u
RIGHT JOIN orders AS o
ON u.id = o.user_id

-- فقط سفارش هایی که کاربر ندارن
SELECT u.name, o.id
FROM users AS u
RIGHT JOIN orders AS o
ON u.id = o.user_id
WHERE o.user_id IS NULL


-- UNION _____________________________________________________________________________________

-- نتایج را ترکیب کن و تکراری ها را حذف کن
SELECT name, age
FROM users

UNION

SELECT name, age
FROM customers

-- درسته ولی کاربردی نداره
SELECT name
FROM users

UNION

SELECT city
FROM users

-- نکته مهم اینه اطلاعات ستون از نظر نوع و تعداد ستون باید یکسان باشه

-- غلط چون یکی 2 ستونه یکی 1 ستون
SELECT name, age
FROM users

UNION

SELECT name
FROM customers

-- غلط چون یکی عدد یکی استرینگ
SELECT age
FROM users

UNION

SELECT name
FROM customers

-- TIMESTAMP _____________________________________________________________________________________
-- 2026-08-29 15:42:30 مثلا

SELECT NOW() -- 2026-08-29 07:09:38.014588-07

SELECT TIMEOFDAY() -- Sat Aug 29 07:11:43.927110 2026 PDT

SELECT CURRENT_TIME -- 07:13:04.464744-07:00

SELECT CURRENT_DATE -- 2026-08-29

-- استخراج بر اساس سال
SELECT EXTRACT(YEAR FROM created_at) AS created_year
FROM users

-- استخراج بر اساس ماه
SELECT EXTRACT(MONTH FROM created_at)
FROM users


-- استخراج بر اساس روز
SELECT EXTRACT(DAY FROM created_at)
FROM users

-- استخراج بر اساس ساعت
SELECT EXTRACT(HOUR FROM created_at)
FROM users

-- استخراج بر اساس دقیقه
SELECT EXTRACT(MINUTE FROM created_at)
FROM users

-- استخراج بر اساس ثانیه
SELECT EXTRACT(SECOND FROM created_at)
FROM users

-- تعداد کاربران ثبت‌شده در هر ماه چقدر است
SELECT EXTRACT(MONTH FROM created_at), COUNT(*)
FROM users
GROUP BY EXTRACT(MONTH FROM created_at)
ORDER BY EXTRACT(MONTH FROM created_at)

-- 1 year 7 mons 17 days مثلا خروجیش اینه
SELECT AGE(created_at)
FROM users

-- August April November
SELECT TO_CHAR(created_at, 'Month') -- جای ماه هر فرمتی بخای میتونی بزاری
FROM users

-- Mathematical Functions _____________________________________________________________________________

SELECT ABS(-25) -- 25

SELECT CEIL(4.01) -- 5

SELECT FLOOR(4.99) -- 4

SELECT POWER(2, 3) -- 2³ = 8 توان

SELECT SQRT(25) -- 5 جذر

SELECT MOD(10, 3) -- 1 باقی مانده تقسیم

-- LIKE ILIKE ______________________________________________________________________________

-- % هر تعداد کاراکتر حتی صفر کاراکتر

-- هر کسی که اسمش با علی شروع میشه
SELECT *
FROM users
WHERE name LIKE 'Ali%'

-- یعنی هر چیزی که با احمدی تمام شود
SELECT *
FROM users
WHERE name LIKE '%Ahmadi'

-- یعنی احمدی هر جای متن باشد
SELECT *
FROM users
WHERE name LIKE '%Ahmadi%'

-- _ دقیقاً یک کاراکتر
SELECT *
FROM users
WHERE name LIKE 'A_i'

-- است postgres اینه به حروف بزرگ کوچیک حساس نیست و مخصوص ILIKE تنها فرق
SELECT DISTINCT *
FROM users
WHERE name ILIKE 'a%' AND city ILIKE 't%'

-- اگر بخوای بگی این الگو نباشد
SELECT *
FROM users
WHERE name NOT LIKE 'A%'

-- String Functions ______________________________________________________________________________

SELECT LENGTH(NAME)
FROM users

-- ادغام میکنه با هم
SELECT name || ' ' || city AS name_city
FROM users

-- بزرگ کوچیک
SELECT UPPER(name) || ' - ' || LOWER(city)
FROM users

-- دو حرف اولشو بده
SELECT LEFT(name, 2) || ' - ' || city
FROM users

-- SubQuery ______________________________________________________________________________
-- دیگری قرار بدهیم Query را داخل Query یک

SELECT *
FROM users
WHERE age > (
    SELECT AVG(age)
    FROM users
)

SELECT *
FROM users
WHERE age = (
    SELECT MAX(age)
    FROM users
)

SELECT *
FROM (
    SELECT name, age
    FROM users
    WHERE age > 30
) AS older_users

SELECT
    name,
    age,
    (SELECT AVG(age) FROM users) AS average_age
FROM users

-- SELF JOIN ______________________________________________________________________________

SELECT
    u.name AS user_name,
    r.name AS referrer_name
FROM users AS u
INNER JOIN users AS r
ON u.referrer_id = r.id

```
