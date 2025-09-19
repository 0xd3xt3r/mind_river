---
up: "[[People MoC]]"
created-date: 2024-12-14
aliases:
  - Nagaraju qcom
  - Nagaraju - Sync-up
tags:
  - "#type/person"
summary: Reporting Manager
---

## Meetings
```dataview
TABLE summary, tags
FROM #type/meeting
WHERE icontains(participants, this.file.link)
SORT created-date DESC
LIMIT 20
```
## Tasks and Questions

### Pending

```dataview
TASK
WHERE (icontains(text, "#task") OR icontains(text, "#question"))
	AND icontains(text, this.file.name)
	AND !completed
GROUP BY file.name as Filename
LIMIT 20
```

### Done

```dataview
TASK
WHERE (icontains(text, "#task") OR icontains(text, "#question"))
	AND icontains(text, this.file.name)
	AND completed
GROUP BY file.name as Filename
LIMIT 10
```

## Logs

```dataview
TASK
WHERE icontains(text, this.file.name) AND 
		!(icontains(text, "#task") OR icontains(text, "#question"))
	AND !completed
GROUP BY file.name as Filename
SORT fows.file.ctime DESC
LIMIT 20
```
