# آموزش کار با API مکس سرور در وب و پایتون

## اول یه کم توضیح بدم

مکس سرور یه پلتفرم باحاله که می‌تونی باهاش فایل‌های JSON، HTML، CSS و حتی عکس‌ها رو مدیریت کنی. این سرویس از طریق APIهایی که توی آدرس `https://api-code.ir/max/` هست کار می‌کنه. توی این آموزش، قراره ببینی چطور می‌تونی از این APIها توی دو محیط استفاده کنی:

1. **وب**: با HTML، CSS و JavaScript یه رابط کاربری ساده درست می‌کنیم.
2. **پایتون**: با کتابخونه `requests` درخواست‌های HTTP می‌فرستیم.

APIها شامل ورود، ثبت‌نام، مدیریت فایل (مثل ساختن، ویرایش، قفل کردن، حذف، گرفتن محتوا) و آپلود عکس هستن. من قدم به قدم با مثال‌های عملی برات توضیح می‌دم که چطور ازشون استفاده کنی و چه پارامترهایی برای هر درخواست لازمه.

## چی لازم داری؟

- یه ذره آشنایی با HTML، CSS، JavaScript (برای وب) یا پایتون (برای اسکریپت).
- ابزارها:
  - برای وب: یه ویرایشگر کد مثل VS Code و یه مرورگر.
  - برای پایتون: پایتون 3.x و کتابخونه `requests` (با دستور `pip install requests` نصبش کن).
- مطمئن شو لینک‌های API (`https://api-code.ir/max/`) کار می‌کنن.
- یه حساب کاربری تو مکس سرور داشته باش (نام کاربری، رمز و ایمیل).

## لینک‌های API و پارامترها

اینجا همه APIهای مکس سرور رو با پارامترهای لازم و طریقه ارسال درخواست لیست کردم. هر API توضیح داره که چه متدی (GET یا POST) می‌خواد و داده‌ها رو چطور باید بفرستی.

### احراز هویت

1. **ورود**  
   - **لینک**: `https://api-code.ir/max/m.php` (POST)  
   - **پارامترها**:
     - `username`: نام کاربری (رشته، اجباری).
     - `pass`: رمز عبور (رشته، اجباری).
   - **طریقه ارسال**: داده‌ها رو به صورت فرم (`application/x-www-form-urlencoded`) یا JSON بفرست.
   - **خروجی**: اگه درست باشه، اطلاعات کاربر (مثل `[{"username": "test", "pass": "123"}]`) برمی‌گرده. اگه خطا باشه، پیام خطا (مثل `"error"`) می‌ده.
   - **مثال درخواست**:
     ```
     POST https://api-code.ir/max/m.php
     Content-Type: application/x-www-form-urlencoded
     username=test&pass=123
     ```

2. **ثبت‌نام**  
   - **لینک**: `https://api-code.ir/max/m2.php` (POST)  
   - **پارامترها**:
     - `username`: نام کاربری (رشته، اجباری).
     - `pass`: رمز عبور (رشته، اجباری).
     - `email`: ایمیل (رشته، اجباری).
   - **طریقه ارسال**: داده‌ها رو به صورت فرم یا JSON بفرست.
   - **خروجی**: اگه موفق باشه، `"ok"` برمی‌گرده. اگه خطا باشه، پیام خطا می‌ده.
   - **مثال درخواست**:
     ```
     POST https://api-code.ir/max/m2.php
     Content-Type: application/x-www-form-urlencoded
     username=test&pass=123&email=test@example.com
     ```

### مدیریت فایل‌های JSON

این APIها برای فایل‌های JSON هستن. برای HTML و CSS هم همین‌ها کار می‌کنن، فقط به‌جای `jsonnm` باید `htmlmk` (برای HTML) یا `cssnk` (برای CSS) بذاری.

