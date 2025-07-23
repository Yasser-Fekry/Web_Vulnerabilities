---
tags:
  - "#InformaionDisclosure"
---
---

🧐 يعني إيه Information Disclosure؟

ثغرة Information Disclosure معناها إن النظام بيسرّب معلومات حساسة المفروض ما حدش يشوفها، زي:
أسماء مستخدمين، كلمات سر، مفاتيح API، أو مفاتيح تشفير
كود المصدر (Source code) أو ملفات الإعدادات
بيانات شخصية عن المستخدمين (PII)
تفاصيل تقنية ممكن تساعد أي مهاجم

> أسبابها الشائعة:

مفيش فلاتر كويسة على الإدخال/الإخراج
ضعف في الـ Authorization
أخطاء بتظهر للمستخدمين
استخدام مكونات قديمة أو مش مؤمنة



---

💥 التأثير على الشركات

1. خسائر مالية:

تسريب أسرار تجارية أو بيانات عملاء

غرامات قانونية (زي GDPR أو HIPAA)

مصاريف التحقيق، المحامين، إبلاغ المستخدمين

🕵️‍♂️ مثال واقعي: اختراق Basecamp

الهاكر: @neex على HackerOne
الهدف: موقع Basecamp
نوع الثغرة: تسريب بيانات من الذاكرة بسبب مكتبة قديمة اسمها librsvg

📌 اللي حصل:

Basecamp كان بيستخدم مكتبة librsvg قديمة بتتعامل مع صور SVG

الهاكر رفع صورة SVG متصممة بطريقة معينة

لما السيرفر عالج الصورة، حصل تسريب من الذاكرة (Memory Leak)

التسريب ده كشف معلومات زي:

مفاتيح AWS

Cookies لمستخدمين



🔧 الحلول:

سريعة: تحديث مكتبة librsvg

أو منع رفع صور SVG في الـ Avatars

على المدى الطويل: اعزل عملية معالجة الصور جوه Docker من غير إنترنت (Sandbox)

متستهونش بثغرات التسريب (Information Disclosure)، ساعات بتبقى البوابة الرئيسية لاختراق كبير

لو بتعمل اختبار اختراق، دور دايمًا على:

Error messages

ملفات config

بيانات بتظهر في responses أو logs

مكتبات قديمة بتتعامل مع ملفات وصور

---
.INFORMATION DISCLOSURE

---
```bash
git clone https://github.com/epi052/feroxbuster.git
git clone https://github.com/danielmiessler/SecLists/tree/master/Web-Shells
```
## 🎯 ملخص الأداة: **Feroxbuster**
هي أداة قوية وسريعة جدًا لـ **Content Discovery** أو **Directory Bruteforce**، معمولة بلغة Rust (يعني خفيفة وسريعة نار🔥)، هدفها الأساسي إنها تكشف الديركتوريز والفايلات المخفية في مواقع الويب.

---

## 🛠️ ليه بنستخدمها؟
- لما تعمل اختبار اختراق أو "Recon"، دايمًا فيه داتا مخفية زي:  
    - **Folders** مش ظاهرين في النفيجيشن بتاع الموقع.
    - **Admin Panels** أو صفحات Debug/Test مخفية.

- فايلات زي `.git`، `.env`، أو Backup فايلات.  
- الأداة دي بتخليك:  
-
✅ تكتشف الحاجات دي أوتوماتيك وبسرعة.  
✅ تستخدم Wordlists عشان تجرب ملايين الاحتمالات.  
✅ تكشف حاجات لو لقيتها ممكن تستغلها زي: 
صفحة Admin مش محمية
فايلات Backup فيها Credentials   
ديركتوري Open Listing فيه داتا.

---

## ⚡ تأثيرها على المواقع:
- على الموقع من ناحية الـ **Security**:
- بتكشف نقاط ضعف زي **Information Disclosure**.
- لو في داتا مش محمية كويس، بتطلعها للنور
- أوقات ممكن تساعدك توصل لـ ثغرات أكبر زي:
- Unauthenticated Access.
- File Inclusion

---

## 💻 مثال سريع للاستخدام:

```bash
feroxbuster -u http://target.com -w  /path/test.txt
```

```bash
feroxbuster -u https://target.com -w wordlist.txt -t 10 --delay 200





```


# 0-Sub Domain Tools
[اضغط هنا](#0-sub-domain-tools)



