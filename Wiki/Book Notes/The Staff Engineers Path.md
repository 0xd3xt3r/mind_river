---
up: "[[Reading MoC]]"
tags:
  - "#type/reading/book"
title: The Staff Engineers Path
author: Tanya Reilly
status: todo
created-date: 2024-12-14
last-read: 2024-12-16
summary: How to pursuie techinque path in Orgainzation
---

Last Read Chapter :  What Would You Say You Do Here?

## Notes

1. Three pillars: big-picture thinking, project execution, and leveling up
2. The senior level as the *anchor* level for a career ladder. The levels below are for people to grow their autonomy; the levels above increase impact and responsibility. The first rung above senior is often called *staff engineer*.
3. 

---

## Review


---

```dataview
TASK
WHERE (icontains(text, this.file.name)) AND 
		!(icontains(text, "#task") OR icontains(text, "#question"))
	AND !completed
GROUP BY file.name as Filename
SORT fows.file.ctime DESC
LIMIT 20
```