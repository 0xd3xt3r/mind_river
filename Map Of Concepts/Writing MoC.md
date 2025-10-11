---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-14
summary: Writing everything down
---

## Blogpost

```dataview
TABLE created-date, status 
FROM #type/writing/article AND -"Templates"
SORT created-date DESC
```

## Social Media Post

```dataview
TABLE created-date, status 
FROM #type/writing/social-media and -"Templates"
WHERE status = "todo"
SORT created-date DESC
```

```dataview
TABLE created-date, status 
FROM #type/writing/social-media
SORT filename
SORT date
WHERE status = "published"
LIMIT 10
```

## Books

[[Reverse Engineers Guide to ARM SoC|Advenctures of SoC]]