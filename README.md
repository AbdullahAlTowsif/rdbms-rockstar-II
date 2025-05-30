## ১. What is PostgreSQL? | PostgreSQL কী?

**PostgreSQL** একটি শক্তিশালী, ওপেন সোর্স **রিলেশনাল ডাটাবেজ ম্যানেজমেন্ট সিস্টেম (RDBMS)**, যা SQL (Structured Query Language) ব্যবহার করে ডাটা ম্যানিপুলেশন এবং ম্যানেজমেন্ট করে। এটি ACID (Atomicity, Consistency, Isolation, Durability) কমপ্লায়েন্ট, অর্থাৎ ডাটা ইন্টিগ্রিটি এবং ট্রানজেকশন সেফটি নিশ্চিত করে।

**PostgreSQL-এর বৈশিষ্ট্য:**
- এটি ACID (Atomicity, Consistency, Isolation, Durability) কমপ্লায়েন্ট, অর্থাৎ এটি ট্রান্স্যাকশনের নিরাপত্তা নিশ্চিত করে।
- এতে Stored Procedures, Triggers, Views, এবং Custom Data Types ব্যবহার করা যায়।
- JSON, XML, এবং hstore এর মতো নন-রিলেশনাল ডেটা টাইপও সমর্থন করে।
- এটি concurrency management এর জন্য **MVCC (Multi-Version Concurrency Control)** ব্যবহার করে, যা একাধিক ইউজারকে একসাথে কাজ করার সুবিধা দেয়।

📌 **উদাহরণ:**  
ধরা যাক আমাদের একটি `students` টেবিল আছে:

```
-- একটি টেবিল তৈরি করা
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    department TEXT,
    cgpa NUMERIC
);
```

## ২. What are the LIMIT and OFFSET clauses used for? | LIMIT এবং OFFSET ক্লজ কী কাজে লাগে?
**LIMIT** ক্লজ ব্যবহার করে কোয়েরির ফলাফলের সীমিত সংখ্যক রো (row) ফেরত নেওয়া যায়। এটি পেজিনেশনে (Pagination) ব্যবহৃত হয়।
**সিনট্যাক্স:**
```
SELECT * FROM table_name LIMIT number;
```
**উদাহরণ:**
```
-- প্রথম ৫টি স্টুডেন্ট ডাটা দেখাবে
SELECT * FROM students LIMIT 5;
```
**OFFSET** ক্লজ নির্দিষ্ট সংখ্যক রো স্কিপ করে পরের রো গুলো দেখায়। এটি LIMIT-এর সাথে ব্যবহার করে পেজিনেশন ইমপ্লিমেন্ট করা যায়।
**সিনট্যাক্স:**
```
SELECT * FROM table_name LIMIT number OFFSET number;
```
**উদাহরণ:**
```
-- প্রথম ৫ রো স্কিপ করে পরের ৫টি রো দেখাবে (পেজ ২-এর ডাটা)
SELECT * FROM students LIMIT 5 OFFSET 5;
```

## ৩. What is the significance of the JOIN operation, and how does it work in PostgreSQL? | JOIN অপারেশন কী এবং এটি PostgreSQL-এ কীভাবে কাজ করে?
**JOIN** হল এমন একটি অপারেশন যা দুটি বা ততোধিক টেবিলকে একটি নির্দিষ্ট সম্পর্ক (common field) এর মাধ্যমে সংযুক্ত করে ডেটা রিটার্ন করে।

**JOIN-এর ধরণসমূহ:**
- **INNER JOIN**: শুধু সেই রেকর্ডগুলো দেখায় যেগুলোর মধ্যে মিল পাওয়া যায়।
- **LEFT JOIN**: বাম টেবিলের সব রেকর্ড ও ডান টেবিলের মিল থাকা রেকর্ড।
- **RIGHT JOIN**: ডান টেবিলের সব রেকর্ড ও বাম টেবিলের মিল থাকা রেকর্ড।
- **FULL OUTER JOIN**: উভয় টেবিলের সব রেকর্ড, মিল থাকুক বা না থাকুক।

**উদাহরণ:**
ধরা যাক আমাদের নিচের দুটি টেবিল আছে:
# 🟩 **students** টেবিল:
```
| id | name  | dept_id  |
| -- | ----- | -------- |
| 1  | Rahim | 101      |
| 2  | Karim | 102      |
| 3  | Sumon | 103      |
| 4  | Jamil | NULL     |

```
# 🟩 **departments** টেবিল:
```
| id  | department_name  |
| --- | ---------------- |
| 101 | CSE              |
| 102 | EEE              |
| 104 | BBA              |
```

# INNER JOIN
**INNER JOIN** শুধু সেই রেকর্ডগুলোই রিটার্ন করে যেগুলোর মধ্যে উভয় টেবিলে মিল আছে।
```
SELECT students.name, departments.department_name
FROM students
INNER JOIN departments
ON students.dept_id = departments.id;
```
# ফলাফল:
```
| name  | department_name  |
| ----- | ---------------- |
| Rahim | CSE              |
| Karim | EEE              |
```
📝 ব্যাখ্যা: শুধুমাত্র dept_id = 101 এবং 102-এর ক্ষেত্রে দুটি টেবিলে মিল পাওয়া গেছে। তাই অন্য রেকর্ডগুলো বাদ গেছে।


# LEFT JOIN / LEFT OUTER JOIN
**LEFT JOIN** বাম পাশে থাকা টেবিলের (প্রথম টেবিলের) সব রেকর্ড ফেরত দেয়, এমনকি যদি ডান পাশে থাকা টেবিলে (দ্বিতীয় টেবিল) মিল না থাকে তবুও।

