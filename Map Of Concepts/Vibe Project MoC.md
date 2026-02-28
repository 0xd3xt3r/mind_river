---
up: "[[MoC Planet]]"
related:
  - "[[Reading MoC]]"
tags:
  - "#type/MoC"
created-date: 2024-12-15
summary: This is the page which actively highlights the documents which I am interacting with to learn new stuff.
---

## Projects
```base
filters:
  and:
    - file.hasTag("type/vibe-specs")
    - not:
        - file.inFolder("Templates")
properties:
  note.summary:
    displayName: Summary
  file.name:
    displayName: File name
views:
  - type: table
    name: General
    order:
      - file.name
      - summary
    sort:
      - property: file.mtime
        direction: DESC
    columnSize:
      file.name: 392
      note.summary: 700

```

## Prompt Ideas

```base
filters:
  and:
    - file.hasTag("type/prompt-idea")
    - not:
        - file.inFolder("Templates")
properties:
  note.summary:
    displayName: Summary
  file.name:
    displayName: File name
views:
  - type: table
    name: General
    order:
      - file.name
      - summary
    sort:
      - property: file.mtime
        direction: DESC
    columnSize:
      file.name: 392
      note.summary: 700

```
