---
tags:
  - SecurityTools
---
---
# 🚀 Nmap Cheat Sheet by Biso ✨

---

[[Nmap-Cheat-Sheet.pdf]]

---

## 🛡️ إيه هو Nmap؟
**Nmap = Network Mapper**
أداة مفتوحة المصدر لمسح الشبكات والـ **IPs**، هدفها تعرف:
- الأجهزة المتوصلة 💻
- البورتات المفتوحة 🚪
- الخدمات والإصدارات 🛠️
- نوع النظام **(Windows/Linux)** 🎭
- الثغرات باستخدام سكربتات **NSE** 😈 

---
>عشان تعرف ال Option اللي موجوده علي ال nmap 

![[12.png]]

---

## 🎯 أهم أوامر Nmap بشكل مختصر ورايق:

| الاستخدام 🔥              | الأمر                            | وظيفته ببساطة            |
| ------------------------- | -------------------------------- | ------------------------ |
| فحص جهاز وبورتاته         | `nmap 192.168.1.10`              | Scan سريع                |
| فحص مع الخدمات والإصدارات | `nmap -sV 192.168.1.10`          | يحدد نوع وإصدار كل خدمة  |
| فحص شبكة كاملة            | `nmap -sn 192.168.1.0/24`        | يشوف مين شغال فالشبكة    |
| فحص كامل شامل             | `nmap -A example.com`            | يشمل كل حاجة 💣          |
| فحص UDP                   | `nmap -sU -p 53 192.168.1.10`    | مثال لفحص DNS            |
| استخدام سكربتات ثغرات     | `nmap --script=vuln example.com` | يكشف الثغرات المعروفة 😈 |

---

## 💡 نصايح سريعة:
✅ لو بتفحص جهاز واحد استخدم: `-sS -sV -O`  
✅ لو فيه فايروول استخدم: `-Pn` أو `-D RND:5`  
✅ سرعة مع تركيز؟ استخدم: `-T4`  
✅ فحص أهم البورتات بسرعة: `--top-ports 100`  
✅ عايز كل البورتات؟ `-p-`

---
###### 🧨 فحص بورتات محددة:

> [!warning]  
> عايز تفحص بورت معين؟ 🧐  
> `nmap -p80 example.com`
> 
> أكتر من بورت مع بعض:  
> `nmap -p22,80,443 example.com`
> 
> Range من بورت كذا لكذا:  
> `nmap -p1-100 example.com`
> 
> أسرع فحص لأشهر البورتات:  
> `nmap -F example.com`
> 
> تحدد عدد أشهر البورتات:  
> `nmap --top-ports 50 example.com`
> عشان تعمل scan بس في الخفاء علي بورت معين 
> 
`nmap -sT -p80 192.168.8.1/255`


---

![[13.png]]

---
![[14.png]]

---
![[15.png]]

---

## 🎭 الكشف عن الأجهزة في الشبكة:

|الأمر|وظيفته|
|---|---|
|`nmap -sn 192.168.1.0/24`|يشوف مين شغال فالشبكة 🖥️|
|`nmap -sn 192.168.1.1-20`|Range محدد من IPs|

###### 🧠 مثال عملي:

```bash
nmap -sn 192.168.1.0/24
namp 192.168.8.1/24
```

---
>[!Warning]
>عشان تعرف عناوين ال ip اللي علي نفس الشبكه هتستخدم الامر ده 
>`nmap -sn 192.168.8.1/24 `
>`nmap -sn 192.168.8.1-10`
>اللي تحت ده دا range ال Ip هات عشان متشككش حد بيراقب الشبكه 
>
>
---

----

![[16.png]]

---

## 🔥 Scan كامل بالتدرج:

1️⃣ **Ping Scan:**

```bash
nmap -sn 192.168.1.1
```

2️⃣ **فحص كل البورتات:**

```bash
nmap -sS -p- 192.168.1.1
```

3️⃣ **كشف الخدمات والإصدارات:**

```bash
nmap -sV -sC -p22,80,443 192.168.1.1
```

4️⃣ **كشف النظام:**

```bash
nmap -O 192.168.1.1
```

5️⃣ **Scan شامل:**

```bash
nmap -sS -sV -sC -O -p- -T4 -v 192.168.1.1
```

6️⃣ **فحص ثغرات:**

```bash
nmap --script vuln -p22,80 192.168.1.1
```

---
![[17.png]]

---

## ⚡️ السرعة في الـ Nmap:

|الخيار|المعنى|تستخدمه إمتى؟|
|---|---|---|
|`-T0`|بطيء جدًا جدًا|تخطي مراقبة قوية|
|`-T2`|بطيء محترم|السيرفر قديم أو خايف من detection|
|`-T4`|سريع ذكي|أفضل خيار للـ Pentest & CTF|
|`-T5`|أسرع حاجة|شبكتك المحلية أو اختبار داخلي|

