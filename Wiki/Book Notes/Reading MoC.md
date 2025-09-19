---
up: "[[MoC Planet]]"
tags:
  - "#type/MoC"
created-date: 2024-12-15
summary: Everything Related to things I have Read
---


```dataview
TABLE tags, status, author, file.mtime as "Last Modified"
FROM #type/reading/book and !"Templates"
SORT file.mtime DESC
LIMIT 20
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
- Meditations - Marcus Aurelius	Done	Philosophy	
- Linux Kernel	Currently Reading	Computers,Security	Robert Love
- Homo Deus	Done	Future	
- Digital Minimalist	Done	Productivity	Cal Newport
- Deep Work	Done	Productivity	Cal Newport
- How to read a book	Paused	Skill-Building	
- Atomic Habits	Currently Reading	Productivity	James Clear
- 7 Habits of Highly Effective People	Want To Read		
- Secrets and Lies: Digital Security in a Networked World	Want To Read	Computer-Security,Skill-Building	Bruce Schneier
- Your Money or Your Life	Want To Read	Skill-Building	
- Make Time: How to focus on what matters every day	Want To Read	Productivity	
- The Theoretical Minimum: What You Need to Know to Start Doing Physics	Want To Read		
- On Writing Well: The Classic Guide to Writing Nonfiction	Want To Read		
- The World Beyond Your Head: On Becoming an Individual in an Age of Distraction	Want To Read	Skill-Building	