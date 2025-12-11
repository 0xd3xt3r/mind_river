---
up: "[[Reading MoC]]"
related: 
tags:
  - "#type/reading/book"
title: The Laws of Human Nature
author: Robert Greene
created-date: 2025-12-08 
status: todo
publisher:
published-date:
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
