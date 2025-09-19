---
up: "[[Qualcomm Project MoC]]"
related: 
created-date: 2025-04-02
status: in-progress
tags:
  - "#type/project"
  - "#qpsi/project"
---

> **Summary**:: This document contains the update from 

## Meetings or Reports

```dataview
TABLE WITHOUT ID
	file.link as "Session",summary,
	created-date, participants , tags, up
FROM !"Templates"
WHERE icontains(related, this.file.link)
AND icontains(tags, "type/meeting")
SORT filename DESC
```

## Important Document

- Bernard
	- [Camera call-graph Generation Conf](https://confluence.qualcomm.com/confluence/display/QPSIFT/Callgraph+and+Function+coverage+for+Linux+Android+Kernel+Drivers)
	- [Modem Off target Fuzzing](https://confluence.qualcomm.com/confluence/display/QPSIFT/Modem+Off-target+Fuzzing+Support)
- Satish
	- [WiFi Fuzzing Project Update](https://confluence.qualcomm.com/confluence/display/QPSIFT/WLAN+FW+Fuzzing)
- Ashish
	- [Automotive Fuzzing](https://confluence.qualcomm.com/confluence/pages/viewpage.action?pageId=1660140318)
- Neeraj
	- [KGSL Conf Link](https://confluence.qualcomm.com/confluence/display/QPSIFT/Android+GPU+Fuzzing%3A+KGSL)
- Myself
	- [Camera Fuzzing Conf Link](https://confluence.qualcomm.com/confluence/display/QPSIFT/Android+Camera+Driver+Fuzzing)
- Nirmal
	- 

## Work

- **Ashish** - Absent
- **Bernard** - Callgraph integration for Android
	- Fuzzing - nothing
- **Munawwar**
	- Camera APT testing integration found 3 crashes
	- Working on SIP Fuzzing
- **Nirmal** 
	- Stone static analysis build issue resolution
- **Neeraj**
	- KGSL - updating the build to latest version. 
	- Callgraph - resolving the issue in CFG algorithm generation
	- Infrastructure Setup - no update
- **Satish**
	- BT Firmware 
		- Many CR are false positive, working with the team to understand false positive.
		- escalated the CR, its high priority work.
	- WiFi Project
		- 20 Function need to document. Document atleast 5 function.
		- Negotiate bandwidth with the tech team.
		- Mainlining the fuzzer
		- Fixes for 2 CRs are not integrated in mainline.


## Tasks and Questions

1. IoT device fuzzing
	1. Build and flash latest image
	2. KASAN Build for the target
	3. Find the drivers which can be fuzzed and the 
		1. evaluate if the syz-grammar already exist for the driver.
		2. Find the driver for which the grammar doesn't exist.
2. Camera Driver
	1. CR analysis
	2. Call graph dashboard
	3. 

## Process

1. The goals of project page is to capture information which can help you retain small important details and challenges as we progress with the project. This little capture should accumulate over the time and will server as document to reflect and create an understand of the overall project.
2. Project related high level details are captured in two form
	1. Confluence page has the all the details related to the projects. 
	2. And the Execution task are captured in the Jira ticket.
		1. In Jira high level project is Top level task and the small task and created in the sub-task.
3. How frequently should we update the document?
	2. **Weekly Frequency** - On weekly basis you should update your Jira tickets which you are working on. The update should be put in the "work log" in the high level task not on the individual sub-task. Updating the ticket on high level should make task review easy.
		1. This update can said in the weekly meeting verbally.
	3. **Monthly Frequency** - On monthly basis you should update the KPI & the CR section of the project page you are working on, if you are working on multiple pages, updates should be pushed on each of the respective pages.
		1. Monthly QPSI project which is the QPSI wide document.
4. What should be the focus of the meeting
	1. What are the road-blocks you are facing?
	2. What new is going in the "Security Domain" - informal discussion.
5. KPI - How do you track the progress of the project?
	1. These are set of indicators which help you to assess the progress of you project.
	2. There are different type of KPI
		1. A typical application which you multiple attack surfaces which could be map to a function in a code, which we call it an entry point. One KPI for this could be Number of entry points you manage to write the harness vs the total number of entry-points you have identified.
		2. The entry point you have covered is give you certain coverage. What coverage is the entry point coverage generating.

---


## Archived Task

- [x] #task Define a structure for the Project 🔒 [[2025-04-30]] 🕸️ Meetings or Reports > Tasks and Questions
