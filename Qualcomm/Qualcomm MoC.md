---
up: "[[MoC Planet]]"
related:
  - "[[Qualcomm Project MoC]]"
tags:
  - "#type/MoC"
aliases:
  - qcom homepage
created-date: 2024-12-09
summary: Everything Related to Qualcomm will be placed here
---

> Last Internet Reimbursement File Date : 01-10-2025

## Pinned Docs

```base
filters:
  and:
    - file.hasTag("type/qcom")
    - not:
        - file.inFolder("Templates")
properties:
  file.name:
    displayName: File name
views:
  - type: table
    name: Knowledge Docs
    order:
      - file.name
      - summary
      - created-date
    sort:
      - property: created-date
        direction: DESC
    limit: 8
    columnSize:
      file.name: 268
      note.summary: 675
```
## Internal Reports

```base
filters:
  and:
    - file.hasTag("qpsi/report")
    - not:
        - file.inFolder("Templates")
properties:
  file.name:
    displayName: File name
views:
  - type: table
    name: Active Reports
    filters:
      and:
        - status != "archived"
    order:
      - file.name
      - summary
      - created-date
    sort:
      - property: created-date
        direction: DESC
    limit: 8
    columnSize:
      file.name: 256
      note.summary: 675
  - type: table
    name: Archived Reports
    filters:
      and:
        - status == "archived"
    order:
      - file.name
      - summary
      - created-date

```

## QIPL QPSI Report Docs

- [Weekly Report Update](https://confluence.qualcomm.com/confluence/display/QPSIFT/Weekly+Report)
- [Jira Task Dashbaord](https://jira-dc.qualcomm.com/jira/projects/QPSIFUZZIN/issues/QPSIFUZZIN-13?filter=allissues)
- [Fuzzing Metric Update](https://confluence.qualcomm.com/confluence/display/QPSIFT/Fuzzing+Projects:+Insights)
- [QIPL QPSI Hardware Asset Tacking](https://confluence.qualcomm.com/confluence/display/QPSIFT/List+of+Lab+PCs+and+Usage)