1. **ساختن فایل**  
   - **لینک**: `https://api-code.ir/max/jsonnm/Cfile.php` (POST)  
   - **پارامترها**:
     - `username`: نام کاربری (رشته، اجباری).
     - `upassword`: رمز عبور کاربر (رشته، اجباری).
     - `name`: نام فایل (بدون پسوند، رشته، اجباری).
     - `password`: رمز فایل (رشته، اجباری).
     - `type`: نوع فایل (`json`, `html`, `css`, اجباری).
   - **طریقه ارسال**: داده‌ها رو به صورت فرم یا JSON بفرست.
   - **خروجی**: اگه موفق باشه، `"ok"` برمی‌گرده. اگه خطا باشه، پیام خطا می‌ده.
   - **مثال درخواست**:
     ```
     POST https://api-code.ir/max/jsonnm/Cfile.php
     Content-Type: application/x-www-form-urlencoded
     username=test&upassword=123&name=myfile&password=filepass&type=json
     ```

2. **ویرایش فایل**  
   - **لینک**: `https://api-code.ir/max/jsonnm/edit.php` (POST)  
   - **پارامترها**:
     - `name`: نام فایل (بدون پسوند، رشته، اجباری).
     - `pass`: رمز فایل (رشته، اجباری).
     - `data`: محتوای جدید فایل (رشته، اجباری).
   - **طریقه ارسال**: داده‌ها رو به صورت فرم یا JSON بفرست.
   - **خروجی**: اگه موفق باشه، `"ok"` برمی‌گرده. اگه خطا باشه، پیام خطا می‌ده.
   - **مثال درخواست**:
     ```
     POST https://api-code.ir/max/jsonnm/edit.php
     Content-Type: application/x-www-form-urlencoded
     name=myfile&pass=filepass&data={"key": "new_value"}
     ```

3. **گرفتن لیست فایل‌ها**  
   - **لینک**: `https://api-code.ir/max/jsonnm/Get.php` (POST)  
   - **پارامترها**:
     - `username`: نام کاربری (رشته، اجباری).
     - `pass`: رمز عبور کاربر (رشته، اجباری).
   - **طریقه ارسال**: داده‌ها رو به صورت فرم یا JSON بفرست.
   - **خروجی**: لیست فایل‌ها به صورت JSON (مثل `[{"name": "myfile"}]`) یا پیام خطا.
   - **مثال درخواست**:
     ```
     POST https://api-code.ir/max/jsonnm/Get.php
     Content-Type: application/x-www-form-urlencoded
     username=test&pass=123
     ```

4. **قفل کردن فایل**  
   - **لینک**: `https://api-code.ir/max/jsonnm/lock.php` (GET)  
   - **پارامترها**:
     - `name`: نام فایل (بدون پسوند، رشته، اجباری).
     - `type`: نوع فایل (`json`, `html`, `css`, اجباری).
   - **طریقه ارسال**: پارامترها رو توی URL به صورت query string بفرست.
   - **خروجی**: اگه موفق باشه، `"ok"` یا پیام موفقیت برمی‌گرده. اگه خطا باشه، پیام خطا می‌ده.
   - **مثال درخواست**:
     ```
     GET https://api-code.ir/max/jsonnm/lock.php?name=myfile&type=json
     ```

5. **باز کردن قفل فایل**  
   - **لینک**: `https://api-code.ir/max/jsonnm/un_lock.php` (GET)  
   - **پارامترها**:
     - `name`: نام فایل (بدون پسوند، رشته، اجباری).
     - `type`: نوع فایل (`json`, `html`, `css`, اجباری).
   - **طریقه ارسال**: پارامترها رو توی URL به صورت query string بفرست.
   - **خروجی**: اگه موفق باشه، `"ok"` یا پیام موفقیت برمی‌گرده. اگه خطا باشه، پیام خطا می‌ده.
   - **مثال درخواست**:
     ```
     GET https://api-code.ir/max/jsonnm/un_lock.php?name=myfile&type=json
     ```

