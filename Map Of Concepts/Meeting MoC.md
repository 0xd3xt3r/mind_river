---
up: "[[MoC Planet]]"
created-date: 2024-12-14
tags:
  - "#type/MoC"
summary: All the Meetings in the World
---

```base
filters:
  and:
    - file.hasTag("type/meeting")
    - not:
        - file.inFolder("Templates")
properties:
  file.name:
    displayName: File name
views:
  - type: table
    name: All the meetings
    order:
      - file.name
      - summary
      - participants
      - created-date
    sort:
      - property: created-date
        direction: DESC
    columnSize:
      file.name: 424
      note.summary: 645

```