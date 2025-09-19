---
up: "[[MoC Planet]]"
related:
  - "[[Qualcomm MoC]]"
tags:
  - "#type/MoC"
created-date: 2024-12-14
summary: All the Project going on in Qualcomm
---

## Pinned Docs

```dataview
TABLE WITHOUT ID
	file.link as "Sub Project", summary, status, created-date AS Date
FROM !"Templates"
WHERE icontains(up, this.file.link)
AND icontains(tags, "type/pinned-docs")
SORT filename DESC
```

### Projects In-Progress

```dataview
TABLE WITHOUT ID
	file.link as "Project",
	summary,
	tags,
	created-date
FROM !"Templates"
WHERE icontains(tags, "type/project") 
	AND icontains(tags, "qpsi/project")
	AND icontains(status, "in-progress")
SORT file.name
```

### Projects On-Hold

```dataview
TABLE WITHOUT ID
	file.link as "Project",
	summary,
	tags,
	created-date
FROM !"Templates"
WHERE icontains(tags, "type/project") 
	AND icontains(tags, "qpsi/project")
	AND icontains(status, "on-hold")
SORT file.name
```

### Closed Projects

```dataview
TABLE WITHOUT ID
	file.link as "Sub Project",
	summary, up, created-date, tags
FROM !"Templates"
WHERE (icontains(tags, "type/project") OR icontains(tags, "type/sub-project"))
	AND icontains(tags, "qpsi/project")
	AND icontains(status, "done")
SORT file.name
```