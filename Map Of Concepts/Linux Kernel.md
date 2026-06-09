---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2026-05-29
summary: All the articles related to Linux Kernel
related:
  - "[[Knowledge Base MoC]]"
---


```base
properties:
  note.summary:
    displayName: Summary
  file.name:
    displayName: File name
views:
  - type: table
    name: Sub-Projects
    filters:
      and:
        - file.hasLink(this.file)
    order:
      - file.name
      - summary
      - file.mtime
    sort:
      - property: file.mtime
        direction: DESC
    limit: 10
    columnSize:
      file.name: 386
      note.summary: 866

  ```