## ১. PostgreSQL কী?

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

## ২. LIMIT এবং OFFSET ক্লজ কী কাজে লাগে?
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
