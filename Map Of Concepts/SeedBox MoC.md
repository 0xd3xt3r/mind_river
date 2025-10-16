---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-14
summary: All notes or idea which you should work on!
---

## Seed Thoughts

```base
filters:
  and:
    - file.inFolder("Seedbox")
    - not:
        - file.inFolder("Templates")
properties:
  note.summary:
    displayName: Summary
  file.name:
    displayName: File name
views:
  - type: table
    name: Done
    filters:
      and:
        - status == "done"
    order:
      - file.name
      - summary
    sort:
      - property: file.mtime
        direction: DESC
    columnSize:
      file.name: 392
      note.summary: 700
  - type: table
    name: All
    order:
      - file.name
      - summary
      - status
    sort:
      - property: file.mtime
        direction: DESC

```
## Seed Logs

```dataview
TASK WHERE icontains(text, "#log/seedbox")
GROUP BY file.name as filename
```