###### ⚠️ نصيحة:

> دايمًا في بيئة حقيقية استخدم `-T2` أو `-T4`



---

## 🎯 أهم سكربتات الـ NSE المفيدة:

| السكربت             | وظيفته                         | أمر مثال                                             |
| ------------------- | ------------------------------ | ---------------------------------------------------- |
| `http-title`        | يطلع عنوان الصفحة              | `nmap --script=http-title -p80 example.com`          |
| `http-enum`         | يكشف صفحات مخفية               | `nmap --script=http-enum -p80 example.com`           |
| `ftp-anon`          | هل فيه FTP Anonymous؟          | `nmap --script=ftp-anon -p21 192.168.1.10`           |
| `ssh-brute`         | تجربة كلمات سر على SSH         | `nmap --script=ssh-brute -p22 example.com`           |
| `smb-enum-shares`   | يشوف الـ Shares في SMB         | `nmap --script=smb-enum-shares -p445 192.168.1.10`   |
| `smb-vuln-ms17-010` | يفحص ثغرة EternalBlue          | `nmap --script=smb-vuln-ms17-010 -p445 192.168.1.10` |
| `vulners`           | يطلع الثغرات المرتبطة بالخدمات | `nmap --script=vulners example.com`                  |

###### 💡 تجمع أكتر من سكربت:

```bash
nmap --script=http-title,http-methods,http-enum -p80 example.com
```

---
## 🤖 لو عايز تطبع كل السكربتات الموجودة:

```bash
ls /usr/share/nmap/scripts
```

أو:

```bash
nmap --script-help <script-name>
```

---
```bash 
remendo@kali /u/s/n/scripts> sudo nmap -sV  --script "vulners" 192.168.8.1
# يطلع الثغرات المرتبطة 
```

![[20.png]]


---
# عشان بقي نتجاوز جدران الحمايه ال ids 
---

`sudo nmap -sS -F -D RAND:3 targete.com `

---

## 🎭 يعني إيه Nmap -D ؟

الـ `-D` option في Nmap بيخليك تستخدم **Decoys** يعني أجهزة أو IPs وهمية تظهر في اللوجات بتاعت السيرفر اللي إنت بتمسحه، فيتهيأليهم إن فيه أكتر من حد بيعمل Scan، وإنت متداري وسط الزحمة 😎

---

## 💣 إزاي تستخدمها؟

### الشكل الأساسي:

```bash
nmap -D <Decoy1>,<Decoy2>,<Decoy3> <Target>
```

إنت بتحدد IPs مزيفة أو حقيقية تظهر في المسح، والسيرفر اللي بتمسحه يشوفهم كلهم في اللوجات.

---

## 🎭 التخفي وتضليل اللوجات:

|الأمر|المعنى|
|---|---|
|`nmap -D RND:3 example.com`|يولد 3 IPs مزيفة تظهر في اللوجات|
|`nmap -D 1.2.3.4,5.6.7.8 example.com`|تحدد IPs مزيفة بنفسك|

###### ⚠️ ملاحظة:

> Decoy بيخدع اللوج مش بيغير الـ IP الحقيقي، التتبع الخارجي مازال ممكن.

---

## 🔥 سناريو سريع لبنتست احترافي:

```bash
nmap -sS -p- --open -T4 example.com -oN open_ports.txt
```

```bash
nmap -sV -sC -p80,443 example.com -oN services.txt
```

```bash
nmap --script=http-enum -p8080 example.com
```

![[]]

---
## ✅ الخلاصة:
- دايمًا ابدأ بـ Ping Scan
- بعدين فحص بورتات بالكامل
- كشف الخدمات والإصدارات
- استخدام سكربتات مخصصة حسب الحاجة
- استخدم سرعات مختلفة حسب الوضع
- التخفي مع `-D` لو خايف من اللوجات
---

## 🐚 اسكربت Bash :

```bash
#!/bin/bash

read -p "🎯 Enter the Target (IP or Domain): " target
output_file="scan_$target.txt"

echo "[*] Starting Ping Scan..."
nmap -sn "$target" | tee -a "$output_file"
echo "[*] Running Full TCP Port Scan..."
nmap -sS -p- -T4 "$target" | tee -a "$output_file"
echo "[*] Extracting Open Ports..."
open_ports=$(nmap -p- --min-rate=1000 "$target" | grep ^[0-9] | cut -d '/' -f 1 | tr '\n' ',' | sed 's/,$//')
if [[ -n "$open_ports" ]]; then
  echo "[*] Open Ports Found: $open_ports"
  echo "[*] Running Service & Version Detection..."
  nmap -sV -sC -p "$open_ports" "$target" | tee -a "$output_file"
  echo "[*] Running OS Detection..."
  nmap -O "$target" | tee -a "$output_file"
  echo "[*] Running Vulnerability Scan on Open Ports..."
  nmap --script vuln -p "$open_ports" "$target" | tee -a "$output_file"
else
  echo "[!] No Open Ports Detected!"
fi
echo "[✔] Scan Completed. Results saved in: $output_file"

```


