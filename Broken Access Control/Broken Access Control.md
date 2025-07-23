---
tags: []
---
---
```dataview
list
where contains(file.tags, "#brokenAccess")
sort number(regexreplace(file.name, "^([0-9]+)-.*", "$1")) asc
```
---
