---
up: "[[Reading MoC]]"
tags:
  - "#type/reading/book"
author: Dominik Merli
created-date: 2024-12-23
status: todo
related: 
completed-date: 
summary:
---

## Reference Note

1. 

---

## Log

```dataview
TASK
WHERE (icontains(text, this.file.name) AND icontains(text, "#log/sprint")) AND 
		!(icontains(text, "#task") OR icontains(text, "#question"))
	AND !completed
GROUP BY file.name as Filename
SORT fows.file.ctime DESC
LIMIT 20
```
