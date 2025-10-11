---
created-date: 2024-12-14
tags:
  - "#type/person"
summary: SD Principle Manager
up: "[[People MoC]]"
full-name: Murali Somanchy
related:
  - "[[Surendra]]"
  - "[[Nagaraju]]"
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
WHERE icontains(text, "#task")
	AND 
	( icontains(text, "#person/nagaraju") OR icontains(text, this.file.name))
	AND completed
GROUP BY file.name as Filename
LIMIT 20
```

## Logs

```dataview
TASK
WHERE icontains(text, this.file.name) AND 
		(!icontains(text, "#task") OR
		  !icontains(text, "#question") OR
		  icontains(text, "#log/nagaraju") OR
		  icontains(text, this.file.name))
	AND completed
GROUP BY file.name as Filename
SORT fows.file.ctime DESC
LIMIT 20
```