---



---

---
<h1><center> مهم فشخ </center></h1>

---

# 🛠️ **منهجية فحص هدف باستخدام Nmap**

---

## 🔍 **المرحلة 1: التأكد من وصول الشبكة - Ping Sweep**

### الأمر:

```bash
nmap -sn example.com
```

✅ **الهدف:** التأكد إن الهدف Online، بدون فحص بورتات، بس كشف إذا الجهاز بيرد ولا لأ.

---

## 🕵️‍♂️ **المرحلة 2: فحص جميع البورتات TCP - Full Port Scan**

### الأمر:

```bash
nmap -sS -p- example.com
```

✅ **الهدف:** تحديد جميع البورتات المفتوحة من 1 لـ 65535 باستخدام TCP SYN Scan (Stealth Scan).

📝 **ملاحظات:**

- `-sS` = TCP SYN Scan، فحص صامت نسبيًا.
- `-p-` = يشمل جميع البورتات.

---

## 🧩 **المرحلة 3: فحص الخدمات والإصدارات - Service Version Detection**

### مثال (بعد ما تحدد البورتات المفتوحة، هنا نفترض بورت 80 مفتوح):

```bash
nmap -sV -sC -p 80 example.com
```

✅ **الهدف:** التعرف على نوع الخدمات وإصداراتها مع تشغيل الـ Default Scripts.

📝 **ملاحظات:**
- `-sV` = إظهار إصدار كل خدمة.
- `-sC` = تشغيل السكربتات الافتراضية من Nmap.
- `-p` = تحديد البورتات اللي هتفحصها.

---

## 🖥️ **المرحلة 4: التعرف على نظام التشغيل - OS Detection**

### الأمر:

```bash
sudo nmap -O example.com
```

✅ **الهدف:** محاولة كشف نظام التشغيل عبر TCP/IP Fingerprinting.

📝 **ملاحظات:**
- يتطلب صلاحيات Root أو Sudo.
- دقة النتائج تعتمد على الاستجابة من الهدف.
    

---

## 🔄 **المرحلة 5: Scan شامل متكامل - Comprehensive Scan**

### الأمر:

```bash
sudo nmap -sS -sV -sC -O -p- -T4 -v example.com
```

✅ **الهدف:** تنفيذ فحص متكامل يحتوي على:

|سويتش|الوظيفة|
|---|---|
|`-sS`|TCP SYN Scan|
|`-sV`|كشف إصدارات الخدمات|
|`-sC`|تشغيل سكربتات افتراضية|
|`-O`|التعرف على نظام التشغيل|
|`-p-`|جميع البورتات|
|`-T4`|سرعة تنفيذ متوسطة مناسبة للـ Pentest|
|`-v`|إظهار معلومات تفصيلية أثناء التنفيذ|

---

## 🛡️ **المرحلة 6: فحص الثغرات باستخدام Nmap Scripting Engine - NSE Vuln Scan**

### الأمر:

```bash
nmap --script vuln -p 80 example.com
```

✅ **الهدف:** استخدام سكربتات كشف الثغرات المعروفة على بورت معين.

📝 **يمكن تعديل الأمر لأكثر من بورت:**

```bash
nmap --script vuln -p 80,443,22 example.com
```

---

## 📂 **المرحلة 7: حفظ النتائج - Output Management**

### حفظ النتائج في ملف نصي عادي:

```bash
nmap -sS -p 80 example.com -oN output.txt
```

### حفظ النتائج في صيغة XML:

```bash
nmap -sS -p 80 example.com -oX output.xml
```

---

# 🎯 **ملاحظات أساسية للفحص الصحيح:**

✅ تأكد من تنفيذ Full Port Scan قبل أي فحص خدمات.  
✅ دائما ابدأ بالأوامر الخفيفة مثل Ping Scan قبل التصعيد في الفحص.  
✅ راقب الـ Firewall والـ IDS إذا لاحظت Blocking أو Drop.  
✅ نتائج OS Detection مش دائمًا دقيقة 100%، اعتمد على أكثر من أداة لو أمكن.  

---

# 💼 **مثال خطة فحص هدف example.com كاملة:**

1. **تأكد من الوصول:**
```bash
nmap -sn example.com
```

2. **فحص كل البورتات:**

```bash
nmap -sS -p- example.com
```

