# مستندات API استخراج متن از فایل‌ها

## معرفی
این API به شما امکان می‌دهد محتوای متنی فایل‌های مختلف را با استفاده از URL (روش GET) یا ارسال مستقیم محتوا/فایل (روش POST) استخراج کنید. API از فرمت‌های مختلف فایل پشتیبانی می‌کند و محتوای متنی استخراج‌شده را در قالب JSON بازمی‌گرداند.

## آدرس API
- **URL پایه**: `https://api-code.ir/file-ext/api2.php`

## روش‌های درخواست
API از دو روش HTTP پشتیبانی می‌کند:
1. **GET**: برای استخراج متن از فایل‌هایی که از طریق URL در دسترس هستند.
2. **POST**: برای استخراج متن از فایل‌های آپلودشده یا محتوای ارسالی به‌صورت JSON.

---

## فرمت‌های پشتیبانی‌شده
API از فرمت‌های زیر پشتیبانی می‌کند (بر اساس پسوند فایل):
- متنی: txt, log, ini, conf, cfg, properties, env, gitignore, asc, text
- کد منبع: css, js, php, py, java, c, cpp, h, sql, sh, bat, ps1, rb, go, ts, pl, lua, r, swift, kt, scala, cs, vb, vbs
- نشانه‌گذاری: html, htm, md, markdown, rst, org, tex, bib
- داده‌ای: csv, tsv, json, yaml, xml, xhtml, rss, atom, opml
- زیرنویس: srt, vtt
- تقویم و مخاطب: ics, ical, vcf
- تفاوت و پچ: diff, patch
- اسناد: pdf, doc, docx, xls, xlsx, ppt, pptx, odt, ods, odp, wpd, pages, numbers, key, epub, mobi, azw, ps, dvi, wps, sxw, sxc, sxi, abw, kwd
- سایر: nfo, rtf

**توجه**: برخی فرمت‌ها (مانند pdf، doc، docx و غیره) نیاز به کتابخانه‌های اضافی دارند که ممکن است در سرور API در دسترس نباشند. در این موارد، پیام خطای `library_required` بازگردانده می‌شود.

---

## درخواست GET
### توضیحات
برای استخراج متن از فایلی که از طریق URL در دسترس است، از روش GET استفاده کنید.

### پارامترها
- **url** (اجباری): آدرس URL فایل موردنظر.

### نمونه درخواست
```
GET https://api-code.ir/file-ext/api2.php?url=https://example.com/sample.txt
```

### نکات
- URL باید معتبر باشد و به یک فایل با فرمت پشتیبانی‌شده اشاره کند.
- پسوند فایل از URL استخراج می‌شود و باید در لیست فرمت‌های پشتیبانی‌شده باشد.

---

## درخواست POST
### توضیحات
برای استخراج متن از محتوای فایل یا فایل آپلودشده، از روش POST استفاده کنید. دو روش برای ارسال داده وجود دارد:
1. **ارسال JSON**: محتوای فایل و فرمت آن را به‌صورت JSON ارسال کنید.
2. **آپلود فایل**: فایل را مستقیماً آپلود کنید.

### پارامترهای JSON
- **content** (اجباری): محتوای فایل به‌صورت رشته.
- **extension** (اجباری): پسوند فایل (مثال: txt، json).
- **name** (اجباری): نام فایل (می‌تواند دلخواه باشد).

### نمونه درخواست JSON
```json
POST https://api-code.ir/file-ext/api2.php
Content-Type: application/json

{
  "content": "This is a sample text content.",
  "extension": "txt",
  "name": "sample.txt"
}
```

### نمونه درخواست آپلود فایل
```bash
curl -X POST -F "file=@/path/to/sample.txt" https://api-code.ir/file-ext/api2.php
```

### نکات
- برای درخواست JSON، هدر `Content-Type: application/json` الزامی است.
- برای آپلود فایل، از فرم‌دیتا با کلید `file` استفاده کنید.
- پسوند فایل باید در لیست فرمت‌های پشتیبانی‌شده باشد.

---

## پاسخ API
API پاسخ را در قالب JSON با ساختار زیر بازمی‌گرداند:
```json
{
  "status": "success | error",
  "message": "پیام وضعیت",
  "data": "محتوای استخراج‌شده یا null"
}
```

### پیام‌های ممکن
- **success**: متن با موفقیت استخراج شد.
- **invalid_method**: متد درخواست نامعتبر است (فقط GET و POST پشتیبانی می‌شوند).
- **no_file**: هیچ فایلی ارائه نشده است (برای POST).
- **unsupported_format**: فرمت فایل پشتیبانی نمی‌شود.
- **error_processing**: خطا در پردازش فایل.
- **url_required**: آدرس URL مورد نیاز است (برای GET).
- **invalid_url**: آدرس URL نامعتبر است (برای GET).
- **library_required**: فرمت نیاز به کتابخانه اضافی دارد.
- **invalid_json**: داده JSON نامعتبر است (برای POST با JSON).
- **missing_content**: محتوای فایل یا فرمت آن ارائه نشده است (برای POST با Hints: [Click here to view the complete API documentation](https://api-code.ir/file-ext/api2.php) با JSON).

---

## نمونه پاسخ‌ها
### موفقیت
```json
{
  "status": "success",
  "message": "متن با موفقیت استخراج شد.",
  "data": "This is a sample text content."
}
```

### خطا
```json
{
  "status": "error",
  "message": "فرمت فایل پشتیبانی نمی‌شود.",
  "data": null
}
```

---

## نمونه کد (Python)
### درخواست GET
```python
import requests

url = "https://api-code.ir/file-ext/api2.php?url=https://example.com/sample.txt"
response = requests.get(url)
print(response.json())
```

### درخواست POST (JSON)
```python
import requests

data = {
    "content": "This is a sample text content.",
    "extension": "txt",
    "name": "sample.txt"
}
headers = {"Content-Type": "application/json"}
response = requests.post("https://api-code.ir/file-ext/api2.php", json=data, headers=headers)
print(response.json())
```

---

## نکات مهم
- API محتوای متنی را استخراج و تمیز می‌کند (حذف فضاهای اضافی و خطوط خالی).
- برای فایل‌های HTML، تگ‌های `<script>` و `<style>` حذف می‌شوند.
- فایل‌های YAML به JSON تبدیل می‌شوند.
- برای فایل‌های پیچیده (مانند pdf)، ممکن است پیام `library_required` دریافت کنید.
- در صورت بروز خطا، فایل موقت ایجادشده روی سرور به‌طور خودکار حذف می‌شود.

## محدودیت‌ها
- فرمت‌هایی که نیاز به کتابخانه‌های اضافی دارند ممکن است پشتیبانی نشوند.
- اندازه فایل ممکن است به محدودیت‌های سرور بستگی داشته باشد.

## تماس با پشتیبانی
برای اطلاعات بیشتر، به وب‌سایت [api-code.ir](https://api-code.ir) مراجعه کنید.
