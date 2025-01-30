
---

###  1.ipynb
# 📄 تحلیل و پردازش فایل PDF با پایتون

این پروژه شامل کدی برای **تحلیل و پردازش فایل‌های PDF** است. این کد قابلیت **جستجوی کاراکترهای مشخص‌شده** در متن PDF را دارد، تعداد وقوع آن‌ها را محاسبه می‌کند و همراه با اطلاعات دیگر نمایش می‌دهد.

---

## 🚀 ویژگی‌های پروژه

✅ خواندن فایل PDF و استخراج متن  
✅ جستجوی حروف مشخص‌شده در متن  
✅ شمارش تعداد دفعات وقوع حروف  
✅ بارگذاری داده‌ها از فایل JSON  
✅ چاپ مقادیر احتمالاتی و آمار حروف  

---

## 🛠️ **پیش‌نیازها**
قبل از اجرای کد، باید کتابخانه‌های مورد نیاز را نصب کنید. برای این کار، دستور زیر را اجرا کنید:

```bash
pip install PyPDF2
```

---

## 📝 **مراحل اجرای برنامه**

### **1️⃣ خواندن فایل PDF**
```python
import PyPDF2

object = PyPDF2.PdfFileReader("Handbook of Data Compression, 5th Edition.pdf")
NumPages = object.getNumPages()
```
📌 این قسمت، **فایل PDF** را باز کرده و تعداد صفحات آن را مشخص می‌کند.  

---

### **2️⃣ تعیین کاراکترهای مورد جستجو**
```python
String = ["a", "b", "c", "d"]
```
📌 این لیست شامل حروفی است که **برنامه در متن PDF جستجو خواهد کرد.**  

---

### **3️⃣ بارگذاری داده‌ها از JSON**
```python
import json

with open('mydic.json') as f:
    mydic = json.load(f)

with open('mydict.json') as f:
    mydict = json.load(f)
```
📌 داده‌های مورد نیاز از **فایل‌های JSON** بارگذاری می‌شوند.  

---

### **4️⃣ جستجوی حروف در PDF**
```python
import re

sc = 0
for String in String:
    for i in range(0, NumPages):
        PageObj = object.getPage(i)
        Text = PageObj.extractText()

        if re.search(String, Text):
            sc = sc + 1
```
📌 **نحوه کار:**  
✔️ حلقه روی **لیست حروف** و **صفحات PDF** اجرا می‌شود.  
✔️ متن هر صفحه استخراج شده و بررسی می‌شود که آیا **کاراکتر مورد نظر در آن هست یا نه**.  
✔️ در صورت یافتن **کاراکتر، شمارنده `sc` یک واحد افزایش می‌یابد**.  

---

### **5️⃣ نمایش نتایج نهایی**
```python
print("مجموع تکرار کاراکترها:", sc)
print("مقادیر دیکشنری:", mydic)
print("مقادیر احتمالاتی:", mydict)
```
📌 این قسمت، **نتایج نهایی را در خروجی نمایش می‌دهد.**  

---

