---
up: "[[Qualcomm Project MoC]]"
related: "[[Linux kernel coverage reporting using GCOV]]"
aliases:
  - Linux Kernel Fuzzing
created-date: 2024-12-14
status: in-progress
tags:
  - "#type/project"
  - "#status/active"
  - "#qpsi/project"
summary: Fuzzing Qualcomm android drivers
---

> **Summary**:: 

## Sub Projects

```dataview
TABLE WITHOUT ID
	file.link as "Sub Project", summary, status, created-date AS Date
FROM !"Templates"
WHERE icontains(up, this.file.link)
AND icontains(tags, "type/sub-project")
SORT filename DESC
```


## Meetings or Reports

```dataview
TABLE WITHOUT ID
	file.link as "Session",summary,
	created-date, participants , tags, up
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
WHERE icontains(text, this.file.NAME)
AND (icontains(tags, "#task") OR icontains(text, "#questions"))
AND done
GROUP BY file.name as filename
SORT filename DESC
```

## Sub Projects Archived

```dataview
TABLE WITHOUT ID
	file.link as "Sub Project", summary, status, created-date AS Date
FROM !"Templates"
WHERE icontains(up, this.file.link)
AND icontains(tags, "type/sub-project")
AND (status = "archived" or status = "done")
SORT filename DESC
```