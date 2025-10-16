---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-15
summary: All the Meta data related unorgainzed Topics Which Need More care and consideration
---

```base
filters:
  and:
    - file.hasTag("type/knowledge")
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

## Operating Systems

```base
filters:
  and:
    - file.hasTag("type/os")
    - not:
        - file.inFolder("Templates")
properties:
  note.summary:
    displayName: Summary
  file.name:
    displayName: File name
views:
  - type: table
    name: Linux
    filters:
      and:
        - file.tags.containsAny("type/os/linux")
    order:
      - file.name
      - summary
    columnSize:
      file.name: 392
      note.summary: 700
  - type: table
    name: Windows
    filters:
      and:
        - file.tags.containsAny("type/os/windows")
    order:
      - file.name
      - summary
    columnSize:
      file.name: 392
      note.summary: 700
  - type: table
    name: Misc
    filters:
      not:
        - file.tags.containsAny("type/os/windows")
        - file.tags.containsAny("type/os/linux")    
    order:
      - file.name
      - summary
    columnSize:
      file.name: 392
      note.summary: 700
```
