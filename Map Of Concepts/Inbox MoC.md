---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-14
summary: All notes is not MoC, people or projects
---

## Inbox
```dataview
TABLE WITHOUT ID
file.link AS "Name", summary, tags
FROM "Inbox"
WHERE !icontains(tags, "#type/person")
AND !icontains(tags, "#type/meeting")
AND !icontains(tags, "#type/project")
AND !icontains(tags, "#type/sub-project")
AND !icontains(tags, "#type/MoC")

```