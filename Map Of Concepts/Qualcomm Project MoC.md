---
up: "[[MoC Planet]]"
related:
  - "[[Qualcomm MoC]]"
tags:
  - "#type/MoC"
created-date: 2024-12-14
summary: All the Project going on in Qualcomm
---

## Pinned Docs

```base
filters:
  and:
    - file.hasTag("type/pinned-docs")
properties:
  note.summary:
    displayName: Summary
  file.name:
    displayName: File name
views:
  - type: table
    name: Pinned Docs
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
      note.summary: 838

```

### Projects

```base
filters:
  and:
    - file.hasTag("type/project")
    - file.hasTag("qpsi/project")
properties:
  note.summary:
    displayName: Summary
  file.name:
    displayName: File name
views:
  - type: table
    name: In-Progress
    filters:
      and:
        - status == "in-progress"
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
  - type: table
    name: On-Hold
    filters:
      and:
        - status == "on-hold"
    order:
      - file.name
      - summary
      - file.ctime
    sort:
      - property: summary
        direction: ASC
    limit: 10
    columnSize:
      file.name: 386
      note.summary: 867
      note.status: 243
  - type: table
    name: Done
    filters:
      and:
        - status == "done"
    order:
      - file.name
      - summary
      - file.ctime
    sort:
      - property: summary
        direction: ASC
    limit: 10
    columnSize:
      file.name: 386
      note.summary: 867
      note.status: 243

```

### Sub-Projects

```base
filters:
  and:
    - file.hasTag("type/sub-project")
    - file.hasTag("qpsi/project")
properties:
  note.summary:
    displayName: Summary
  file.name:
    displayName: File name
views:
  - type: table
    name: In-Progress
    filters:
      and:
        - status == "in-progress"
    order:
      - file.name
      - summary
      - status
      - file.ctime
    sort:
      - property: summary
        direction: ASC
    limit: 10
    columnSize:
      file.name: 386
      note.summary: 647
      note.status: 174
  - type: table
    name: On-Hold
    filters:
      and:
        - status == "on-hold"
    order:
      - file.name
      - summary
      - status
      - file.ctime
    sort:
      - property: summary
        direction: ASC
    limit: 10
    columnSize:
      file.name: 386
      note.summary: 867
      note.status: 243
  - type: table
    name: Done
    filters:
      and:
        - status == "done"
    order:
      - file.name
      - summary
      - file.ctime
      - file.mtime
    sort:
      - property: summary
        direction: ASC
    limit: 10
    columnSize:
      file.name: 420
      note.summary: 867
      file.ctime: 220

```