---
up: "[[Reading MoC]]"
related: 
tags:
  - "#type/reading/book"
title:
author: 
created-date: 2026-01-26 
status: todo
publisher:
published-date:
summary:
---

## Reference Note

1. This book is an excellent guide for middle managers. What is does the terrain looks like and what are the rules of the game.

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
