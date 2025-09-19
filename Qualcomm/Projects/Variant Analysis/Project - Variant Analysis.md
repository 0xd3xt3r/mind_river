---
up: "[[Qualcomm Project MoC]]"
tags:
  - "#type/project"
created-date: 2024-12-16 13:45
status: todo
summary: Using the pattern of existing bug to find new bugs
---

> **Summary**:: 

## Sub Projects

```dataview
TABLE WITHOUT ID
	file.link as "Sub Projects",
	up, created-date, summary, tags
FROM !"Templates"
WHERE icontains(up, this.file.link) OR icontains(related, this.file.link)
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
> Every bug is pattern that can be used to explore and find other bugs of similar nature hiding near newly discovered bug

## Objective

```dataview
TABLE date, status 
FROM #variant-analysis and -"Templates"
sort filename
sort status
SORT date
```