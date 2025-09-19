---
up: "[[Reading MoC]]"
tags:
  - "#type/reading/book"
author: " Tom Shanley, Don Anderson, Ravi Budruk"
created-date: 2025-01-02
publisher: Addison-Wesley Professional
published-date: 2003
status: todo
summary:
---

## Reference Note

1. Communication Model
	1. A root complex can communicate with an endpoint. 
	2. An endpoint can communicate with a root complex. 
	3. An endpoint can communicate with another endpoint

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