6. **حذف فایل**  
   - **لینک**: `https://api-code.ir/max/jsonnm/Rfile.php` (POST)  
   - **پارامترها**:
     - `username`: نام کاربری (رشته، اجباری).
     - `upassword`: رمز عبور کاربر (رشته، اجباری).
     - `name`: نام فایل (بدون پسوند، رشته، اجباری).
     - `password`: رمز فایل (رشته، اجباری).
   - **طریقه ارسال**: داده‌ها رو به صورت فرم یا JSON بفرست.
   - **خروجی**: اگه موفق باشه، `"ok"` برمی‌گرده. اگه خطا باشه، پیام خطا می‌ده.
   - **مثال درخواست**:
     ```
     POST https://api-code.ir/max/jsonnm/Rfile.php
     Content-Type: application/x-www-form-urlencoded
     username=test&upassword=123&name=myfile&password=filepass
     ```

7. **گرفتن محتوای فایل**  
   - **لینک**: `https://api-code.ir/max/jsonnm/fetch.php` (GET)  
   - **پارامترها**:
     - `name`: نام فایل (بدون پسوند، رشته، اجباری).
     - `type`: نوع فایل (`json`, `html`, `css`, اجباری).
   - **طریقه ارسال**: پارامترها رو توی URL به صورت query string بفرست.
   - **خروجی**: اگه موفق باشه، محتوای فایل (مثل `{"key": "value"}` برای JSON) برمی‌گرده. اگه خطا باشه، پیام خطا به شکل JSON (مثل `{"error": "فایل پیدا نشد"}`).
   - **مثال درخواست**:
     ```
     GET https://api-code.ir/max/jsonnm/fetch.php?name=myfile&type=json
     ```

### مدیریت فایل‌های HTML و CSS

برای HTML، همه لینک‌های بالا رو با `htmlmk` به‌جای `jsonnm` استفاده کن (مثلا `https://api-code.ir/max/htmlmk/Cfile.php`). برای CSS هم با `cssnk` (مثلا `https://api-code.ir/max/cssnk/Cfile.php`). پارامترها و طریقه ارسال همون‌جوریه.

### آپلود عکس

- **لینک**: `https://api-code.ir/max/up/upload.php` (POST)  
- **پارامترها**:
  - `username`: نام کاربری (رشته، اجباری).
  - `upassword`: رمز عبور کاربر (رشته، اجباری).
  - `file`: فایل عکس (فرمت JPG, PNG, GIF، حداکثر 5MB، اجباری).
- **طریقه ارسال**: داده‌ها رو به صورت `multipart/form-data` بفرست (برای آپلود فایل).
- **خروجی**: اگه موفق باشه، آدرس فایل آپلودشده یا پیام موفقیت برمی‌گرده. اگه خطا باشه، پیام خطا می‌ده.
- **مثال درخواست**:
  ```
  POST https://api-code.ir/max/up/upload.php
  Content-Type: multipart/form-data
  username=test&upassword=123&file=@/path/to/image.jpg
  ```

## بخش 1: استفاده توی وب

بیا یه رابط کاربری ساده با HTML، CSS (با Tailwind) و JavaScript درست کنیم که به APIهای مکس سرور وصل بشه.

### کد نمونه (index.html)

