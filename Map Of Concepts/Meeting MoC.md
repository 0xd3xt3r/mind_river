---
up: "[[MoC Planet]]"
created-date: 2024-12-14
tags:
  - "#type/MoC"
summary: All the Meetings in the World
---

```dataview
TABLE WITHOUT ID
file.link AS "Meeting", summary, participants, created-date
FROM !"Templates"
WHERE icontains(tags, "#type/meeting")
SORT created-date DESC
```