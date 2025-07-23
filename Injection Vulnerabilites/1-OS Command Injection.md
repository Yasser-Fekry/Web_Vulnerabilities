---
tags:
  - injection
---
---

# 🎯 **Command Injection – حقن الأوامر**
---

## 💡 **الوصف**

هجوم Command Injection هو نوع من أنواع الهجمات الهدف منه إن المهاجم يشغل أوامر على نظام التشغيل نفسه (OS) عن طريق تطبيق فيه ثغرة.

الثغرة بتحصل لما التطبيق يبعت بيانات المستخدم (زي الفورم، الكوكيز، أو الـ HTTP headers) بشكل غير آمن للـ shell بتاع النظام (زي /bin/sh).  
في الحالة دي، الهاكر يقدر يضيف أوامر للنظام تتنفذ بسطحية جدًا، وكمان ممكن تتنفذ بصلاحيات التطبيق اللي فيه الثغرة.

الثغرة دي بتحصل غالبًا بسبب إن مفيش تحقق كافي من البيانات اللي جاية من المستخدم.

الفرق بين Command Injection و Code Injection؟

- Code Injection: المهاجم بيضيف كود يشتغل جوه التطبيق نفسه.
- Command Injection: المهاجم بيوسّع وظيفة التطبيق وبيخليه يشغّل أوامر على النظام، من غير ما يحقن كود جديد.

---

## 🧪 **أمثلة**

### 🔸 **مثال 1:**

كود بيستخدم أمر `cat` عشان يطبع محتوى ملف:

```c
#include <stdio.h>
#include <unistd.h>

int main(int argc, char **argv) {
 char cat[] = "cat ";
 char *command;
 size_t commandLength;

 commandLength = strlen(cat) + strlen(argv[1]) + 1;
 command = (char *) malloc(commandLength);
 strncpy(command, cat, commandLength);
 strncat(command, argv[1], (commandLength - strlen(cat)) );

 system(command);
 return (0);
}
```

📌 الاستخدام العادي:

```bash
$ ./catWrapper Story.txt
When last we left our heroes...
```

لكن لو الهاكر كتب:

```bash
$ ./catWrapper "Story.txt; ls"
```

هيطبع الملف، وبعدين يشغّل أمر `ls` ويعرض كل الملفات! ولو البرنامج بيشتغل بصلاحيات عالية، ممكن يشغّل أوامر خطيرة بنفس الصلاحيات 😨

---

### 🔸 **مثال 2:**

برنامج بسيط بياخد اسم ملف من المستخدم ويعرض محتواه. والبرنامج معمول إنه يشتغل بـ `setuid root`.

```c
int main(char* argc, char** argv) {
 char cmd[CMD_MAX] = "/usr/bin/cat ";
 strcat(cmd, argv[1]);
 system(cmd);
}
```

لو الهاكر كتب:

```
;rm -rf /
```

البرنامج بدل ما يشغل cat، هيشغّل أمر حذف جذور النظام بالكامل 🤯

---

### 🔸 **مثال 3:**

برنامج بياخد مسار من environment variable اسمه `APPHOME` ويشغّل سكريبت من المسار ده:

```c
char* home=getenv("APPHOME");
char* cmd=(char*)malloc(strlen(home)+strlen(INITCMD));
if (cmd) {
 strcpy(cmd,home);
 strcat(cmd,INITCMD);
 execl(cmd, NULL);
}
```

الهاكر يقدر يعدل قيمة `APPHOME` تخلي البرنامج يشغّل سكريبت ضار بدل السكريبت الحقيقي، ويتنفذ بصلاحيات عالية.

---

### 🔸 **مثال 4:**

برنامج CGI بيشغّل أمر `make` لتحديث الباسورد في `/var/yp`:

```c
system("cd /var/yp && make &> /dev/null");
```

رغم إن الأمر هنا ثابت ومفيهوش إدخال من المستخدم، الهاكر يقدر يعدل متغير البيئة `$PATH` ويحط فيه نسخة خبيثة من `make`.  
وبما إن البرنامج `setuid root`، النسخة دي هتشتغل كـ root 😬

---

### 🔸 **معلومة جانبية عن Java:**

ناس كتير بتقول إن `Runtime.exec` في Java زي `system()` في C، وده **مش صح**.

- `system()` بيبعت الأمر للشل بالكامل، يعني تقدر تكتب `;`, `|`, `&&`, إلخ.
    
- أما `Runtime.exec()` في Java، بيفصل الأمر لكلمات وبيشغّل أول واحدة بس كأمر والباقي كـ args.
    

يعني الحاجات اللي بتشتغل في الشل مش هتشتغل هنا بنفس الطريقة.

---

### 🔸 **مثال 5:**

#### ✅ C:

```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>

int main(int argc, char **argv)
{
     char command[256];

     if(argc != 2) {
          printf("Error: Please enter a program to time!\n");
          return -1;
     }

     memset(&command, 0, sizeof(command));
     strcat(command, "time ./");
     strcat(command, argv[1]);

     system(command);
     return 0;
}
```

لو حد كتب:

```
ls; cat /etc/shadow
```

هيعرض الملفات وبعدين يعرض بيانات حساسة جدًا.

---

#### ✅ Java:

نفس النقطة:

- `Runtime.exec()` مش زي `system()`،
    
- مش بيشغّل الشل، فـ الأوامر المركبة أو redirect مش هتشتغل.
    

---

### 🔸 **مثال 6: PHP**

```php
<?php
print("Please specify the name of the file to delete");
print("<p>");
$file=$_GET['filename'];
system("rm $file");
?>
```

طلب URL زي ده:

```
http://127.0.0.1/delete.php?filename=bob.txt;id
```

هينفذ `rm bob.txt` وبعدين `id`:

**الرد هيكون:**

```
Please specify the name of the file to delete
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

---

## 🧼 **تنضيف الإدخال (Sanitizing Input)**

- امنع الرموز الخطيرة زي `;`, `&&`, `|`
- استخدم whitelist للحروف المسموحة
- ابعد عن تنفيذ أوامر بالنص

---

## ✅ **الحماية (Related Controls)**

- لو في API موجود للوظيفة، استخدمه بدل shell.  
    مثال:  
    في Java استخدم `javax.mail.*` بدل `Runtime.exec("mail")`.
- لو مضطر تستخدم أمر، لازم تنظف كل input من المستخدم كوي جدًا.

---

