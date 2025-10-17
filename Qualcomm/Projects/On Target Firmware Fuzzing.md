---
aliases:
  - QCOM Firmware Fuzzing
tags:
  - "#qpsi/project"
  - "#firmware-security"
  - "#type/project"
status: on-hold
up: "[[Qualcomm Project MoC]]"
summary: Fuzzing SoC Firmware
related:
  - "[[Snapshot Fuzzing using QEMU]]"
created-date: 2024-09-25
---

## Objective
> What were you trying to achieve with this project.

- Fuzzing Qualcomm firmware like
	1. Hypervisor
	2. DSP
	3. Venues


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