## 📂 **ساختار فایل‌ها**
📂 **DNA-Encoding-Decoding/**
```
│── main.py              # کد اصلی پردازش PDF
│── mydic.json           # دیکشنری حروف
│── mydict.json          # دیکشنری احتمالات
│── Handbook.pdf         # فایل PDF ورودی
│── README.md            # مستندات پروژه
```

---
# 📄 تبدیل داده‌های JSON به CSV با پایتون  

این کد Python برای کار با چندین فایل JSON و تبدیل داده‌های آن‌ها به فرمت CSV طراحی شده است. هر بخش از کد وظیفه خاصی را بر عهده دارد. بیایید هر بخش را بررسی کنیم:

---

## 🚀 ویژگی‌های پروژه  

✅ خواندن داده‌ها از فایل‌های JSON  
✅ تبدیل دیکشنری‌ها به DataFrameهای pandas  
✅ ذخیره داده‌ها به فرمت CSV  
✅ مرتب‌سازی کلیدهای دیکشنری‌ها  
✅ ترکیب داده‌های مختلف در یک فایل  

---

## 🛠️ پیش‌نیازها  

قبل از اجرای کد، باید کتابخانه‌های مورد نیاز را نصب کنید. برای این کار، دستور زیر را اجرا کنید:  

```bash
pip install pandas

---

# 📄 مراحل اجرای برنامه
##
1️⃣ ایمپورت کردن کتابخانه‌ها
```python
Copy
Edit
import pandas as pd
import json
📌 در اینجا، دو کتابخانه pandas و json ایمپورت می‌شوند. pandas برای کار با داده‌های جدولی و json برای کار با داده‌های فرمت JSON استفاده می‌شود.

2️⃣ خواندن فایل‌های JSON
python
Copy
Edit
with open('mydic.json') as f:
    mydic = json.load(f)

with open('mydict.json') as f:
    mydict = json.load(f)

with open('encoding_dict.json') as f:
    encoding_dict = json.load(f)
📌 در اینجا، سه فایل JSON با استفاده از کتابخانه json باز و محتویات آن‌ها بارگذاری می‌شوند و در متغیرهای mydic, mydict و encoding_dict ذخیره می‌شوند.

3️⃣ تبدیل داده‌های JSON به DataFrame‌های pandas و ذخیره آن‌ها به CSV
python
Copy
Edit
probabilities = mydict
huffman_table = pd.DataFrame.from_dict(encoding_dict, orient='index')
huffman_table.to_csv('huffman_table.csv', index=True)
probabilities_table = pd.DataFrame.from_dict(probabilities, orient='index')
probabilities_table.to_csv('probabilities_table.csv', index=True)
📌 داده‌های دو فایل JSON (mydict و encoding_dict) به DataFrame‌های pandas تبدیل می‌شوند و سپس به فایل‌های CSV (huffman_table.csv و probabilities_table.csv) ذخیره می‌شوند.

4️⃣ مرتب‌سازی کلیدهای دیکشنری‌ها و ایجاد دیکشنری‌های مرتب‌شده
python
Copy
Edit
myKeys = list(probabilities.keys())
myKeys.sort()
sorted_prob = {i: probabilities[i] for i in myKeys}

myKeys = list(encoding_dict.keys())
myKeys.sort()
sorted_encode = {i: encoding_dict[i] for i in myKeys}

myKeys = list(mydic.keys())
myKeys.sort()
sorted_char = {i: mydic[i] for i in myKeys}
📌 کلیدهای هر دیکشنری (یعنی probabilities, encoding_dict و mydic) استخراج، مرتب‌سازی و سپس دوباره به صورت دیکشنری‌های مرتب‌شده ذخیره می‌شوند.

5️⃣ ترکیب دیکشنری‌های مرتب‌شده و تبدیل به DataFrame
python
Copy
Edit
dicts = [sorted_prob, sorted_encode, sorted_char]
df = pd.DataFrame(dicts)
df.to_csv("list_of_dict_to_csv.csv")
📌 دیکشنری‌های مرتب‌شده در یک لیست قرار داده می‌شوند، سپس به یک DataFrame pandas تبدیل می‌شوند و در نهایت به فایل CSV به نام list_of_dict_to_csv.csv ذخیره می‌شوند.

🔍 نتیجه‌گیری
این کد اساساً داده‌های موجود در فایل‌های JSON را می‌خواند، آن‌ها را به فرمت DataFrame تبدیل می‌کند و سپس به فایل‌های CSV ذخیره می‌کند. همچنین دیکشنری‌های موجود را مرتب کرده و به یک فایل CSV واحد ذخیره می‌کند.

Copy
Edit

این متن به صورت یکپارچه و بدون جداسازی بخش‌ها نوشته شده است. شما می‌توانید آن را به راحتی کپی کرده و در هر محیط Markdown قرار دهید.






