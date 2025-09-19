---
up: "[[Qualcomm Project MoC]]"
related: 
aliases: 
created-date: {{DATE:YYYY-MM-DDTHH:mm}}
status: todo
tags:
  - "#type/project"
  - "#status/active"
summary: 
---

> **Summary**:: 

## Sub Projects

```dataview
TABLE WITHOUT ID
	file.link as "Sub Projects",
	up, created-date, summary, tags
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
WHERE icontains(related, this.file.link)
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