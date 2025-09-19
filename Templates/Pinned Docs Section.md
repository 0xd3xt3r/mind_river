## Pinned Docs

```dataview
TABLE WITHOUT ID
	file.link as "Sub Project", summary, status, created-date AS Date
FROM !"Templates"
WHERE icontains(up, this.file.link)
AND icontains(tags, "type/pinned-docs")
SORT filename DESC
```