```html
<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>مکس سرور - نمونه وب</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body { font-family: 'Vazirmatn', sans-serif; }
    .hidden { display: none; }
  </style>
  <link href="https://fonts.googleapis.com/css2?family=Vazirmatn:wght@400;700&display=swap" rel="stylesheet">
</head>
<body class="bg-gray-900 text-white min-h-screen p-4">
  <section id="loginSection" class="max-w-md mx-auto">
    <h2 class="text-2xl font-bold mb-4">ورود</h2>
    <form id="loginForm">
      <input type="text" name="username" placeholder="نام کاربری" class="w-full p-2 mb-2 bg-gray-800 rounded" required>
      <input type="password" name="pass" placeholder="رمز عبور" class="w-full p-2 mb-2 bg-gray-800 rounded" required>
      <button type="submit" class="w-full bg-blue-500 p-2 rounded">ورود</button>
    </form>
    <p id="loginError" class="text-red-500 mt-2 hidden"></p>
  </section>

  <section id="dashboardSection" class="hidden">
    <h2 class="text-2xl font-bold mb-4">مدیریت فایل</h2>
    <form id="createFileForm" class="mb-4">
      <select name="type" class="p-2 bg-gray-800 rounded">
        <option value="json">JSON</option>
        <option value="html">HTML</option>
        <option value="css">CSS</option>
      </select>
      <input type="text" name="name" placeholder="نام فایل" class="p-2 bg-gray-800 rounded" required>
      <input type="password" name="password" placeholder="رمز فایل" class="p-2 bg-gray-800 rounded" required>
      <button type="submit" class="bg-green-500 p-2 rounded">ایجاد</button>
    </form>
    <div id="fileList"></div>
  </section>

  <script>
    const API_BASE = 'https://api-code.ir/max/';
    let currentUser = null;

    function showSection(sectionId) {
      document.querySelectorAll('section').forEach(s => s.classList.add('hidden'));
      document.getElementById(sectionId).classList.remove('hidden');
    }

    function showError(id, message) {
      const errorEl = document.getElementById(id);
      errorEl.textContent = message;
      errorEl.classList.remove('hidden');
      setTimeout(() => errorEl.classList.add('hidden'), 3000);
    }

    document.getElementById('loginForm').addEventListener('submit', async (e) => {
      e.preventDefault();
      const formData = new FormData(e.target);
      try {
        const response = await fetch(`${API_BASE}m.php`, {
          method: 'POST',
          body: formData
        });
        const data = await response.text();
        if (data.includes('error')) {
          showError('loginError', 'نام کاربری یا رمز عبور اشتباهه.');
        } else {
          currentUser = JSON.parse(data)[0];
          showSection('dashboardSection');
          loadFiles();
        }
      } catch (err) {
        showError('loginError', 'مشکلی تو اتصال به سرور پیش اومد.');
      }
    });

    document.getElementById('createFileForm').addEventListener('submit', async (e) => {
      e.preventDefault();
      const formData = new FormData(e.target);
      formData.append('username', currentUser.username);
      formData.append('upassword', currentUser.pass);
      const type = formData.get('type');
      try {
        const response = await fetch(`${API_BASE}${type}nm/Cfile.php`, {
          method: 'POST',
          body: formData
        });
        const data = await response.text();
        if (data === 'ok') {
          loadFiles();
        } else {
          alert('مشکلی تو ساختن فایل پیش اومد.');
        }
      } catch (err) {
        alert('مشکلی تو اتصال به سرور پیش اومد.');
      }
    });

    async function loadFiles() {
      const fileList = document.getElementById('fileList');
      fileList.innerHTML = '';
      const types = ['json', 'html', 'css'];
      for (const type of types) {
        const formData = new FormData();
        formData.append('username', currentUser.username);
        formData.append('pass', currentUser.pass);
        try {
          const response = await fetch(`${API_BASE}${type}nm/Get.php`, {
            method: 'POST',
            body: formData
          });
          const data = await response.text();
          if (!data.includes('error')) {
            JSON.parse(data).forEach(file => {
              const div = document.createElement('div');
              div.className = 'p-2 bg-gray-800 rounded mb-2';
              div.innerHTML = `
                ${file.name}.${type}
                <button onclick="fetchFile('${file.name}', '${type}')" class="bg-blue-500 p-1 rounded ml-2">نمایش محتوا</button>
                <button onclick="showEditModal('${file.name}', '${type}')" class="bg-yellow-500 p-1 rounded ml-2">ویرایش</button>
              `;
              fileList.appendChild(div);
            });
          }
        } catch (err) {
          console.error('Error:', err);
        }
      }
    }

    async function fetchFile(name, type) {
      try {
        const response = await fetch(`${API_BASE}${type}nm/fetch.php?name=${encodeURIComponent(name)}&type=${type}`);
        if (!response.ok) throw new Error('خطا تو گرفتن محتوا');
        const data = await response.text();
        alert(`محتوای فایل ${name}.${type}:\n${data}`);
      } catch (err) {
        alert('مشکلی تو گرفتن محتوای فایل پیش اومد: ' + err.message);
      }
    }

    async function showEditModal(name, type) {
      try {
        const response = await fetch(`${API_BASE}${type}nm/fetch.php?name=${encodeURIComponent(name)}&type=${type}`);
        if (!response.ok) throw new Error('خطا تو گرفتن محتوا');
        const data = await response.text();
        const modal = document.createElement('div');
        modal.className = 'fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center';
        modal.innerHTML = `
          <div class="bg-gray-800 p-4 rounded max-w-lg w-full">
            <h3 class="text-xl mb-4">ویرایش ${name}.${type}</h3>
            <textarea class="w-full p-2 bg-gray-900 rounded" rows="10">${data}</textarea>
            <button onclick="this.parentElement.parentElement.remove()" class="bg-red-500 p-2 rounded mt-2">بستن</button>
          </div>
        `;
        document.body.appendChild(modal);
      } catch (err) {
        alert('مشکلی تو گرفتن محتوای فایل پیش اومد: ' + err.message);
      }
    }
  </script>
</body>
</html>
```