```
SELECT students.name, departments.department_name
FROM students
LEFT JOIN departments
ON students.dept_id = departments.id;

```

# ফলাফল:
```
| name  | department_name  |
| ----- | ---------------- |
| Rahim | CSE              |
| Karim | EEE              |
| Sumon | NULL             |
| Jamil | NULL             |
```
📝 ব্যাখ্যা: Sumon-এর dept_id (103) এবং Jamil-এর dept_id নেই, তাই তাদের ক্ষেত্রে department_name NULL হয়েছে, কিন্তু তার পরেও তারা ফলাফলে এসেছে।


# RIGHT JOIN / RIGHT OUTER JOIN
**RIGHT JOIN** ডান পাশে থাকা টেবিলের সব রেকর্ড ফেরত দেয়, এমনকি যদি বাম পাশে মিল না থাকে।

```
SELECT students.name, departments.department_name
FROM students
RIGHT JOIN departments
ON students.dept_id = departments.id;
```

# ফলাফল:
```
| name  | department_name  |
| ----- | ---------------- |
| Rahim | CSE              |
| Karim | EEE              |
| NULL  | BBA              |
```
📝 ব্যাখ্যা: departments টেবিলের id = 104 (BBA) এর সাথে students টেবিলে কোনো মিল নেই, তাই নাম NULL হয়েছে, কিন্তু BBA তবুও রিটার্ন হয়েছে।


# FULL OUTER JOIN
**FULL OUTER JOIN** উভয় টেবিলের সব রেকর্ড ফেরত দেয়, মিল থাকুক বা না থাকুক। মিল না থাকলে NULL দেখায়।

```
SELECT students.name, departments.department_name
FROM students
FULL OUTER JOIN departments
ON students.dept_id = departments.id;
```

# ফলাফল:
```
| name  | department_name  |
| ----- | ---------------- |
| Rahim | CSE              |
| Karim | EEE              |
| Sumon | NULL             |
| Jamil | NULL             |
| NULL  | BBA              |
```
📝 ব্যাখ্যা: এটি LEFT JOIN এবং RIGHT JOIN এর সংমিশ্রণ। উভয় টেবিলের সব রেকর্ড দেখায়, এবং যেখানে মিল নেই, সেখানে NULL দেখায়।

## Summary
```
| JOIN টাইপ         | কী করে                                            |
| ----------------- | -------------------------------------------------- |
| `INNER JOIN`      | শুধু মিল পাওয়া রেকর্ড দেখায়।                          |
| `LEFT JOIN`       | বাম টেবিলের সব রেকর্ড, ডান পাশে মিল না থাকলে NULL   |
| `RIGHT JOIN`      | ডান টেবিলের সব রেকর্ড, বাম পাশে মিল না থাকলে NULL   |
| `FULL OUTER JOIN` | উভয় টেবিলের সব রেকর্ড, মিল না থাকলে NULL দেখায়।    |
```

## ৪. Explain the GROUP BY clause and its role in aggregation operations. | GROUP BY ক্লজ কী এবং এটি অ্যাগ্রিগেশন অপারেশনে কীভাবে কাজ করে?
**GROUP BY** ক্লজ ব্যবহার করা হয় তখন, যখন আমরা ডেটাকে কোনো নির্দিষ্ট কলামের ভিত্তিতে ভাগ করে প্রতিটি গ্রুপের উপর অ্যাগ্রিগেট ফাংশন (যেমন COUNT, SUM, AVG) প্রয়োগ করতে চাই।

**উদাহরণ:**
ধরি আমাদের **sales** টেবিল আছে যেখানে **product_id** ও **quantity_sold** ফিল্ড রয়েছে। এখন প্রতিটি পণ্যের মোট বিক্রয় দেখতে চাই:
```
SELECT product_id, SUM(quantity_sold)
FROM sales
GROUP BY product_id;
```
এই কুয়েরিটি প্রতিটি পণ্যের জন্য মোট বিক্রয় দেখাবে।
**কার্যকারিতা:** GROUP BY ক্লজ ছাড়া আমরা অ্যাগ্রিগেট ফলাফল বিশ্লেষণ করতে পারি না, যেমন— প্রতিটি বিভাগের গড় সিজিপিএ, প্রতিটি শহরের মোট গ্রাহক ইত্যাদি।


## ৫. How can you calculate aggregate functions like COUNT(), SUM(), and AVG() in PostgreSQL? | PostgreSQL-এ অ্যাগ্রিগেট ফাংশন (COUNT(), SUM(), AVG()) কিভাবে কাজ করে?
**PostgreSQL** অ্যাগ্রিগেট ফাংশন ব্যবহারের মাধ্যমে আমরা নির্দিষ্ট কলামের উপর ভিত্তি করে গণনামূলক বিশ্লেষণ করতে পারি।

**COUNT():**
ডেটার সংখ্যা গণনা করে।
**উদাহরণ:**
```
SELECT COUNT(*) FROM students;
```
এটি **students** টেবিলে মোট কতজন শিক্ষার্থী আছেন তা দেখাবে।

**SUM():**
নির্দিষ্ট কলামের মোট যোগফল প্রদান করে।
**উদাহরণ:**
```
SELECT SUM(salary) FROM employees;
```
এই কুয়েরিটি **employees** টেবিলে থাকা সকল কর্মচারীর বেতন যোগফল দেখাবে।

**AVG():**
গড় মান নির্ণয় করে।
**উদাহরণ:**
```
SELECT AVG(cgpa) FROM students;
```
এই কুয়েরিটি সকল শিক্ষার্থীর গড় CGPA দেখাবে।