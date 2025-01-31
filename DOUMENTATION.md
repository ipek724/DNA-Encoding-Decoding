# 📌 ذخیره‌سازی داده‌ها در DNA و تصحیح خطا

## 📖 مقدمه
با رشد تصاعدی داده‌های دیجیتال، فناوری‌های ذخیره‌سازی سنتی با چالش‌هایی مانند ظرفیت محدود، طول عمر کوتاه و تراکم پایین مواجه شده‌اند. ذخیره‌سازی داده در **DNA** به دلیل چگالی بالا (تا **700 ترابایت در هر گرم**) و طول عمر طولانی (**هزاران سال**) به عنوان یک جایگزین نوین مورد توجه قرار گرفته است. با این حال، ذخیره‌سازی و بازیابی کارآمد داده‌ها نیازمند حل چالش‌هایی مانند **مدیریت خطا، هزینه سنتز و کارایی کدگذاری** است.

🎯 **هدف این پروژه** ارائه روشی ترکیبی از **کدگذاری هافمن** برای فشرده‌سازی و **کدگذاری همینگ** برای تشخیص و تصحیح خطا است که باعث بهبود دقت ذخیره‌سازی و بازیابی داده‌ها می‌شود.

---
## ✨ ویژگی‌ها
✅ **فشرده‌سازی داده**: استفاده از **کدگذاری هافمن** برای کاهش حجم داده‌ها.
✅ **تشخیص و تصحیح خطا**: استفاده از **کد همینگ** برای خواندن و نوشتن صحیح داده‌ها.
✅ **رمزگذاری و رمزگشایی**: تبدیل متن به **رشته‌های DNA** و بازگردانی دقیق متن اصلی.
✅ **ذخیره‌سازی بهینه**: بهره‌گیری از **زیست‌محاسبات** برای بهبود ذخیره‌سازی داده در **محیط DNA**.

---
## 📂 ساختار فایل‌ها و توضیحات توابع
این مخزن شامل چندین **Jupyter Notebook** است که مراحل **کدگذاری، فشرده‌سازی، تصحیح خطا و بازیابی** داده‌ها را پیاده‌سازی می‌کند.

### 📌 1. فشرده‌سازی داده و تحلیل فرکانس (`1.ipynb`)
📌 **توابع اصلی:**

**استخراج متن از فایل PDF:**
```python
def extract_text_from_pdf(file_path):
    with open(file_path, 'rb') as file:
        reader = PyPDF2.PdfReader(file)
        text = "".join([page.extract_text() for page in reader.pages])
    return text
```
- `extract_text_from_pdf(file_path)`: خواندن و استخراج متن از PDF.

---
### 📌 2. کدگذاری هافمن و جدول احتمالات (`2.ipynb`)
📌 **توابع اصلی:**

**ایجاد دیکشنری کدگذاری هافمن:**
```python
def create_huffman_encoding_dict(frequencies):
    heap = [[weight, [symbol, ""]] for symbol, weight in frequencies.items()]
    heapq.heapify(heap)
    while len(heap) > 1:
        lo = heapq.heappop(heap)
        hi = heapq.heappop(heap)
        for pair in lo[1:]: pair[1] = '0' + pair[1]
        for pair in hi[1:]: pair[1] = '1' + pair[1]
        heapq.heappush(heap, [lo[0] + hi[0]] + lo[1:] + hi[1:])
    return dict(sorted(heap[0][1:], key=lambda p: (len(p[-1]), p)))
```
- `create_huffman_encoding_dict(frequencies)`: ایجاد دیکشنری کدگذاری.

---
### 📌 3. تبدیل متن به رشته های دی ان ای  (`3.ipynb`)
📌 **توابع اصلی:**

**تبدیل رشته دودویی به DNA:**
```python
def binary_to_dna(binary_sequence):
    mapping = {"00": "A", "01": "T", "10": "C", "11": "G"}
    return "".join([mapping[binary_sequence[i:i+2]] for i in range(0, len(binary_sequence), 2)])
```
- `binary_to_dna(binary_sequence)`: تبدیل داده‌ها به **DNA**.

---
### 📌 4. نمایش درخت هافمن (`4.ipynb`)
📌 **توابع اصلی:**

**ساخت و نمایش گراف درخت هافمن:**
```python
def build_huffman_tree_graph(tree):
    dot = graphviz.Digraph()
    def add_nodes_edges(node, parent=None, edge_label=""):
        if isinstance(node, list):
            label = node[1] if isinstance(node[1], str) else ""
            dot.node(str(id(node)), label)
            if parent:
                dot.edge(parent, str(id(node)), label=edge_label)
            for i, child in enumerate(node[2:]):
                add_nodes_edges(child, str(id(node)), str(i))
    add_nodes_edges(tree)
    return dot
```
- `build_huffman_tree_graph(tree)`: نمایش گرافیکی درخت هافمن.

---
### 📌 5. تشخیص و تصحیح خطا (`Main Retrieval.ipynb`)
📌 **توابع اصلی:**

**شناسایی و اصلاح خطا در داده‌های کدگذاری‌شده:**
```python
def detect_and_correct_errors(encoded_data):
    error_position = 0
    for i in range(len(encoded_data)):
        if encoded_data[i] == "1":
            error_position ^= (i + 1)
    if error_position:
        encoded_data = list(encoded_data)
        encoded_data[error_position - 1] = "0" if encoded_data[error_position - 1] == "1" else "1"
    return "".join(encoded_data)
```
- `detect_and_correct_errors(encoded_data)`: شناسایی و اصلاح خطا.

---
### 📌 6. ذخیره‌سازی نهایی داده‌ها (`main store.ipynb`)
📌 **توابع اصلی:**

**تقسیم داده‌های DNA به بخش‌های کوچک‌تر برای ذخیره‌سازی:**
```python
def segment_dna_sequence(dna_sequence, segment_size):
    return [dna_sequence[i:i+segment_size] for i in range(0, len(dna_sequence), segment_size)]
```
- `segment_dna_sequence(dna_sequence, segment_size)`: تقسیم داده‌های DNA.

---
## 👨‍💻 توسعه‌دهندگان
📩 این پروژه به عنوان بخشی از **پایان‌نامه کارشناسی ارشد** درباره **ذخیره‌سازی داده در DNA** توسعه داده شده است.

�
