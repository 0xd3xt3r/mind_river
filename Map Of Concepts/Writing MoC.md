---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-14
summary: Writing everything down
---

## Blogpost

```base
filters:
  and:
    - file.hasTag("type/writing/article")
    - not:
        - file.inFolder("Templates")
properties:
  file.name:
    displayName: File name
views:
  - type: table
    name: ToDo
    filters:
      and:
        - status != "done"
    order:
      - file.name
      - status
      - created-date
    sort:
      - property: file.mtime
        direction: DESC
    columnSize:
      file.name: 597
      note.status: 195

```

## Social Media Post

```base
filters:
  and:
    - file.hasTag("type/writing/social-media")
    - not:
        - file.inFolder("Templates")
properties:
  file.name:
    displayName: File name
views:
  - type: table
    name: Todo Post
    filters:
      and:
        - status == "todo"
    order:
      - file.name
      - created-date
    sort:
      - property: summary
        direction: ASC
    columnSize:
      file.name: 796
  - type: table
    name: Published Post
    filters:
      and:
        - status == "published"
    order:
      - file.name
      - status
      - created-date
    sort:
      - property: summary
        direction: ASC
    columnSize:
      file.name: 559
      note.status: 195

```

## Books

[[Reverse Engineers Guide to ARM SoC|Advenctures of SoC]]