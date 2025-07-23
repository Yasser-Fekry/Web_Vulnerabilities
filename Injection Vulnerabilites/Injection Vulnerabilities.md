---
tags: []
---
----
```dataview
list
where contains(file.tags, "#injection")
sort number(regexreplace(file.name, "^([0-9]+)-.*", "$1")) asc
```
---
