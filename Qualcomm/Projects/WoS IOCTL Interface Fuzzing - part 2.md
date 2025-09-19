---
tags:
  - "#type/sub-project"
  - "#status/active"
status: todo
up: "[[WoS Kernel Driver Fuzzing]]"
related: 
created-date: 2025-04-15
---

> **Summary**:: In this project I will be fuzzing specific driver which will get much deeper coverage 


### Tasks and Questions

- [ ] #task Figure out the entry points ➕ 2025-04-15

## Code Cues

1. [CoreIoctlEnqueueEncoderInput](http://win-0222:8080/source/xref/SC8480X.WP_GL.1.0/camera/rel/11.1/CamZ/Drivers/KMD/JpegE/src/JpegQueue.c#802)


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
