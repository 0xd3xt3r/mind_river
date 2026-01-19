---
up: "[[Knowledge Base MoC]]"
---

> This is a home screen for Project "Mind River"

---

## Important Docs

| Office                   | Creative               | Misc                     |
| ------------------------ | ---------------------- | ------------------------ |
| [[Qualcomm Project MoC]] | [[Writing MoC]]        | [[Knowledge Base MoC]]   |
| [[Qualcomm MoC]]         | [[Reading MoC]]        | [[Inbox MoC]]            |
| [[Task MoC]]             | [[Learning MoC]]       | [[Finance MoC]]          | 
| [[People MoC]]           | [[SeedBox MoC]]        | [[Personal Project MoC]] |
| [[Meeting MoC]]          | [[Research Paper MoC]] | [[MoC Planet]]           |

---

## Pinned Docs

```base
views:
  - type: table
    name: Projects
    filters:
      and:
        - or:
            - file.tags.contains("type/pinned-docs")
        - not:
            - file.inFolder("Templates")
            - file.hasTag("type/project/personal")
    order:
      - file.name
      - summary
      - created-date
    sort:
      - property: file.name
        direction: DESC
      - property: file.mtime
        direction: DESC
    limit: 10
    columnSize:
      file.name: 329
      note.summary: 838
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
--- 
## Weekly Notes

```base
filters:
  and:
    - file.hasTag("type/note/weekly")
    - not:
        - file.inFolder("Templates")
properties:
  file.name:
    displayName: File name
views:
  - type: table
    name: Most Recent Weekly Notes
    order:
      - file.name
      - summary
      - score
      - created-date
    sort:
      - property: created-date
        direction: DESC
    limit: 5
    columnSize:
      file.name: 159
      note.summary: 675
      note.score: 98

```
---

## Active Projects

```base
views:
  - type: table
    name: Projects
    filters:
      and:
        - or:
            - file.tags.contains("type/project")
            - file.tags.contains("qpsi/project")
            - file.tags.contains("type/sub-project")
        - not:
            - file.inFolder("Templates")
            - file.hasTag("type/project/personal")
        - file.tags.contains("status/active")
    order:
      - file.name
      - summary
    sort:
      - property: file.name
        direction: DESC
      - property: file.mtime
        direction: DESC
    limit: 10
    columnSize:
      file.name: 329
      note.summary: 823
      file.tags: 449
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