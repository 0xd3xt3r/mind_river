---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-15
summary: All the Meta data related unorgainzed Topics Which Need More care and consideration
---

```dataview
TABLE summary as Summary, tags
FROM #type/knowledge and -"Templates"
SORT file.name
```

## Operating Systems

```dataview
TABLE summary as Summary, tags
FROM #type/os/linux and -"Templates"
SORT file.name
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