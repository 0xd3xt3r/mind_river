---
aliases:
  - Fuzzing Farm
  - Scalable Fuzzing
tags:
  - "#qpsi/project"
  - "#fuzzing"
  - "#type/sub-project"
  - "#status/active"
status: archived
up: "[[Android Kernel Driver Fuzzing]]"
related: 
created-date: 2024-09-25
summary: Setup a continues fuzzing farm with all the old devices
---

> **Summary**:: 


## Meetings

```dataview
TABLE WITHOUT ID
	file.link as "Session",
	up, created-date, participants, summary, tags
FROM !"Templates"
WHERE icontains(up, this.file.link)
AND icontains(tags, "type/meeting")
SORT file.link DESC
```

## Tasks and Questions

### ToDo

```dataview
TASK
WHERE icontains(text, this.file.name)
AND (icontains(tags, "#task") OR icontains(text, "#questions"))
AND !completed
GROUP BY file.name as filename
SORT filename DESC
```

### Completed Task

```dataview
TASK
WHERE icontains(text, this.file.name)
AND (icontains(tags, "#task") OR icontains(text, "#questions"))
AND completed
GROUP BY file.name as filename
SORT filename DESC
```

## Sprints

```dataview
TASK
WHERE icontains(text, this.file.name)
AND icontains(text, "#log/sprints")
GROUP BY file.name as filename
SORT filename DESC
```

## Objective

> What were you trying to achieve with this project.

1. Scalable Fuzzing setup for Qualcomm Android kernel Driver
2. Fuzzing with Pakala device 
	1. Corpus enrichment - to expand the breath of baseline corpus.
		1. we can use the unittest code to find the good parameter value
	2. Fuzzing without any instrumentation to increase the speed and accuracy of race-condition fuzzing, with hardware ASAN running
	3. Also fuzzing to uncover Race condition issues KCSAN.
3. Continuously fuzzing the Android devices through out their life in the market
	1. Recycle old device in QCOM to fuzz it while it is still in life in market
	2. two more year of fuzzing while device is in market, next year Kanapali
	3. always fuzz three generation of devices
4. Central dashboard to track all the parallel fuzzing device in-progress
5. Testing the driver code as part of CI/CD, so when there is a new code in the driver it should be sent for fuzzing.
6. Monitor the attack surface of the kernel drivers.
	1. Static analysis tools like syz-describe can help use to get the denominator value, numerator will be the syz-description.
	2. We don't know new function added to the code base, grammar only addresses existing syskalls.
7. Focused fuzzing
	1. create deployment which focuses on specific driver for example kgsl, camera, etc.
	2. It can also have special configuration to improve the fuzzing like
		1. camera has procs:1 requirement
		2. KCSAN instrumented build or build we no instrumentation
---

## Project Execution

### Phase 1 : Setup

1. [x] #task Collect all the devices in Hyderabad with [[Nagaraju]] help
2. [ ] #task Hardware Requirement
	1. [ ] USB Hub
	2. [ ] Lab Machines to attach MTP Devices
	3. [ ] MTP Devices which will be sent to Hyderabad
	4. [ ] Configure the syzkaller setup for multiple devices
3. Start with Camera driver fuzzing
4. Then add support for other drivers

### Phase 2 : 
- #task Docker container for syzkaller execution 
	- Challanges
		- APT uses axiom, not sure it supports docker as they need to attach device into axiom too
		- pcat to collect ramdump too
		- here were issues using qualcomm tools (quts service or so)
		- qpm installation issue
- #task MPT Build image automation 

## Commands

```bash
# some of the important command in this project
```
---

## Notes

---

## Important Docs
> Some of the important document which would you often need to refer very often.

---

## CR Raised

### Bug Tracker

| ID | CR | Rating | Tag | Comment |
|---|---|---|---|---|
| 1 | 01 | High | #bug-tag| What happened?|

---

## Meeting Notes

### 25-09-2024

#### Notes and Decisions

- Large fuzzing farm on old Pakala devices create large corpus
- Race-condition with tc asan
- Collect all devices in India/Hyderabad
- Get the farm up and running
- slowly integrate or all drivers
- syzkaller dashboard
- get USB hub
- lab machine
- configure the setup
- two more year of fuzzing while device is in market, next year Kanapali
- always fuzz three generation of devices
- same for windows

---
---

## History

> A brief timeline of the project

1. 25-09-2024 - Start project discussion