### توضیح کد وب

این کد یه صفحه ساده داره با دو بخش: ورود و داشبورد. فرم ورود به `m.php` درخواست می‌فرسته و اطلاعات کاربر رو ذخیره می‌کنه. توی داشبورد می‌تونی فایل بسازی (با `Cfile.php`) و لیست فایل‌ها رو با `Get.php` ببینی. دکمه "نمایش محتوا" محتوای فایل رو با `fetch.php` نشون می‌ده و دکمه "ویرایش" یه پنجره باز می‌کنه که می‌تونی محتوای فایل رو ببینی. برای استایل از Tailwind و برای فونت فارسی از Vazirmatn استفاده کردم. این کد ساده‌ست، اگه برای پروژه واقعی می‌خوای، باید یه سری چیزا مثل CSRF توکن بهش اضافه کنی.

### چطور اجراش کنی؟

1. کد رو توی فایل `index.html` ذخیره کن.
2. یه سرور محلی راه بنداز (مثل `http-server` توی Node.js):
   ```bash
   npm install -g http-server
   http-server
   ```
3. توی مرورگر برو به `http://localhost:8080`.
4. با حساب مکس سرورت وارد شو، فایل بساز و با دکمه‌های "نمایش محتوا" یا "ویرایش" محتوای فایل‌ها رو چک کن.

## بخش 2: استفاده توی پایتون

حالا می‌ریم سراغ پایتون و یه اسکریپت می‌نویسیم که به APIها وصل بشه و کارای اصلی رو انجام بده.

### کد نمونه (max_server_api.py)

```python
import requests
import json

API_BASE = 'https://api-code.ir/max/'

def login(username, password):
    url = f'{API_BASE}m.php'
    data = {'username': username, 'pass': password}
    response = requests.post(url, data=data)
    if 'error' in response.text:
        return None
    return json.loads(response.text)[0]

def register(username, password, email):
    url = f'{API_BASE}m2.php'
    data = {'username': username, 'pass': password, 'email': email}
    response = requests.post(url, data=data)
    return response.text == 'ok'

def create_file(username, user_password, file_type, file_name, file_password):
    url = f'{API_BASE}{file_type}nm/Cfile.php'
    data = {
        'username': username,
        'upassword': user_password,
        'name': file_name,
        'password': file_password,
        'type': file_type
    }
    response = requests.post(url, data=data)
    return response.text == 'ok'

def get_files(username, password, file_type):
    url = f'{API_BASE}{file_type}nm/Get.php'
    data = {'username': username, 'pass': password}
    response = requests.post(url, data=data)
    if 'error' in response.text:
        return []
    return json.loads(response.text)

def fetch_file(file_name, file_type):
    url = f'{API_BASE}{file_type}nm/fetch.php?name={file_name}&type={file_type}'
    response = requests.get(url)
    if response.status_code == 200:
        return response.text
    return None

def edit_file(username, password, file_type, file_name, file_password, new_content):
    url = f'{API_BASE}{file_type}nm/edit.php'
    data = {
        'name': file_name,
        'pass': file_password,
        'data': new_content
    }
    response = requests.post(url, data=data)
    return response.text == 'ok'

def main():
    username = 'your_username'
    password = 'your_password'
    email = 'your_email@example.com'

    if not login(username, password):
        if register(username, password, email):
            print('ثبت‌نام با موفقیت انجام شد!')
        else:
            print('مشکلی تو ثبت‌نام پیش اومد.')
            return

    user = login(username, password)
    if not user:
        print('مشکلی تو ورود پیش اومد.')
        return
    print('ورود موفق:', user['username'])

    if create_file(username, password, 'json', 'test', 'file_password'):
        print('فایل JSON ساخته شد.')
    else:
        print('مشکلی تو ساختن فایل پیش اومد.')

    files = get_files(username, password, 'json')
    print('فایل‌های JSON:', files)

    content = fetch_file('test', 'json')
    if content:
        print('محتوای فایل:', content)
        new_content = content.replace('}', ',"new_key":"new_value"}')
        if edit_file(username, password, 'json', 'test', 'file_password', new_content):
            print('فایل ویرایش شد.')
        else:
            print('مشکلی تو ویرایش فایل پیش اومد.')
    else:
        print('مشکلی تو گرفتن محتوا پیش اومد.')

if __name__ == '__main__':
    main()
```

