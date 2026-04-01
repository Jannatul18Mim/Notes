

# Apache থেকে Nginx মাইগ্রেশন গাইড

Apache থেকে Nginx-এ মাইগ্রেশন করা বর্তমান সময়ের ওয়েব ডেভেলপারদের জন্য খুবই জনপ্রিয় একটি সিদ্ধান্ত। Apache যেখানে **Process-driven architecture** ব্যবহার করে, সেখানে Nginx কাজ করে **Event-driven (asynchronous) architecture**-এ, যা অনেক বেশি ট্রাফিক সামলাতে সক্ষম।

---

## ১. Apache বনাম Nginx: মূল পার্থক্য

মাইগ্রেশনের আগে আপনাকে বুঝতে হবে কেন আপনি এটি করছেন।

| ফিচার | Apache | Nginx |
| :--- | :--- | :--- |
| **আর্কিটেকচার** | প্রতি রিকোয়েস্টের জন্য নতুন থ্রেড/প্রসেস। | একটি সিঙ্গেল থ্রেড অনেক রিকোয়েস্ট হ্যান্ডেল করে। |
| **পারফরম্যান্স** | স্ট্যাটিক কন্টেন্টে ধীর। | স্ট্যাটিক কন্টেন্টে অত্যন্ত দ্রুত। |
| **কনফিগারেশন** | `.htaccess` সাপোর্ট করে। | `.htaccess` সাপোর্ট করে না (সরাসরি মেইন ফাইলে করতে হয়)। |
| **রিসোর্স** | র‍্যাম (RAM) বেশি খরচ হয়। | খুব অল্প র‍্যামে অনেক কাজ করে। |

---

## ২. কনফিগারেশন রূপান্তর (Main Tasks)

Apache-এর অনেক কনসেপ্ট Nginx-এ একটু ভিন্নভাবে লিখতে হয়।

### ডিরেক্টরি ইনডেক্স এবং রুট
Apache-তে আমরা লিখতাম `DocumentRoot`, Nginx-এ সেটা হবে `root`।

**Apache:**
```apache
DocumentRoot "/var/www/html"
DirectoryIndex index.php index.html
```

**Nginx:**
```nginx
root /var/www/html;
index index.php index.html;
```

### Virtual Hosts বনাম Server Blocks
Apache-তে যাকে আমরা **Virtual Host** বলি, Nginx-এ তাকে বলা হয় **Server Block**।

**Nginx Server Block উদাহরণ:**
```nginx
server {
    listen 80;
    server_name example.com [www.example.com](https://www.example.com);

    location / {
        root /var/www/example.com;
        index index.html;
    }
}
```

---

## ৩. .htaccess থেকে Nginx কনফিগারেশন

সবচেয়ে বড় চ্যালেঞ্জ হলো `.htaccess` রুলগুলো পরিবর্তন করা। Nginx ডিরেক্টরি লেভেলে ফাইল রিড করে না, তাই সব রুল `nginx.conf` বা আপনার সাইট স্পেসিফিক ফাইলে লিখতে হবে।

**উদাহরণ (Rewriting URLs):**

* **Apache:**
    ```apache
    RewriteRule ^products/([0-9]+)$ product.php?id=$1 [L]
    ```
* **Nginx:**
    ```nginx
    rewrite ^/products/([0-9]+)$ /product.php?id=$1 last;
    ```

---

## ৪. PHP হ্যান্ডেল করা (PHP-FPM)

Apache সাধারণত `mod_php` ব্যবহার করে সরাসরি PHP ফাইল রান করে। কিন্তু Nginx নিজে PHP প্রসেস করতে পারে না। এর জন্য **PHP-FPM (FastCGI Process Manager)** ব্যবহার করতে হয়।

**Nginx-এ PHP কনফিগারেশন:**
```nginx
location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.2-fpm.sock; # আপনার PHP ভার্সন অনুযায়ী
}
```

---

## ৫. মাইগ্রেশন স্টেপস (Checklist)

- [ ] **Nginx ইন্সটল করুন:** `sudo apt install nginx`
- [ ] **ব্যাকআপ নিন:** Apache-এর সব কনফিগারেশন এবং সাইট ডেটা ব্যাকআপ রাখুন।
- [ ] **কনফিগারেশন কনভার্ট করুন:** অনলাইনে অনেক "Apache to Nginx converter" টুল আছে যা দিয়ে আপনার `.htaccess` রুলগুলো কনভার্ট করে নিতে পারেন।
- [ ] **Apache স্টপ করুন:** `sudo systemctl stop apache2`
- [ ] **Nginx স্টার্ট করুন:** `sudo systemctl start nginx`
- [ ] **টেস্টিং:** ব্রাউজারে সাইটটি চেক করুন এবং দেখুন রিরাইট রুলগুলো ঠিকঠাক কাজ করছে কি না।

---

## কিছু সতর্কতা (Tips)

> **১. Case Sensitivity:** Apache মাঝে মাঝে ফাইল পাথ বা ইউআরএল-এর ক্ষেত্রে নমনীয় হতে পারে, কিন্তু Nginx অত্যন্ত কঠোর। তাই ফাইলের নাম ছোট হাতের (lowercase) রাখা ভালো।
> 
> **২. Security:** Nginx-এ সিকিউরিটি রুলগুলো (যেমন: কোনো ফোল্ডার ব্লক করা) সরাসরি `location` ব্লকের ভেতরে দিতে হয়।
