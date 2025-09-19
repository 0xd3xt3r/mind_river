---
up: "[[Personal Project MoC]]"
aliases:
  - Shaman DBI
created-date: 2024-12-16
status: in-progress
tags:
  - "#type/project/personal"
---

> **Summary**:: Shaman is a Dynamic Binary Analysis framework

## Sub Projects

```dataview
TABLE WITHOUT ID
	file.link as "Sub Projects", summary,
	 tags, up, created-date
FROM !"Templates"
WHERE icontains(up, this.file.link)
AND icontains(tags, "type/sub-project")
SORT filename DESC
```

## Meetings

```dataview
TABLE WITHOUT ID
	file.link as "Session",
	up, created-date, participants, summary, tags
FROM !"Templates"
WHERE icontains(up, this.file.link)
AND icontains(tags, "type/meeting")
SORT filename DESC
```

## Tasks and Questions

### ToDo

```dataview
TASK
WHERE icontains(text, this.file.name)
AND (icontains(tags, "#task") OR icontains(text, "#questions"))
AND !completed
GROUP BY file.name as filename
SORT filename DESC
```

### Completed Task

```dataview
TASK
WHERE icontains(text, this.file.name)
AND (icontains(tags, "#task") OR icontains(text, "#questions"))
AND completed
GROUP BY file.name as filename
SORT filename DESC
```

## Sprints

```dataview
TASK
WHERE icontains(text, this.file.name)
AND icontains(text, "#log/sprint")
GROUP BY file.name as filename
SORT filename DESC
```