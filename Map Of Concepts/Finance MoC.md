---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-23
summary: All the finance related stuff
---
```dataview
TABLE summary as Summary, file.mtime as "Last Modified"
FROM #type/finance and -"Templates"
SORT file.name
```
