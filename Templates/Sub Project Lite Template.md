---
tags:
  - "#type/sub-project"
  - "#status/active"
status: todo
up: 
related: 
created-date : {{DATE}}
summary:
---

> **Summary**:: 

## Notes
> What did you learn?
- 

## Task
> What needs to be done!
- 

## Project Management

### Meetings

```dataview
TABLE WITHOUT ID
	file.link as "Session",
	up, created-date, participants, summary, tags
FROM !"Templates"
WHERE icontains(related, this.file.link)
AND icontains(tags, "type/meeting")
SORT file.link DESC
```

### Tasks and Questions

```dataview
TASK
WHERE icontains(text, this.file.name)
AND (icontains(tags, "#task") OR icontains(text, "#questions"))
AND !completed
GROUP BY file.name as filename
SORT filename DESC
```

### Archived Task
