---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-15
summary: Everything Related to things I have Read
---

```base
filters:
  and:
    - file.hasTag("type/reading/book")
    - not:
        - file.inFolder("Templates")
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
      - author
      - status
    sort:
      - property: summary
        direction: ASC
    columnSize:
      file.name: 386
      note.summary: 700
      note.author: 348
      note.status: 190

```

## Logs

```dataview
TASK
WHERE (icontains(text, "#log/read")) AND 
		!(icontains(text, "#task") OR icontains(text, "#question"))
	AND !completed
GROUP BY file.name as Filename
SORT fows.file.ctime ASC
LIMIT 20
```

## Wish List

- Godel Escher Bach	Paused	Computers, Math,Science	
- Linux Kernel	Currently Reading	Computers,Security	Robert Love
- Digital Minimalist	Done	Productivity	Cal Newport
- How to read a book	Paused	Skill-Building	
- 7 Habits of Highly Effective People	Want To Read		
- Secrets and Lies: Digital Security in a Networked World	Want To Read	Computer-Security,Skill-Building	Bruce Schneier
- Your Money or Your Life	Want To Read	Skill-Building	
- Make Time: How to focus on what matters every day	Want To Read	Productivity	
- The Theoretical Minimum: What You Need to Know to Start Doing Physics	Want To Read		
- The World Beyond Your Head: On Becoming an Individual in an Age of Distraction	Want To Read	Skill-Building	