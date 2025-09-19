---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-26
summary: All the people I know
---

```dataview
TABLE WITHOUT ID
file.link AS "Name", summary
FROM !"Templates"
WHERE icontains(tags, "#type/person")
```