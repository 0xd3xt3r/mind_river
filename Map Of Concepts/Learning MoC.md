---
up: "[[MoC Planet]]"
related:
  - "[[Reading MoC]]"
tags:
  - "#type/MoC"
created-date: 2024-12-15
summary: This is the page which actively highlights the documents which I am interacting with to learn new stuff.
---
## Books

```dataview
TABLE summary, status, created-date
FROM #type/reading/book and -"Templates"
WHERE status = "reading"
SORT file.mtime DESC
LIMIT 10
```

## Video Streams

```dataview
TABLE summary, status, created-date
FROM #type/video-stream and -"Templates"
SORT created-date
```

## Security Training Courses

```dataview
TABLE summary, status, created-date
FROM #learning/course and -"Templates"
SORT created-date
```
