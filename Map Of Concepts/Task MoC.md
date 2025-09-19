---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-14
summary: All the task in the World
---

## Open

```dataview
TASK 
WHERE icontains(text, "#task")
AND !completed
GROUP BY file.name as filename
SORT filename DESC
```

## Closed

```dataview
TASK 
WHERE icontains(text, "#task")
AND completed
GROUP BY file.name as filename
SORT filename DESC
```