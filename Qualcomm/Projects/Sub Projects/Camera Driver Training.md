---
tags:
  - "#type/sub-project"
  - "#status/active"
  - qpsi/project
status: in-progress
up: "[[QPSI Security Trainings]]"
related:
created-date: 2025-10-16
summary: Create training for Camera Kernel team and integerate it in Workday. This will be a mandatory training for the team.
---

## Notes
> What did you learn?

- [ ] Discussion with the [[Nicolas]] on how to implement the training.
	1. the learning team, create slide with specific teams, n2k support to team specific team and channels team
	2. LMS team - learning team
	3. Channel - create channel for the training, do it with channel team.
		1. create user list - two list 
			1. attend group
			2. complete training
		2. channels.qualcomm.com
		3. QGroup - perforce will look at channel it cannot query channel.
	4. p4 - list of repo to which the qgroup 
	5. articular 360 - create training in this tool. get license. 1000$
	6. When team pushes the code to code repo the server will check if the individual pushing the training 
		1. callback code on perforce server.
		2. the code run by n2k team

## Task
> What needs to be done!

- [ ] #task #qpsi Create the v1 of the training and present it to the tech team and verify the content. ➕ 2025-10-16
- [x] #task #qpsi Get confirmation from tech team to conduct the Camera Driver Training ➕ 2025-10-17 ✅ 2026-02-27

## Project Management

### Meetings

```dataview
TABLE WITHOUT ID
	file.link as "Session",
	up, created-date, participants, summary, tags
FROM !"Templates"
WHERE icontains(related, this.file.link)
AND icontains(tags, "type/meeting")
SORT file.link DESC
```

### Tasks and Questions

```dataview
TASK
WHERE icontains(text, this.file.name)
AND (icontains(tags, "#task") OR icontains(text, "#questions"))
AND !completed
GROUP BY file.name as filename
SORT filename DESC
```

### Archived Task
