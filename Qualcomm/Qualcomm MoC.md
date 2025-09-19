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

> Last Internet Reimbursement File Date : 11-12-2024

## Pinned Docs

```dataview
TABLE WITHOUT ID
	file.link as "Project",
	summary,
	tags,
	created-date
FROM #type/qcom AND !"Templates"
SORT file.name
```

## Internal Reports

```dataview
TABLE WITHOUT ID
	file.link as "Project",
	summary,
	status,
	tags,
	created-date
FROM #qpsi/report AND !"Templates"
SORT file.name
```

## QIPL QPSI Report Docs

- [Weekly Report Update](https://confluence.qualcomm.com/confluence/display/QPSIFT/Weekly+Report)
- [Jira Task Dashbaord](https://jira-dc.qualcomm.com/jira/projects/QPSIFUZZIN/issues/QPSIFUZZIN-13?filter=allissues)
- [Fuzzing Metric Update](https://confluence.qualcomm.com/confluence/display/QPSIFT/Fuzzing+Projects:+Insights)
- [QIPL QPSI Hardware Asset Tacking](https://confluence.qualcomm.com/confluence/display/QPSIFT/List+of+Lab+PCs+and+Usage)
