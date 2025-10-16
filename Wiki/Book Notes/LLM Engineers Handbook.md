---
up: "[[Reading MoC]]"
related:
tags:
  - "#type/reading/book"
title: LLM Engineer's Handbook
author: 
- Paul Iusztin
- Maxime Labonne
created-date: 2025-10-16
status: todo
publisher: Packt Publisher
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