3. **فحص الخدمات والإصدارات (بعد تحديد البورتات المفتوحة):**
```bash
nmap -sV -sC -p 80 example.com
```

4. **تحديد نظام التشغيل:**

```bash
sudo nmap -O example.com
```

5. **سكان شامل متكامل:**

```bash
sudo nmap -sS -sV -sC -O -p- -T4 -v example.com
```

6. **فحص الثغرات على بورت محدد:**

```bash
nmap --script vuln -p 80 example.com
```

7. **حفظ النتائج:**

```bash
nmap -sS -p 80 example.com -oN results.txt
```

---

# ⚙️ **مراحل إضافية للفحص المتقدم باستخدام Nmap**

---

## 🌐 فحص بورتات UDP - UDP Port Scan

✅ الهدف: اكتشاف الخدمات اللي شغالة على بروتوكول UDP.

### الأمر الأساسي:

```bash
sudo nmap -sU -p- example.com
```

📝 **ملاحظات:**

| سويتش | معناه                     |
| ----- | ------------------------- |
| `-sU` | فحص بورتات UDP            |
| `-p-` | كل البورتات من 1 لـ 65535 |

⚠️ فحص UDP أبطأ بكتير من TCP، استخدمه بحذر مع ضبط سرعة لو الشبكة تستحمل:

```bash
sudo nmap -sU -T4 -p- example.com
```

---

## 🎯 **فحص مشترك TCP و UDP**

✅ الهدف: سكان شامل يجمع البروتوكولين مع بعض.

```bash
sudo nmap -sS -sU -p T:1-65535,U:1-65535 example.com
```

📝 **معاني الأوامر:**

- `-sS` = TCP SYN Scan
- `-sU` = UDP Scan
- `-p T:…` = تحديد بورتات TCP
- `-p U:…` = تحديد بورتات UDP
    

---

## 🛠️ **تفصيل إضافي في Nmap Scripting Engine - NSE Advanced**

✅ الهدف: استخدام سكربتات محددة للفحص الدقيق مش بس vuln.
### أمثلة مهمة:

1. **اكتشاف الثغرات العامة:**

```bash
nmap --script vuln -p 80 example.com
```

2. **فحص DNS بالتفصيل:**
```bash
nmap --script dns-brute example.com
```

3. **فحص HTTP بشكل شامل:**

```bash
nmap --script http-enum -p 80 example.com
```

4. **استخراج معلومات SSL/TLS:** 

```bash
nmap --script ssl-enum-ciphers -p 443 example.com
```

5. **البحث عن الـ SMB والثغرات المرتبطة بيه:**
    
```bash
nmap --script smb-vuln* -p 445 example.com
```

---

## 🗂️ **تصدير نتايج الفحص - Output Options**

---

✅ الهدف: تحافظ على تنظيم النتايج وتحليلها بعدين.

| صيغة     | الأمر               | الاستخدام                       |
| -------- | ------------------- | ------------------------------- |
| Normal   | `-oN results.txt`   | قراءة بسيطة                     |
| XML      | `-oX results.xml`   | تحليل أو استيراد في أدوات تانية |
| JSON     | `-oJ results.json`  | استخدام مرن مع لغات برمجة       |
| Grepable | `-oG results.gnmap` | مناسب للفلترة في الـ Terminal   |

**مثال:** تصدير أكثر من صيغة مع بعض:

```bash
nmap -sS -p 80 example.com -oN results.txt -oX results.xml -oG results.gnmap
```

---

## 🚀 **خطوات إضافية في خطة الفحص الاحترافية - Advanced Workflow**

✅ لو الهدف شبكة أو دومين كبير، قسم الشغل بالشكل التالي:

### 1. تحديد النطاق:

```bash
nmap -sn 192.168.1.0/24
```

### 2. جمع الأهداف في ملف:

```bash
nmap -sn 192.168.1.0/24 -oG live_hosts.gnmap
grep "Up" live_hosts.gnmap | cut -d " " -f 2 > targets.txt
```

### 3. سكان جماعي بناءً على Targets:

```bash
nmap -sS -iL targets.txt -p- -oN ports_scan.txt
```

### 4. استهداف خدمات محددة بناءً على النتايج:

```bash
nmap -sV -sC -iL targets.txt -p 22,80,443 -oN services_scan.txt
```

---

# 🎯 **ملاحظات ختامية للتوسيع الاحترافي**

✅ دايمًا:
- ابدأ بفحص خفيف (Ping, Discovery)
- فحص شامل للبورتات (TCP & UDP)
- تحديد الخدمات والإصدارات
- تشغيل NSE Scripts ذكية
- تصدير النتايج وتحليلها بهدوء
- تجنب الفحص العنيف لو في Monitoring أو IDS

---
