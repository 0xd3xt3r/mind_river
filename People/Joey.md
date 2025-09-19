---
up: "[[People MoC]]"
full-name: Joey Jiao
alias: Joey Jiao
created-date: 2024-12-17
tags:
  - "#type/person"
summary: China team, Staff engineer
---

## Meetings
```dataview
TABLE summary, tags
FROM #type/meeting
WHERE icontains(participants, this.file.link)
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
