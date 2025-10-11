---
up: "[[Qualcomm Project MoC]]"
aliases:
  - WoS Fuzzing
tags:
  - "#type/project"
  - "#qpsi/project"
status: on-hold
created-date: 2024-12-14
summary: Fuzzing Windows Kernel Driver in WoS Laptops
---

> **Summary**:: 

## Pinned Docs

```dataview
TABLE WITHOUT ID
	file.link as "Sub Project", summary, created-date AS Date
FROM !"Templates"
WHERE icontains(up, this.file.link)
AND icontains(tags, "type/pinned-docs")
SORT created-date DESC
```

## Sub Projects

```dataview
TABLE WITHOUT ID
	file.link as "Sub Projects", status, summary , created-date
FROM !"Templates"
WHERE icontains(up, this.file.link)
AND icontains(tags, "type/sub-project")
SORT created-date DESC
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
AND !done
GROUP BY file.name as filename
SORT filename DESC
```

### Completed Task

```dataview
TASK
WHERE icontains(text, this.file.name)
AND (icontains(tags, "#task") OR icontains(text, "#questions"))
AND done
GROUP BY file.name as filename
SORT filename DESC
```
