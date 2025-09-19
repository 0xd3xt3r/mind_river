
## Logs

```dataview
TASK
WHERE (icontains(text, this.file.name) AND icontains(text, "#log/sprint")) AND 
		!(icontains(text, "#task") OR icontains(text, "#question"))
	AND !completed
GROUP BY file.name as Filename
SORT fows.file.ctime DESC
LIMIT 20
```