### توضیح کد پایتون

این اسکریپت از کتابخونه `requests` برای فرستادن درخواست‌های HTTP استفاده می‌کنه. توابعش این کارا رو می‌کنن: `login` برای ورود، `register` برای ثبت‌نام، `create_file` برای ساختن فایل، `get_files` برای گرفتن لیست فایل‌ها، `fetch_file` برای گرفتن محتوای فایل و `edit_file` برای ویرایش فایل. خطاها رو هم یه جورایی مدیریت می‌کنه که بدونی کجا مشکل پیش اومده. این کد ساده‌ست، اگه برای پروژه بزرگ می‌خوای، باید مدیریت خطا و امنیت رو قوی‌تر کنی.

### چطور اجراش کنی؟

1. کد رو توی فایل `max_server_api.py` ذخیره کن.
2. کتابخونه `requests` رو نصب کن:
   ```bash
   pip install requests
   ```
3. اطلاعات حسابت (`your_username`, `your_password`, `your_email@example.com`) رو توی کد بذار.
4. اسکریپت رو اجرا کن:
   ```bash
   python max_server_api.py
   ```
5. خروجی نشون می‌ده که ورود، ساختن فایل، گرفتن لیست و محتوا و ویرایش فایل چطور انجام شدن.

## بخش 3: کار با fetch.php برای گرفتن محتوای فایل

یه endpoint به اسم `fetch.php` به APIهای مکس سرور اضافه شده که می‌تونی باهاش محتوای فایل‌های JSON، HTML و CSS رو بگیری. این برای وقتی خوبه که بخوای محتوای فایل رو ببینی یا ویرایش کنی.

### fetch.php چیکار می‌کنه؟

- **کارش چیه؟**: محتوای فایل‌های JSON، HTML یا CSS رو از سرور می‌گیره.
- **پارامترها**:
  - `name`: اسم فایل (بدون پسوند).
  - `type`: نوع فایل (`json`, `html`, `css`).
  - مثال: `https://api-code.ir/max/jsonnm/fetch.php?name=test&type=json`
- **طریقه ارسال**: پارامترها رو توی URL به صورت query string بفرست (GET).
- **خروجی**:
  - اگه اوکی باشه: محتوای فایل (مثلا `{"key": "value"}` برای JSON یا `<p>Hello</p>` برای HTML).
  - اگه خطا باشه: پیام خطا به شکل JSON (مثلا `{"error": "فایل پیدا نشد"}`).
- **کاربرد**: می‌تونی ازش برای نمایش محتوا توی رابط کاربری وب یا پردازش داده‌ها توی پایتون استفاده کنی.

### چطور توی وب از fetch.php استفاده کنی؟

