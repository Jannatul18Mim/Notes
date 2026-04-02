# Nginx:


**Nginx** (উচ্চারণ: "Engine-X") হলো একটি অত্যন্ত শক্তিশালী এবং ওপেন সোর্স **Web Server**, যা একই সাথে **Reverse Proxy**, **Load Balancer**, এবং **HTTP Cache** হিসেবে কাজ করে। আধুনিক ইন্টারনেটের দ্রুততম ওয়েবসাইটগুলোর একটি বড় অংশ (যেমন: Netflix, Instagram, Pinterest) Nginx ব্যবহার করে।

নিচে Nginx সম্পর্কে বিস্তারিত আলোচনা করা হলো:

---

## ১. Nginx কেন ব্যবহার করবেন? (মূল বৈশিষ্ট্য)
Nginx জনপ্রিয় হওয়ার প্রধান কারণ এর কর্মক্ষমতা। এটি অল্প রিসোর্স ব্যবহার করে হাজার হাজার রিকোয়েস্ট একসাথে সামলাতে পারে।

* **Event-driven Architecture:** Apache যেখানে প্রতিটি ভিজিটর বা কানেকশনের জন্য আলাদা প্রসেস তৈরি করে, Nginx সেখানে একটি সিঙ্গেল প্রসেসের ভেতরেই অসংখ্য কানেকশন হ্যান্ডেল করে।
* **Static Content Handling:** ছবি, CSS, এবং JS ফাইলের মতো স্ট্যাটিক কন্টেন্ট সার্ভ করার ক্ষেত্রে Nginx অন্য যেকোনো সার্ভারের চেয়ে অনেক দ্রুত।
* **Reverse Proxy:** এটি আপনার ব্যাকএন্ড সার্ভারের সামনে থেকে সিকিউরিটি গার্ড হিসেবে কাজ করতে পারে।
* **Load Balancing:** আপনার যদি একাধিক সার্ভার থাকে, তবে Nginx ট্রাফিকগুলোকে সমানভাবে সব সার্ভারে ভাগ করে দিতে পারে।



---

## ২. Nginx-এর কাজের ধরণ (How it Works)
Nginx একটি **Master-Worker** মডেলে কাজ করে:
1.  **Master Process:** এটি কনফিগারেশন ফাইল পড়ে এবং পরবর্তী প্রসেসগুলোকে নিয়ন্ত্রণ করে।
2.  **Worker Processes:** মেইন কাজগুলো এরা করে। প্রতিটি ওয়ার্কার প্রসেস কয়েক হাজার কানেকশন একসাথে প্রসেস করতে সক্ষম।

---

## ৩. প্রধান কনফিগারেশন ফাইলসমূহ
আপনি যদি Nginx নিয়ে কাজ করতে চান, তবে এই ফাইলগুলো সম্পর্কে ধারণা থাকা জরুরি:

* **`/etc/nginx/nginx.conf`:** এটি মূল কনফিগারেশন ফাইল।
* **`/etc/nginx/sites-available/`:** এখানে প্রতিটি আলাদা ওয়েবসাইটের জন্য কনফিগারেশন ফাইল থাকে।
* **`/etc/nginx/sites-enabled/`:** যে সাইটগুলো লাইভ বা সক্রিয় আছে, সেগুলোর লিংক এখানে থাকে।
* **`/var/log/nginx/access.log`:** সাইটে কারা ভিজিট করছে তার লগ।
* **`/var/log/nginx/error.log`:** কোনো সমস্যা হলে তার বিস্তারিত এখানে পাওয়া যায়।

---

## ৪. Nginx বনাম Apache: একটি দ্রুত তুলনা

| ফিচার | Apache | Nginx |
| :--- | :--- | :--- |
| **আর্কিটেকচার** | Process-driven (ভারী) | Event-driven (হালকা) |
| **পারফরম্যান্স** | মাঝারি ট্রাফিকের জন্য ভালো | অত্যন্ত উচ্চ ট্রাফিকের জন্য সেরা |
| **কনফিগারেশন** | `.htaccess` সাপোর্ট করে | `.htaccess` সাপোর্ট করে না |
| **রিসোর্স ব্যবহার** | র‍্যাম খরচ বেশি হয় | খুব কম র‍্যামে চলে |

---

## ৫. প্রাথমিক কিছু কমান্ড (Linux)
Nginx কন্ট্রোল করার জন্য নিচের কমান্ডগুলো সবচেয়ে বেশি ব্যবহৃত হয়:

```bash
sudo systemctl start nginx    # সার্ভিস চালু করতে
sudo systemctl stop nginx     # সার্ভিস বন্ধ করতে
sudo systemctl restart nginx  # রিস্টার্ট করতে
sudo nginx -t                 # কনফিগারেশনে ভুল আছে কি না চেক করতে
sudo systemctl reload nginx   # সার্ভিস বন্ধ না করেই নতুন কনফিগারেশন লোড করতে
```

---

## ৬. Nginx-এর ব্যবহারক্ষেত্র
* **Web Server:** সাধারণ HTML/CSS সাইট হোস্ট করতে।
* **API Gateway:** মাইক্রোসার্ভিস আর্কিটেকচারে API রিকোয়েস্ট ম্যানেজ করতে।
* **SSL Termination:** ওয়েবসাইটের HTTPS সার্টিফিকেট হ্যান্ডেল করতে।
* **Caching:** সার্ভারের লোড কমাতে কন্টেন্ট ক্যাশ করে রাখতে।

**আপনি কি Nginx ইন্সটল করা বা নির্দিষ্ট কোনো কনফিগারেশন (যেমন: Reverse Proxy বা SSL) সম্পর্কে জানতে আগ্রহী?**

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
