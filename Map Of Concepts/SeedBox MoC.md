---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-14
summary: All notes or idea which you should work on!
---

## Seed Thoughts

```dataview
TABLE
file.mtime as "Last Modified"
from "Seedbox"
SORT file.ctime ASC
```

## Seed Logs

```dataview
TASK WHERE icontains(text, "#log/seedbox")
GROUP BY file.name as filename
```

