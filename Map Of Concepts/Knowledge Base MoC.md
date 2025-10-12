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
    columnSize:
      file.name: 392
      note.summary: 700
```

## Operating Systems

```base
filters:
  and:
    - file.hasTag("type/os/linux")
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
    order:
      - file.name
      - summary
    columnSize:
      file.name: 392
      note.summary: 700
```

```dataview
TABLE summary as Summary
FROM #type/os/windows  and -"Templates"
SORT file.name
```

```dataview
TABLE summary as Summary
FROM #type/os AND -"Templates" AND -#type/os/windows AND -#type/os/linux
SORT file.name
```