---
tags:
  - injection
---
---

## 🎯 يعني إيه SQL Injection؟

هجوم SQL Injection بيحصل لما حد يحقن (يدخل) كود SQL من خلال بيانات بيبعتها للسيستم (زي فورم تسجيل دخول).  
لو الهجوم نجح، الهاكر يقدر:

- يقرأ بيانات حساسة من قاعدة البيانات 💾
- يعدل أو يمسح بيانات 🧨
- يشغل أوامر إدارية زي إنه يقفل قاعدة البيانات ⚠️
- يوصل لملفات في السيرفر 📂
- وفي بعض الحالات، يشغل أوامر في نظام التشغيل نفسه 😵‍💫

النوع ده من الهجمات بيعتبر نوع من **Injection Attacks**، لأن أوامر SQL بتدخل مكان البيانات اللي المفروض السيستم يتعامل معاها بس كـ input.

---

## 🔎 تهديدات SQL Injection

الهجوم ده ممكن يخلي الهاكر:

- **ينتحل شخصية** (spoof identity)
- **يلعب في الداتا** (tamper with data)
- **يغير تراكنات العمليات** (repudiation)
- **يكشف كل الداتا** (data disclosure)
- **يدمر البيانات** أو يخليها مش متاحة
- **ياخد صلاحيات أدمين** على قاعدة البيانات

الـ SQLi شائع جدًا في تطبيقات PHP وASP علشان بيستخدموا طرق قديمة في التعامل مع قواعد البيانات.  
لكن في J2EE وASP.NET بيكون الموضوع أصعب شوية، بسبب الطرق الحديثة في التعامل مع الـ database.

**مدى خطورة الهجوم بيتوقف على خبرة الهاكر، وكمان على إن كان في طبقات حماية أو لا.**  
بس بشكل عام، الهجوم ده **خطير جدًا** 😨.

---

## 🧰 أنشطة الأمان المرتبطة بالثغرة
 
### إزاي تتجنب SQL Injection:

- شوف [OWASP SQL Injection Prevention Cheat Sheet](https://owasp.org/www-community/attacks/SQL_Injection)
    

### إزاي تراجع الكود وتكشف الثغرة:

- شوف [OWASP Code Review Guide](https://owasp.org/www-project-code-review-guide/)
    

### إزاي تختبر وجود الثغرة:

- شوف [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/) 
 
### إزاي تتخطى الجدران النارية WAF:

- شوف [OWASP Article on SQLi Bypass WAF](https://owasp.org/www-community/attacks/SQL_Injection_Bypassing_WAF)

---

## 🧨 إمتى بتحصل؟

لما:

1. الداتا اللي داخلة للبرنامج بتيجي من مصدر مش موثوق فيه.
2. الداتا دي بتُستخدم في تكوين كويري SQL ديناميكي.

---

## 👀 النتائج:

- **الخصوصية**: ممكن الداتا السرية تتسرب.
- **المصادقة**: ممكن الهاكر يدخل كيوزر تاني.
- **الصلاحيات**: يقدر يغيّر صلاحيات يوزر أو يزود صلاحياته.
- **السلامة**: يقدر يعدل أو يمسح الداتا.

---

## ⚠️ عوامل الخطورة

- اللغة: SQL
- المنصة: أي نظام بيستخدم SQL

SQL Injection شائع جدًا علشان سهل يتعرف ويتنفذ، وأي موقع ليه عدد مستخدمين ممكن يتعرض ليه.

السبب في الثغرة دي إن لغة SQL مش بتفرق بين الكود والداتا في الكويري.

---

## 🧪 أمثلة

### 💥 مثال 1:

```sql
SELECT id, firstname, lastname FROM authors WHERE firstname = 'evil'ex' AND lastname ='newman'
```

❌ هيحصل Error بسبب وجود `'` زيادة.

### ✅ الحل في Java:

```java
String query = "SELECT id, firstname, lastname FROM authors WHERE firstname = ? and lastname = ?";
PreparedStatement pstmt = connection.prepareStatement(query);
pstmt.setString(1, firstname);
pstmt.setString(2, lastname);
```

---

### 💥 مثال 2 (C#):

```csharp
string query = "SELECT * FROM items WHERE owner = '" + userName + "' AND itemname = '" + ItemName.Text + "'";
```

لو الهاكر كتب:

```sql
name' OR 'a'='a
```

الكويري هيبقى:

```sql
SELECT * FROM items WHERE owner = 'wiley' AND itemname = 'name' OR 'a'='a';
```

يعني كل الصفوف هتترجع، ومفيش تحقق من اليوزر 😨.

---

### 💥 مثال 3 (تدمير داتا):

الهاكر يدخل:

```sql
name'); DELETE FROM items; -- 
```

الكويري يبقى:

```sql
SELECT * FROM items WHERE owner = 'hacker' AND itemname = 'name';
DELETE FROM items;
--'
```

يعني كل البيانات اللي في جدول `items` هتتمسح 😰.

---

## 🧱 طرق الحماية:

1. ✅ **Parameterized Queries**: استخدم الاستعلامات الجاهزة زي PreparedStatement. 
2. ✅ **Input Validation**: قبل ما تستخدم أي داتا، اتأكد إنها في فورمات مظبوط. 
3. ❌ **Deny-List**: ما تعتمدش على إنك تمنع رموز معينة بس. دي طريقة ضعيفة.
4. ⚠️ **Stored Procedures**: ممكن تقلل الخطر بس مش بتحمي 100%.


---

## 🔁 هجمات مرتبطة:

- **Blind SQL Injection*  
- **WAF Bypass**
- **Double Encoding**
- **Code Injection**
- **ORM Injection**

---

## 🧠 مصادر خارجية:

- SQL Injection Knowledge Base (MySQL, MSSQL, Oracle)
- GreenSQL (Open Source DB Firewall)
- مقال: "Intro to SQL Injection for Oracle Devs"


---

