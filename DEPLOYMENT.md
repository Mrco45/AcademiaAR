# دليل النشر السريع لتطبيق AcademicPlus

## خيارات النشر

### 1. النشر على Vercel (مجاني ومُوصى به)

#### الخطوات:
1. **إنشاء حساب على Vercel**
   - اذهب إلى https://vercel.com
   - سجل دخول باستخدام GitHub

2. **رفع المشروع إلى GitHub**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git remote add origin https://github.com/yourusername/academicplus.git
   git push -u origin main
   ```

3. **ربط المشروع مع Vercel**
   - في Vercel، اضغط "New Project"
   - اختر المستودع من GitHub
   - اضغط "Deploy"

4. **إضافة متغيرات البيئة**
   - في إعدادات المشروع، اذهب إلى "Environment Variables"
   - أضف المتغيرات من ملف `.env.example`

### 2. النشر على Netlify

#### الخطوات:
1. **بناء التطبيق**
   ```bash
   npm run build
   ```

2. **رفع مجلد dist**
   - اذهب إلى https://netlify.com
   - اسحب مجلد `dist` إلى الموقع

3. **إعداد متغيرات البيئة**
   - في إعدادات الموقع، اذهب إلى "Environment variables"
   - أضف المتغيرات المطلوبة

### 3. النشر على خادم مخصص

#### متطلبات الخادم:
- Node.js 18+
- Nginx أو Apache
- SSL Certificate

#### الخطوات:
1. **رفع الملفات**
   ```bash
   scp -r dist/ user@server:/var/www/academicplus/
   ```

2. **إعداد Nginx**
   ```nginx
   server {
       listen 80;
       server_name academicplus.yourdomain.com;
       root /var/www/academicplus;
       index index.html;
       
       location / {
           try_files $uri $uri/ /index.html;
       }
   }
   ```

## إعداد قاعدة البيانات

### Supabase Setup:
1. إنشاء مشروع جديد على https://supabase.com
2. تشغيل SQL التالي لإنشاء الجداول:

```sql
-- جدول المستخدمين
CREATE TABLE users (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    role VARCHAR(50) DEFAULT 'student',
    created_at TIMESTAMP DEFAULT NOW()
);

-- جدول المحادثات
CREATE TABLE conversations (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    question TEXT NOT NULL,
    answer TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

-- جدول الجلسات
CREATE TABLE sessions (
    id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    session_data JSONB,
    created_at TIMESTAMP DEFAULT NOW()
);
```

## اختبار التطبيق

### قبل النشر:
1. **اختبار محلي**
   ```bash
   npm run build
   npm run preview
   ```

2. **فحص الروابط**
   - تأكد من عمل جميع الصفحات
   - اختبر تسجيل الدخول
   - اختبر المساعد الذكي

3. **اختبار الأداء**
   - استخدم Chrome DevTools
   - فحص سرعة التحميل
   - اختبار على أجهزة مختلفة

## نصائح مهمة

### الأمان:
- لا تشارك متغيرات البيئة الحقيقية
- استخدم HTTPS دائماً
- فعل CORS بشكل صحيح

### الأداء:
- ضغط الصور قبل الرفع
- استخدم CDN للملفات الثابتة
- فعل التخزين المؤقت

### المراقبة:
- راقب استخدام قاعدة البيانات
- تتبع الأخطاء باستخدام Sentry
- راقب أداء الخادم

## الدعم الفني
في حالة مواجهة مشاكل:
1. تحقق من logs الخادم
2. تأكد من صحة متغيرات البيئة
3. اختبر الاتصال بقاعدة البيانات
4. تواصل مع فريق الدعم

