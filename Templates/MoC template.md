---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: {{DATE}} 
summary:
---

```dataview
TABLE WITHOUT ID
file.link AS "Name", summary, tags
FROM !"Templates"
WHERE icontains(tags, "#type/")
```