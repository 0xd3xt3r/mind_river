---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-15
summary: All the projects which are personal
---

### Open Projects

```dataview
TABLE WITHOUT ID
	file.link as "Project",
	summary,
	tags,
	created-date
FROM !"Templates"
WHERE icontains(tags, "type/project/personal")
SORT file.name
```

### Closed Projects

```dataview
TABLE WITHOUT ID
	file.link as "Sub Project",
	summary,
	tags,
	up,
	created-date
FROM !"Templates"
WHERE icontains(tags, "type/project/personal")
AND status = "done"
SORT filename DESC
```