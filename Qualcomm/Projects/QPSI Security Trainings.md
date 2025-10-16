---
up: "[[Qualcomm Project MoC]]"
related:
  - "[[Qualcomm MoC]]"
status: in-progress
tags:
  - "#qpsi/project"
  - type/project
created-date: 2024-12-14
summary: Security Training in QIPL
---

> **Summary**:: 

## Sub Projects

```base
filters:
  and:
    - file.hasTag("type/sub-project")
    - file.hasTag("qpsi/project")
    - file.hasLink(this.file)
properties:
  note.summary:
    displayName: Summary
  file.name:
    displayName: File name
views:
  - type: table
    name: Sub-Projects
    order:
      - file.name
      - summary
      - status
      - file.mtime
    sort:
      - property: file.mtime
        direction: DESC
    limit: 10
    columnSize:
      file.name: 386
      note.summary: 866
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
