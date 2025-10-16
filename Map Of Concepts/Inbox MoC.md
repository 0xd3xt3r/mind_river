---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-14
summary: All notes is not MoC, people or projects
---

```base
filters:
  and:
    - file.inFolder("Inbox")
properties:
  file.name:
    displayName: File name
views:
  - type: table
    name: Map of Knowledge
    order:
      - file.name
      - Summary
      - file.mtime
    sort:
      - property: file.mtime
        direction: DESC
    columnSize:
      file.name: 386
      note.summary: 834

```