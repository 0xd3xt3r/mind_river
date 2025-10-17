---
up: "[[Knowledge Base MoC]]"
---

> This is a home screen for Project "Mind River"

## Important Docs


| Office                   | Creative               | Misc                     |
| ------------------------ | ---------------------- | ------------------------ |
| [[Qualcomm Project MoC]] | [[Writing MoC]]        | [[Knowledge Base MoC]]   |
| [[Qualcomm MoC]]         | [[Reading MoC]]        | [[Inbox MoC]]            |
| [[Task MoC]]             | [[Learning MoC]]       | [[Finance MoC]]          | 
| [[People MoC]]           | [[SeedBox MoC]]        | [[Personal Project MoC]] |
| [[Meeting MoC]]          | [[Research Paper MoC]] | [[MoC Planet]]           |

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
      note.summary: 1027
      note.score: 98

```

## Recent Files

```base
views:
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

```