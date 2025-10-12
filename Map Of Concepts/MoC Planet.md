---
up: "[[Welcome Home]]"
created-date: 2024-12-14
---

## Map of Concepts

```base
filters:
  and:
    - file.hasTag("type/MoC")
    - not:
        - file.inFolder("Templates")
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
      - property: summary
        direction: ASC
    columnSize:
      file.name: 241
      note.summary: 834
```