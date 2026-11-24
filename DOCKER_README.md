# تشغيل ApkPatcher بواجهة ويب عبر Docker

```bash
docker compose up -d --build
```
ثم افتح: http://localhost:8000

أو بدون compose:
```bash
docker build -t apkpatcher-web .
docker run -d -p 8000:8000 -v apkpatcher-data:/data apkpatcher-web
```

## كيف يعمل
- ترفع ملف `.apk` (أو `.apks/.apkm/.xapk`) من الصفحة، وتختار نوع المعالجة.
- يُحفظ الملف **بنفس اسمه الأصلي**، والأداة تُنتج `الاسم_Patched.apk` (سلوكها الأصلي) فتنزّله مباشرة.
- لكل مهمة مجلد مستقل يُحذف تلقائياً بعد `JOB_TTL` ثانية (6 ساعات افتراضياً).

## متغيرات البيئة
| المتغير | الافتراضي | المعنى |
|---|---|---|
| `MAX_JOBS` | 1 | معالجات متزامنة |
| `JOB_TIMEOUT` | 1800 | مهلة كل ملف بالثواني |
| `MAX_UPLOAD_MB` | 2048 | أقصى حجم رفع |
| `JOB_TTL` | 21600 | مدة الاحتفاظ بالنتائج |

## ملاحظات
- يحتاج البناء إلى إنترنت لتنزيل أدوات الجافا (APKTool, APKEditor, ApkSig) من GitHub.
- الواجهة بلا تسجيل دخول: لا تعرّضها للإنترنت العام دون حماية (reverse proxy مع مصادقة).
- لإضافة أعلام أخرى للأداة، أضفها في القواميس أعلى `webui/app.py` وفي `index.html`.
