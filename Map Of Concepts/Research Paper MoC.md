---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-16
summary: Notes on all the research papers
---

```base
views:
  - type: table
    name: Projects
    filters:
      and:
        - file.tags.contains("reading/research-paper")
        - not:
            - file.inFolder("Templates")
    order:
      - file.name
      - summary
      - file.tags
      - status
    sort:
      - property: file.mtime
        direction: DESC
    limit: 10
    columnSize:
      file.name: 437
      note.summary: 599
      file.tags: 443
  - type: table
    name: Recent Files
    order:
      - file.name
      - summary
      - file.tags
    sort:
      - property: file.mtime
        direction: DESC
    limit: 5
    columnSize:
      note.summary: 553
```