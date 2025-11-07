---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-15
summary: Everything Related to things I have Read
---

```base
filters:
  and:
    - file.hasTag("type/reading/book")
    - not:
        - file.inFolder("Templates")
properties:
  note.summary:
    displayName: Summary
  file.name:
    displayName: File name
views:
  - type: table
    name: Reading
    filters:
      and:
        - status == "reading"
    order:
      - file.name
      - summary
      - author
    sort:
      - property: file.mtime
        direction: DESC
    columnSize:
      file.name: 340
      note.summary: 700
      note.author: 348
      note.status: 190
  - type: table
    name: Todo
    filters:
      and:
        - status == "todo"
    order:
      - file.name
      - summary
      - author
    sort:
      - property: status
        direction: ASC
      - property: file.mtime
        direction: ASC
    columnSize:
      file.name: 340
      note.summary: 700
      note.author: 348
      note.status: 190

```

## Logs

```dataview
TASK
WHERE (icontains(text, "#log/read")) AND 
		!(icontains(text, "#task") OR icontains(text, "#question"))
	AND !completed
GROUP BY file.name as Filename
SORT fows.file.ctime DESC
LIMIT 20
```

## Wish List

1. Godel Escher Bach
2. Linux Kernel	
3. Digital Minimalist
4. How to read a book
5. 7 Habits of Highly Effective People
6. Secrets and Lies: Digital Security in a Networked World
7. Your Money or Your Life
8. The Theoretical Minimum: What You Need to Know to Start Doing Physics
9. The World Beyond Your Head: On Becoming an Individual in an Age of Distraction