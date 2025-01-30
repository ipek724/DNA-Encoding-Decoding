
---

### 2. فایل `Documentation.md`

```markdown
# Documentation for DNA Encoding and Decoding Project

## مقدمه
این مستند شامل توضیحات دقیق درباره کدهای پروژه و نحوه کار آن‌ها است. هدف از این پروژه، رمزگذاری و دکد کردن داده‌ها به توالی‌های DNA و بالعکس است.

## توضیحات ساختاری

### 1. `dna-encoder.py`

#### توابع موجود

###### 1.1 `encode_data(data)`

این تابع مسئول کدگذاری داده‌ها به توالی‌های DNA است. با تبدیل هر کاراکتر از داده‌ها به باینری و سپس به نمایندگی‌های DNA (A, T, C, G) کار می‌کند.

```python
def encode_data(data):
    binary_string = ''.join(format(ord(c), '08b') for c in data)
    return binary_to_dna(binary_string)
Copy
