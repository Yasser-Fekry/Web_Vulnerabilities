---
tags: []
---
  ---

![[wallpaperflare.com_wallpaper (7).jpg]]

---
# Important Tool For Tester

```dataview
list
where contains(file.tags, "#SecurityTools")
sort number(regexreplace(file.name, "^([0-9]+)-.*", "$1")) asc
```



---
# Information Disclosure 🔎

```dataview
list
where contains(file.tags, "#InformaionDisclosure")
sort number(regexreplace(file.name, "^([0-9]+)-.*", "$1")) asc
```
---
# Injection Vulnerabilities 💉

```dataview
list
where contains(file.tags, "injection")
sort number(regexreplace(file.name, "^([0-9]+)-.*", "$1")) asc
```
---
# Broke Access Control 🦾

```dataview
list
where contains(file.tags, "brokenAccess")
sort number(regexreplace(file.name, "^([0-9]+)-.*", "$1")) asc
```

---