توی کد وب بالا، تابع `fetchFile` از `fetch.php` استفاده می‌کنه و محتوا رو توی یه alert نشون می‌ده. تابع `showEditModal` هم یه پنجره باز می‌کنه که محتوای فایل رو نشون می‌ده. دکمه "ویرایش" توی لیست فایل‌ها این پنجره رو باز می‌کنه. اگه بخوای تغییرات رو ذخیره کنی، باید با `edit.php` کار کنی که توی این کد نیست، ولی می‌تونی بعدا اضافه کنی.

### چطور توی پایتون از fetch.php استفاده کنی؟

توی کد پایتون، تابع `fetch_file` محتوای فایل رو با `fetch.php` می‌گیره. توی تابع `main` می‌بینی که چطور محتوای یه فایل JSON گرفته شده و بعد با تابع `edit_file` ویرایش شده. مثلا، یه کلید جدید به JSON اضافه کردیم. اگه بخوای کارهای پیچیده‌تر بکنی، می‌تونی تابع `fetch_file` رو گسترش بدی یا با APIهای دیگه مثل `edit.php` ترکیبش کنی.

### چطور APIها رو تست کنی؟

برای مطمئن شدن که APIها درست کار می‌کنن، می‌تونی با Postman یا مرورگر تست کنی. چندتا مثال:

- **fetch.php**:
  ```
  GET https://api-code.ir/max/jsonnm/fetch.php?name=test&type=json
  ```
  اگه فایل `test.json` باشه، محتواش (مثلا `{"key": "value"}`) برمی‌گرده. اگه نباشه، خطا می‌گیرید.
- **ساختن فایل**:
  ```
  POST https://api-code.ir/max/jsonnm/Cfile.php
  Content-Type: application/x-www-form-urlencoded
  username=test&upassword=123&name=test&password=filepass&type=json
  ```
  باید `"ok"` برگرده.
- **آپلود عکس**:
  ```
  POST https://api-code.ir/max/up/upload.php
  Content-Type: multipart/form-data
  username=test&upassword=123&file=@/path/to/image.jpg
  ```
  آدرس فایل آپلودشده یا پیام موفقیت می‌گیرید.

برای HTML و CSS هم لینک‌ها رو با `htmlmk` یا `cssnk` تست کن.

## چند نکته مهم

- **امنیت**: این APIها یه سری مشکلات دارن، مثل امکان SQL Injection یا ذخیره رمز به صورت متن ساده. اگه برای پروژه واقعی می‌خوای، باید سرور رو ایمن کنی. توی وب، CSRF توکن بذار و توی پایتون مدیریت جلسه رو درست کن.
- **CORS**: اگه توی وب خطای CORS گرفتی، چک کن که سرور API درخواست‌ها رو از دامنه‌ات (مثل `localhost` یا `netlify.app`) قبول کنه.
- **محدودیت‌ها**:
  - عکس‌ها فقط می‌تونن JPG، PNG یا GIF باشن و حداکثر 5MB.
  - اسم فایل‌ها نباید تکراری باشه یا کاراکترهای عجیب (مثل `/` یا `\`) داشته باشه.
- **تست**: قبل از اینکه حسابی از APIها استفاده کنی، با Postman تستشون کن که مطمئن شی همه‌چیز روبراهه.

## حرف آخر

با این آموزش می‌تونی به همه APIهای مکس سرور وصل شی، از ساختن و ویرایش فایل گرفته تا گرفتن محتوا و آپلود عکس. کدهایی که دادم ساده‌ان، ولی می‌تونی گسترششون بدی. مثلا:

- توی وب، با React یه رابط کاربری خفن‌تر بساز.
- توی پایتون، با Flask یه API محلی درست کن یا با Tkinter یه رابط گرافیکی بساز.
- قابلیت‌های دیگه مثل قفل کردن پیشرفته‌تر یا آپلود چندتا فایل رو اضافه کن.

اگه سوالی داری یا چیزی بیشتر لازمته، بگو که کمکت کنم!
