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

```base
filters:
  and:
    - file.hasTag("type/pinned-docs")
properties:
  note.summary:
    displayName: Summary
  file.name:
    displayName: File name
views:
  - type: table
    name: Pinned Docs
    order:
      - file.name
      - summary
      - status
      - file.ctime
    sort:
      - property: summary
        direction: ASC
    limit: 10
    columnSize:
      file.name: 386
      note.summary: 586
      note.status: 190

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