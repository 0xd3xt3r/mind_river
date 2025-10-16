---
up: "[[Qualcomm MoC]]"
tags:
  - "#qpsi/report"
summary: Historical attempt to capture daily work logging
status: archived
---

# Quarter #4

##  December

## November

### 04/12 - 04/12

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- IRVR ticket analysis, found the bug but 
- **Tuesday**
	- found bug in SOC register dump and that pattern on in 16 different location.
- **Wednesday**
	- found bug in OPE sub-system by code review
	- more core review found 2 more bugs
- **Thursday**
	- cam_mem_get_cpu_buf code review
- **Friday**

#### Task
> What all things you have to do and what all things you want to achieve this week.


#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #04-12-2024
	- **Title:** Weekly Sync-Up
	- *Attendees* : Nagaraju & Fuzzing team
	- **Action Item**
	- **Meeting Minutes***
		- Manager on Leave

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.


#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

### 25/11 - 29/11

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- added DR Reviewer
	- patched camera driver and recompiled the driver and flashed only the camera driver, driver didn't initialized.
- **Tuesday**
	- Because of camera code failure, latest Pakala code
	- dealing with pakala build systems issue, python3 and python2 issue
- **Wednesday**
	- 
- **Thursday**
- **Friday**

#### Task
> What all things you have to do and what all things you want to achieve this week.


#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #25-11-2024
	- **Title:** Weekly Sync-Up
	- *Attendees* : Nagaraju & Fuzzing team
	- **Action Item**
	- **Meeting Minutes***
		- Cancelled

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- 

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

### 18/11 - 23/11

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- FastRPC Training improvement
	- go module debugging with vscode and dlv
- **Tuesday**
	- fixed buffer offset syz-lang issue by referring to dev_binder.txt *binder_transaction_data* declaration.
- **Wednesday**
	- completed 90% with jpeg *cam_jpeg_add_command_buffers* function.
- **Thursday**
	- jpeg analysis and 
	- ioctl data dump vulnerability QPSIVR-2070
- **Friday**
	- FastRPC Training - Part 1
		- was expecting lingtong to speak in 2nd half but 1st half got extended because of discussions.

#### Task
> What all things you have to do and what all things you want to achieve this week.
- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [ ] JPEG
		- [ ] TFE
		- [ ] ICP
		- [ ] Request Manager
		- [ ] Make single struct grammar into array grammar for different struct
- [ ] Open Source Newsletter
	- [ ] Qualcomm Open Source News
	- [ ] Opensource Articles from Opensource Forums
		- [ ] Syzkaller article
- [ ] FastRPC Training on Nov 22 & 29

##### Maintainer's Task

- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
	- [ ] Dhananjay Latkar
- Paper work 
	- [ ] Gratuity Nominee Form
	- [ ] Internet bills

#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #18-11-2024
	- **Title:** Weekly Sync-Up
	- *Attendees* : Nagaraju & Fuzzing team
	- **Action Item**
	- **Meeting Minutes***
		- WFH discussion, 12 days per quarter and if there are public holiday/leaves that would be taken form rest of the office days
		- for rest of office days you have to be in office for 75% of the days.
- #19-11-2024
	- **Title:** FastRPC Training meeting
	- *Attendees* : Lingtong, Abhinash and Surendra
	- **Action Item**
		- book meeting room
		- Introduction paragraphs
	- **Meeting Minutes***
		- 

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.

- the problem of debugging syzkaller on UI has made significant progress, we can how use vscode (vscode-go) and dlv (go packet) to do dynamic debugging.

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

### 11/11 - 15/11

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- able to figure-out JPEG patch buffer creation
	- meeting with Suraj about JPEG buffer
- **Tuesday**
	- Integrating JPEG PoC Code in syzkaller grammar
	- Able to fix grammar for VIDIOC_CAM_MGR_ALLOC_CMD with in/out resources.
	- Able to allocate patch buffer with VIDIOC_CAM_MGR_ALLOC_CMD with proper flags and redirect it to JPEG config dev 
- **Wednesday**
	- One-to-one meeting with surendra
		- not ready for promotion
	- Confidential Compute Meeting with Surendra.
		- Step closer to zero trust model.
		- User can trust the exec env, for execution correction-ness and **data privacy**
		- important for data privacy
		- this for data center.
		- Assurance
			- send data is encrypted, decrypt only when needed
			- when data is stored and process it. protected when stored on DDR or device.
			- if the data is transmitted, transmission will be protected.
			- data at execution will be only available owner of the data.
		- TZ is only meets some of the requirement.
		- Data center, VM are users. User data are raw data, complete system image.
		- VM is trusted & IO Trusted
		- backbone component for trusting.
- **Thursday**
	- 
- **Friday**
	- improved FastRPC training content

#### Task
> What all things you have to do and what all things you want to achieve this week.
- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [ ] JPEG
		- [ ] TFE
		- [ ] ICP
		- [ ] Request Manager
		- [ ] Make single struct grammar into array grammar for different struct
- [ ] Open Source Newsletter
	- [ ] Qualcomm Open Source News
	- [ ] Opensource Articles from Opensource Forums
		- [ ] Syzkaller article
- [ ] FastRPC Training on Nov 22 & 29

##### Maintainer's Task

- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
	- [ ] Dhananjay Latkar
- Paper work 
	- [ ] Gratuity Nominee Form
	- [ ] Internet bills

#### Meetings Logs
> Description what all discussion you have with what people about anything important.


- #11-11-2024
	- **Title:** Weekly Sync-Up
	- *Attendees* : Nagaraju & Fuzzing team
	- **Action Item**
	- **Meeting Minutes***
		- cancelled nothing to discuss

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- 

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

### 04/11 - 08/11

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- 
- **Tuesday**
- **Wednesday**
- **Thursday**
	- jpeg PoC started working
- **Friday**
	- QC Global Holiday

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [ ] JPEG
		- [ ] TFE
		- [ ] ICP
		- [ ] Request Manager
		- [ ] Make single struct grammar into array grammar for different struct
- [ ] Open Source Newsletter
	- [ ] Qualcomm Open Source News
	- [ ] Opensource Articles from Opensource Forums
		- [ ] Syzkaller article

##### Maintainer's Task

- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
	- [ ] Dhananjay Latkar
- Paper work 
	- [ ] Gratuity Nominee Form
	- [ ] Internet bills

#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #04-11-2024
	- **Title:** Weekly Sync-Up
	- *Attendees* : Nagaraju & Fuzzing team
	- **Action Item**
	- **Meeting Minutes***
		- 

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- 

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

## October

### 28/10 - 01/11

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- Faiz Marriage
- **Tuesday**
	- Syzkaller code walk-through
- **Wednesday**
	- Faiz Walima
	- Travelling to Ahmedabad and Property inspection
- **Thursday**
	- Diwali Holiday
- **Friday**
	- FastRPC Training Material
	- DR Training Test
	- Camera PoC For patch buffer
	- News Letter by Taniya

#### Task
> What all things you have to do and what all things you want to achieve this week.


#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #28-10-2024
	- **Title:** Weekly Sync-Up
	- *Attendees* : Nagaraju & Fuzzing team
	- **Action Item**
	- **Meeting Minutes***
		- WFH policy Discussion which he already discussed on Friday

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- Our conclusion is that we always have to test new code by emulating bugs or reproducing old ones.
- Fuzzers are not magical, in order to find bugs we need to make sure we cover most of the attack surfaces in our target.
	- We went back to read more prior work of Win32k, understand old bugs and bug classes. We then tried to support the newly learned attack surfaces to our fuzzer.

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---
### 21/10 - 21/10

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- jpeg code analysis
	- jpeg acquire hardware
- **Tuesday**
	- jpeg config dev
	- jpeg patch buffer not working as it needs special type of buffer
		- patch offset is sum two buffers
- **Wednesday**
- **Thursday**
- **Friday**

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [ ] JPEG
		- [ ] TFE
		- [ ] ICP
		- [ ] Request Manager
		- [ ] Make single struct grammar into array grammar for different struct

##### Maintainer's Task

- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
	- [ ] Dhananjay Latkar
- Paper work 
	- [ ] Gratuity Nominee Form
	- [ ] Internet bills
	- [ ] Medi-assist bill reimbursement issue.


#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #21-10-2024
	- **Title:** Weekly Sync-Up
	- *Attendees* : Nagaraju & Fuzzing team
	- **Action Item**
	- **Meeting Minutes***
		- Weekly Goal
		- Motivational Video
- #29-10-2024
	- **Title:** Syzkaller Brain Dump
	- *Attendees* : 
	- **Action Item**
	- **Meeting Minutes***
		- Camx overview

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- 

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

### 14/10 - 18/10

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- FastRPC preparation 
- **Tuesday**
	- Fixed MTP device issue, now able to do remote reboot.
- **Wednesday**
	- Syzkaller continues fuzzing
- **Thursday**
	- FastRPC Research
	- Camera CR email
- **Friday**

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [ ] TFE
		- [ ] JPEG
		- [ ] ICP
		- [ ] Request Manager
		- [ ] Make single struct grammar into array grammar for different struct

##### Maintainer's Task

- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
	- [ ] Dhananjay Latkar
- Paper work 
	- [ ] Gratuity Nominee Form
	- [ ] Internet bills
	- [ ] Medi-assist bill reimbursement issue.

#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #14-10-2024
	- **Title:** Weekly Sync-Up
	- *Attendees* : Nagaraju & Fuzzing team
	- **Action Item**
	- **Meeting Minutes***
		- Meeting started and Nagaraju didn't attend the meeting.
		- He was on leave

- #16-10-2024
	- **Title:** Open-Source Newsletters Contribution
	- *Attendees* : Taniya Das (SoC Team)
	- **Action Item**
	- **Meeting Minutes***
		- Contribute article
		- Add Zephyr related article/data points in the news letters
#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- 

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

### 07/10 - 11/10

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- CISPHY grammar completed
	- completed CISPHY corpus
- **Tuesday**
	- started research in TFE_MC sub-system and added ioctl grammar, more research into this sub-system is needed.
- **Wednesday**
	- completed CPAS sub-system
- **Thursday**
	- Improved CPAS sub-system grammar and added corpus for the sub-system
- **Friday**
	- FastRPC research
	- SoC Research

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [x] CISPHY
		- [ ] TFE
		- [x] CPAS
		- [ ] JPEG

##### Maintainer's Task

- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
		- [ ] rizwan schedule the meeting
	- [ ] Dhananjay Latkar
- Paper work 
	- [ ] Gratuity Nominee Form
	- [ ] Internet bills
	- [ ] Medi-assist bill reimbursement issue.

#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #08-10-2024
	- **Title** : Fastrpc Training
	- *Attendees* : Abhinash
	- **Action Item:**ls 
		- Recommended internal training for tech team 
			- Linux Training
			- Code Review
		- Nov 11-15 for first session and next week 2nd session and session duration 2 hours
	- **Meeting Minutes***
		- candidate are too young
		- Location : Hyderabad(7) & San Diego (7)
		- Max lead engineer and above
		- Bug pattern and how to fix the architecture
		- DSP compromise
		- how is exploitation is actually done.
		- understand the threat model.
		- as the software evolve attack vector will also evolve and change.
		- FASTRPC connects two sub-system
- #09-10-2024
	- **Title:** QPSI Monthly Meeting
	- *Attendees* : 
	- **Action Item**
	- **Meeting Minutes***
		- Venus to HLOS, Scott
			- DMA attack or DMA to other sub-system attack
		- Auto bus, FastRPC.

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- Pakala Leap SP build for 8850 until Feb
- KLM & K2L - Linux support for our chipset
- Device issue:
	- Bus 001 Device 003: ID 0403:6001 Future Technology Devices International, Ltd FT232 USB-Serial (UART) IC
		- FT232 should be FT432

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

### 30/09 - 04/10

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- Gen AI Training & Cyber security training
- **Tuesday**
	- Device debugging & coverage investigation with APT team.
- **Wednesday**
	- Holiday
- **Thursday**
	- improve coverage for sensor sub-system.
- **Friday**
	- finished implementing Sensor except probe sensor command.

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [x] Sensor
		- [ ] CISPHY
		- [ ] TFE
		- [ ] CPAS
		- [ ] JPEG
- [x] How much bench space is available for us?
	- only 2

##### Maintainer's Task

- [x] Cyber security training
- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
		- [ ] rizwan schedule the meeting
	- [ ] Dhananjay Latkar
- Paper work 
	- [ ] Gratuity Nominee Form
	- [ ] Internet bills
	- [ ] Medi-assist bill reimbursement issue.

#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #30-09-2024
	- **Title** : Weekly Sync-up
	- *Attendees* : Nagaraju & Fuzzing Team
	- **Meeting Minutes***
		- No meeting update
		- Yearly goal update on time
		- Bernard attended Compiler conference.
- #01-10-2024
	- **Title** : Surendra Sync-Up
	- *Attendees* : Surendra & Me
	- **Meeting Minutes***
		- Microchip has good talent
		- 2 Year for our growth of security org
- #03-10-2024
	- **Title** : Fuzzing Update
	- *Attendees* : Fuzzing Team & Murali
	- **Meeting Minutes***
		- Camera Grammar improvement is priority over setup issues
		- depending on APT team for finding bug.
		- Reproduce the bugs which are reported

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- power charging in extreme low power is done with ADSP (audio sub-system) core because it requires very low power to operate.
- When you see a bug in fuzzing project, do one of the blow:
	- fix the bug to unblock the execution path for further exploration 
	- disable the buggy path so that fuzzer can explore the other paths
		- for eg, disable ioctl config to disable buggy ioctl command so that other paths can be explored 

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---


# Quarter #3
## September

### 23/09 - 27/09

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- camera database issue in syzkaller
		- now working on new version but working on older version
- **Tuesday**
	- debugging corpus issue
		- fixed by added `-os linux -arch arm64`
		- new version of syzkaller has more strict corpus grammar checking.
	- FastRPC training meeting
	- Windows WiFi CR Meeting
- **Wednesday**
	- Discussion with Murali about fuzzing farm project
	- Patch strategy to use for upstream open-source projects.
- **Thursday**
	- syzkaller setup for new version
	- coverage issue : started displaying coverage sometime after execution started
- **Friday**
	- started working on sensor sub-system grammar

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [ ] Sensor
		- [ ] CISPHY
		- [ ] TFE
		- [ ] CPAS
		- [ ] JPEG

##### Maintainer's Task

- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
		- [ ] rizwan schedule the meeting
	- [ ] Dhananjay Latkar
- [ ] Paper work - Gratuity Nominee Form

#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #24-09-2024
	- **Title** : fastRPC Training
	- *Attendees* : Lingtong Shen, Surendra Singh, Abhinash Jain
	- **Meeting Minutes***
		- Content
			- Part 1 - Basic Security 
			- Part 2 - fastRPC Security
			- Lingtong has already created one.
		- Expectation :
			- understand the security mindset
			- multiple session of training that might include refresh existing QCOM training
		- Preparation & Schedule
		- KPI
		- Assumption
			- Doesn't understand basic security.

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.

- Exploring code using dynamic analysis tools like debugger. for example you are interested how to reach the function you can simply put a breakpoint on the function and trigger the use-case. Once you hit the breakpoint run the backtrace command. You can achieve more advance version of this by automatically recording the backtrace each time it hit the function and dumping it in the file. This feature is provided by strace for syscall tracing for some architecture.

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

### 16/09 - 20/09

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- didn't do much, meeting
- **Tuesday**
	- Third party AR Review
	- Raised request to new folks
- **Wednesday**
	- WoS WiFi CR fix and patch review for following 5 CRs:
			1. 3817233 - there was some issue with merging the patch in one component which is now resolved.
			2. 3817238 - approved
			3. 3818037 - approved
			4. 3818109 - approved
			5. 3818118 - approved
- **Thursday**
	- spent time in AR Review
- **Friday**
	- completed self AR Review

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [ ] Sensor
		- [ ] CISPHY
		- [ ] TFE
		- [ ] CPAS
		- [ ] JPEG
- [ ] [[Sub Project - Video Driver security assessment|WoS Kernel Driver Fuzzing]]
	- [ ] Driver Fuzzing Pipeline
		- [ ] Bluetooth 
		- [ ] Camera
- Syzkaller Patch Management proposal

##### Maintainer's Task

- [ ] Build QPIL CTF challenges
	- Deadline is in October 
- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
		- [ ] rizwan schedule the meeting
	- [ ] Dhananjay Latkar
- [ ] Paper work - Gratuity Nominee Form

#### Meetings Logs
> Description what all discussion you have with what people about anything important.
- 
- #19-09-2024
	- **Title** : Linux Kernel Fuzzing Sync-up
	- *Attendees* : Scott, Joey, Gabriel, etc
	- **Meeting Minutes***
		- APT PoC : Rajesh Gandla
		- Camera dedicated setup because of "procs = 1" value. *Action Item*
		- is my linux on ubuntu 22
- #25-09-2024
	- **Title** : Syzkaller Fuzz Farm
	- *Attendees* : Murali
	- **Meeting Minutes***
		- Syzkaller Fuzz Farm
		- Syzkaller upstream patching strategy
		- Ownership of the project

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- Process to record information about the project progress and key metrics like
	- Start and End date
	- Number of CR's Raised along with grouping it by Severity
	- Code Coverage information like *Line & Function* coverage
	- Number of attack surface functions and how many of them are covered coverage
- Lex Podcast on Roman Empire
	- One of the reason for Alexzander empire collapsed much faster then Roman empire is that Alexzander didn't have any process or structure in his empire while Roman people had structure. And also one of the reason for success of the Alexzander conquer is because of systems and structure that was put by his father Phiphs II and Alexzander was smart enough to use those tools to his advantage.
	- There was also the problem of succession in Alexzander
- China used to export deflation by making things cheap and exporting it to other country. if china start increasing its cost structure. 
- Copywork - copying the existing great work by hand to understand it in much better way.
	- nice [blog](https://www.craftyourcontent.com/copywork-daily-writing-routine/)


#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

### 9/09 - 13/09

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
- **Tuesday**
- **Wednesday**
- **Thursday**
- **Friday**

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] 

#### Meetings Logs
> Description what all discussion you have with what people about anything important.
-

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- On Leave for Blueteam Con conference

#### Review
> Summary of what all things happened in this week.
- 

---

### 02/09 - 06/09

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- travelling to Chicago for Blueteam Con training.
- **Tuesday**
	- Explored downtown Millennium park
	- Yesterday slept at 6 pm and woke-up next day at 5 am.
	- Explored Michigan Avenue and Millineum Park.
- **Wednesday**
	- slept yesterday a 7 pm and workup at 4 am next day.
- **Thursday**
	- Blueteam Con [[NPRE]] Training Day 1
		- 
- **Friday**
	- Blueteam Con [[NPRE]] Training Day 2
		- 
- **Saturday**
	- Blueteam Con - Conference Day 1
- **Sunday**
	- Blueteam Con - Conference Day 2

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] 

#### Meetings Logs
> Description what all discussion you have with what people about anything important.
- 

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- On Leave for Blueteam Con conference
- [Seek Out New Protocols, and Boldly Go Where No One has Gone Before!](https://www.linkedin.com/events/7235310778696790017/comments/)
	- Youtube [link](https://www.youtube.com/watch?v=ppZ9lfj0ztw)
	- Talk on Proprietary Protocol on [[Stephen Sims]] channel.
	- Author : Douglas Mckee
	- Talk Notes
		- [[scapy]] as the goto tool for decoding and this can integrate with [zeek](https://zeek.org/) to do detection. Doesn't write decoder for wireshark but for scapy. 
		- ICS systems have protocol which are well-defined the there is option to provide customised data which is completely upto the developer to define the specification.
		- replay attack often works even in case of encrypted packets since they don't manage session well.
		- Any particular product?
			- medicial
			- ICS
			- HMI interfacing with hardware
		- its typical and 3 and 4. Mostly in application protocol, but there are cases of custom protocol.
		- What is a proprietary protocol?
			- Undocumented
			- Doesn't have RFC 
			- created by a vendor for special use-case.
		- 8 Step Process to decode the protocol
			- Simplify the Scope
				- packet capture will generate large data.
				- Reduce the dataset to a reasonable size.
				- Using the below technique reduce the scope of the data. Also called as grouping of packets:
					- protocol
						- protocol using TCP or UDP packet then they are two different protocol because the design decision will be different
					- conversations or flows
						- group by stream - one type/format of data
					- ports
						- destination port can indicate different process/thread handling the packet which also means the packet parsing code is different
					- payload lengths
						- length is an indicator of a type of a packet.
						- Similar length or the exact same length.
						- very common to see this.
					- similar payloads
						- exact same payload value.
						- or you can hash the payload and compare the hash value to see the similarity
						- you can use clustering algorithm to find similar packet or use fuzzy match to same thing.
				- Use scapy and pandas to apply above technique to do EDA(Exploratory data analysis).
					- Create Graph on all the above parameters.
					- github.com/fulmetalpackets/protohacking
					- isn't this already part of wireshark
			- Embedded Networking
				- add networking data which already exist in the application layer. 
					- Why? : because the application is only given application data and the application whats to see if the data is coming from right source.
					- if there is a port number there is a IP or mac address next/near by.
			- Enumerate patterns
				- same value of the byte pattern
				- data present at the same offset changing by constant value
					- this could be value of time incremented by certain value, usually for the heart-beat packet which is set at constant interval. Time could be epoch time.
				- this value could be of different byte size.
			- Clear-text Clues
			- Examine externally
			- Behaviour modification
			- Application reverse engineering
				- Either there is a code building it and there is a code parsing, looking at either of the code will help you understand the protocol specification.
				- He mention about the medical device which uses the custom protocol and to reverse engineer it he doesn't look at the firmware binary since there will be too much work reverse. Instead the windows client also has parsing code you can have a look at that as well.
				- search the string found in the packet capture in the 
			- Documentation with scapy
			- Client emulation
				- if you understand the protocol you can create server/client a which behaves like actual application and run it on the raspberry pi and walk around. 

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

## August

### 26/08 - 30/08

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- started migrating the patch old branch to master branch.
		- should have done proper backup as while later merging it was giving error.
		- It better to use get patch.
- **Tuesday**
	- resolved compiler and other errors and raised the pull request into main branch.
- **Wednesday**
	- pull request merged.
- **Thursday**
- **Friday**

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [x] EEPROM Sub-system
		- [x] Flash
		- [x] OIS
		- [ ] Sensor
		- [ ] CISPHY
		- [ ] TFE
		- [ ] CPAS
		- [ ] JPEG
- [ ] [[Sub Project - Video Driver security assessment|WoS Kernel Driver Fuzzing]]
	- [ ] Driver Fuzzing Pipeline
		- [ ] Bluetooth 
		- [ ] Camera
- [ ] [[On Target Firmware Fuzzing]] proposal 

##### Maintainer's Task

- [ ] Start writing Annual Review documents.
- [ ] Build QPIL CTF challenges
	- Deadline is in October 
- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
- [ ] Paper work
	- [ ] Gratuity Nominee Form

#### Meetings Logs
> Description what all discussion you have with what people about anything important.
- 

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- 
1.	IRVR Tickets
	a.	KGSL Automotive Bug review
2.	Windows Fuzzing
	a.	FastRPC Code review
	b.	IOCTL Code discovery and fuzzing
	c.	Bluetooth
	d.	WiFi
3.	Android Camera Driver Fuzzing.

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

### 18/08 - 18/08

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- wasted in procrastinating
- **Tuesday**
	- Monthly presentation
- **Wednesday**
	- Completed flash device Grammar and corpus.
- **Thursday**
	- Started working on OIS device
- **Friday**
	- Added more corpus for OIS

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [x] EEPROM Sub-system
		- [x] Flash
		- [ ] OIS
		- [ ] Sensor
		- [ ] CISPHY
		- [ ] TFE
		- [ ] CPAS
		- [ ] JPEG
- [ ] [[Sub Project - Video Driver security assessment|WoS Kernel Driver Fuzzing]]
	- [ ] Driver Fuzzing Pipeline
		- [ ] Bluetooth 
		- [ ] Camera
- [ ] [[On Target Firmware Fuzzing]]
- [x] Tech 21 & 22 August Presentation.
- [x] QPSI Monthly Meeting Presentation, to be held on 14-Aug at 10:30am-12:30pm (IST). Your slides are due Friday, 9-Aug 

##### Maintainer's Task

- [x] Carl Huang Re: Security CR: https://orbit/CR/3817188
- [ ] Start writing Annual Review documents.
- [x] Fuzz-Vision tool issue
	- [x] Not able to trace LTE RRC binary the, its ubuntu 22 and previously I have tested it on ubuntu 18.
- [x] Build QPIL CTF challenges
	- Deadline is in October 
- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
- [ ] Paper work
	- [ ] Gratuity Nominee Form

#### Meetings Logs
> Description what all discussion you have with what people about anything important.
- 

- #19-08-2024
	- **Title** : Sync-up
	- *Attendees* : Akash
	- **Meeting Minutes***
		- Setup meeting with Surendra, Nagaraju about Firmware fuzzing
		- QECP, QT-VM, QSM, Hypervisor fuzzing.
- #23-08-2024
	- **Title** : 
	- *Attendees* : 
	- **Meeting Minutes***
		- Security features which compromises user experience will be not welcomed for example if there is a bug in the SoC and it hangs the whole system and other chip when there is a bug the restart the system in graceful manner. This is what security policies help you to achieve and configure. They state what to do when a security violation is detected.
		- Networking in office to college in other team will help you to get interesting projects

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.


#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

### 12/08 - 16/08

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- merged the command buffer setup call sequence into one pseudo syzcall. Tested it with actuator and its completed.
- **Tuesday**
	- contacted Suraj about camera debug logs
	- EEPROM config_dev 1 sub-ioctl covered almost fully with manual syz-prog.
- **Wednesday**
	- wasted
- **Thursday**
	- Independence Day Holiday
- **Friday**
	- wasted

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [ ] EEPROM Sub-system
- [ ] [[Sub Project - Video Driver security assessment|WoS Kernel Driver Fuzzing]]
	- [ ] Driver Fuzzing Pipeline
		- [ ] Bluetooth 
		- [ ] Camera
- [ ] Tech 21 & 22 August Presentation.
- [ ] QPSI Monthly Meeting Presentation, to be held on 14-Aug at 10:30am-12:30pm (IST). Your slides are due Friday, 9-Aug 

##### Maintainer's Task

- [ ] Carl Huang Re: Security CR: https://orbit/CR/3817188
- [ ] Start writing Annual Review documents.
- [ ] Fuzz-Vision tool issue
	- [ ] Not able to trace LTE RRC binary the, its ubuntu 22 and previously I have tested it on ubuntu 18.
- [ ] Build QPIL CTF challenges
	- Deadline is in October 
- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
- [ ] Paper work
	- [ ] Gratuity Nominee Form

#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #12-08-2024
	- **Title** : 
	- *Attendees* : 
	- **Meeting Minutes***
		- Satish - BT Fuzzing 

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- 

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

### 05/08 - 05/08

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- worked on improving Actuator sub-system added grammar for config_dev
- **Tuesday**
	- syzkaller highlighter plugin helped to debug comment syz-prog testcase.
	- Actuator work continue.
- **Wednesday**
	- improve the grammar for cam_packet_v2 and the challenge was the offset, array size and payload size calculation which was resolved using bytesize & len operator.
	- fixed some test cases for Actuator CAM_ACTUATOR_PACKET_OPCODE_READ IOCTL.
- **Thursday**
	- nothing much just updated the corpus program as per the yesterdays grammar update
	- Had team meeting with Syzkaller fuzzing team.
- **Friday**
	- this day was wasted doing nothing.

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [ ] TPG Sub-system
			- [ ] Start working on syzlang shared buffer command
		- [x] Ask Nilo to work on JPEG PoC code
- [ ] [[Sub Project - Video Driver security assessment|WoS Kernel Driver Fuzzing]]
	- [ ] WiFi Driver Fuzzing
		- [ ] Create Axiom Crash Reproducer
		- [ ] Filed CR Discussion
			- sent the reproducer for first 4 CR's on #24-06-2024
			- [ ] Duplicated all the CR's and assigned the rating to low (Discuss it with Ranjana)
				- 3818037 is marked as parent CR of 3817212, 3817188 which is not true.
				- 3818037 is marked as parent CR of 3817212, and removed the previous relation with 3817188. This relation is still invalid.
	- [ ] Driver Fuzzing Pipeline
		- [ ] Bluetooth 
		- [ ] Camera
- [ ] Tech 21 & 22 August Presentation.
- [ ] QPSI Monthly Meeting Presentation, to be held on 14-Aug at 10:30am-12:30pm (IST). Your slides are due Friday, 9-Aug 

##### Maintainer's Task

- [ ] Carl Huang Re: Security CR: https://orbit/CR/3817188
- [ ] Windows Drivers Workshop on #30-07-2024
	- this is postponed indefinitely. 
- [ ] Start writing Annual Review documents.
- [ ] Fuzz-Vision tool issue
	- [ ] Not able to trace LTE RRC binary the, its ubuntu 22 and previously I have tested it on ubuntu 18.
- [x] Nicolas email about a CR 3847126 ?
	- CR is a clone of 3757119 which was create by Guilan Li (guilli)
	- CR Description : Incorrect action to retrieve input buffer for METHOD_NEITHER buffer mode 
- [ ] Build QPIL CTF challenges
	- Deadline is in October 
- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
- [ ] Paper work
	- [ ] Gratuity Nominee Form
- [ ] Open sourcing Fuzz-vision tool.


#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #05-08-2024
	- **Title** : Weekly Sync-Up
	- *Attendees* : Nagaraj & Fuzzing Team
	- **Meeting Minutes***
		- No one update the weekly sheet
			- Weekly Goals are not clear
			- measurable deliverable
		- Ashish - Auto Kernel fuzzing with Syzkaller
			- current coverage 18%
			- Nicolas & Scott
- #08-08-2024
	- **Title** : Linux kernel fuzzing FRs regular sync-up
	- *Attendees* : Joey, Scott
	- **Meeting Minutes***
		- Scott: fastrpc, no device, biswajit will share device to test
		- Nilo: wait for device shipping, run jpeg module for experience
		- kgsl - neeraj ooo no update
		- wlan - wifi 7 in whunt
		- camera - can generate coverage, created tool to generate description for subdevices
		- add how to write syz-desc confluence
		- Munawwar shared camera work on how to find/solve fields dependency in struct; way to query camera sub devices, written camera initial corpus
- #08-08-2024
	- **Title** : AR24 Transparency
	- *Attendees* : All Leads and Above
	- **Meeting Minutes***
		- When you are senior staff engineer you should be interacting more widely in the organization in the higher title.
		- Promotion is not always guarantee and it will depend upon if there is job opening for that particular position.
		- you need to show two to five good achievements. 
#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- 

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

## July

### 29/07 - 02/08

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- figured out how to write *command payload* pattern in syzlang and tested in on TPG command.
- **Tuesday**
	- run the the manual corpus with the syzkaller run.
	- researching for Chicago trip
- **Wednesday**
	- C program to generate the syzlang to open file descriptor for various camera sub-system. Since this varies across chipset we need to write program to generate this.
	- started working on actuator camera sub-system.
	- lrme not accessible for pakala
- **Thursday**
	- excel sheet map of all the ioctl calls and sub-calls
- **Friday**
	- QCOM Global Holiday

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [ ] TPG Sub-system
			- [ ] Start working on syzlang shared buffer command
		- [x] Ask Nilo to work on JPEG PoC code
- [ ] [[Sub Project - Video Driver security assessment|WoS Kernel Driver Fuzzing]]
	- [ ] WiFi Driver Fuzzing
		- [ ] Create Axiom Crash Reproducer
		- [ ] Filed CR Discussion
			- sent the reproducer for first 4 CR's on #24-06-2024
			- [ ] Duplicated all the CR's and assigned the rating to low (Discuss it with Ranjana)
				- 3818037 is marked as parent CR of 3817212, 3817188 which is not true.
				- 3818037 is marked as parent CR of 3817212, and removed the previous relation with 3817188. This relation is still invalid.
	- [ ] Driver Fuzzing Pipeline
		- [ ] Bluetooth 
		- [ ] Camera

##### Maintainer's Task

- [ ] Windows Drivers Workshop on #30-07-2024
- [ ] Start writing Annual Review documents.
- [ ] QPSI Monthly Meeting Presentation, to be held on 14-Aug at 10:30am-12:30pm (IST). Your slides are due Friday, 9-Aug 
- [ ] Fuzz-Vision tool issue
	- [ ] Not able to trace LTE RRC binary the, its ubuntu 22 and previously I have tested it on ubuntu 18.
- [ ] Lab Access training
- [x] Nicolas email about a CR 3847126 ?
	- CR is a clone of 3757119 which was create by Guilan Li (guilli)
	- CR Description : Incorrect action to retrieve input buffer for METHOD_NEITHER buffer mode 
- [ ] Build QPIL CTF challenges
	- Deadline is in October 
- [ ] DR Reviewer Test Candidates
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
- Paper work
	- [x] Fill Internet bills reimbursement since Jan 2024
	- [ ] Gratuity Nominee Form
- [ ] Open sourcing Fuzz-vision tool.

#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #29-07-2024
	- **Title** : Weekly Sync-Up
	- *Attendees* : Nagaraju & Team
	- **Meeting Minutes***
		- No Update
		- One conference visit this year.

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- QOT-Fuzz trouble with deep start fuzzing because the fuzzer is modifying all the 
- Chicago National parks [list](https://www.wheelsforwishes.org/news/best-national-parks-near-chicago/)

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

### 22/07 - 26/07

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- Klockwork review
	- camera fuzzing syzkaller integration
		- pseudo syscall notworking because device enumeration takes lot of time to execute so it execprog kills it halfway
		- need to kill the camera service.
- **Tuesday**
	- syz-grammar fixed for tpg and request manager.
	- How can we have syzkaller which has multiple out parameter in struct and use the same struct as input to next call?
	- Nagaraju urgently asked for 2023-24 project achievement data by tonight.
		- responded next day morning.
- **Wednesday**
	- Improved the syz-grammar and broke-down the exiting calls to more granular calls.
	- manually crafted a corpus for TPG config dev call
- **Thursday**
	- removed pseudo syscall and added syzlang for that and got it working
	- added all TPG sub-system ioctl calls except for shared buffer command
- **Friday**
	- in syzkaller config file when *procs* set to 1 fuzzing started normally, only one process can open /dev/video
	- TPG dev, dma buf has camx_packet will holds ref to other DMA buf which is used by TPG dev.
	- TPG dev all ioctl executed atleast once, now need to focus on *CAM_CONFIG_DEV*.
	- remove 90% dependency on psuedo calls and TPG is 90% integerated with syzlang.

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [x] syz-extract to extract camera constant
			- manually entering it in the .const file
		- [ ] TPG Sub-system
			- [x] syzkaller integration
			- [ ] Start working on syzlang shared buffer command 
- [ ] [[Sub Project - Video Driver security assessment|WoS Kernel Driver Fuzzing]]
	- [ ] WiFi Driver Fuzzing
		- [ ] Create Axiom Crash Reproducer
		- [ ] Filed CR Discussion
			- sent the reproducer for first 4 CR's on #24-06-2024
			- [ ] Duplicated all the CR's and assigned the rating to low (Discuss it with Ranjana)
				- 3818037 is marked as parent CR of 3817212, 3817188 which is not true.
				- 3818037 is marked as parent CR of 3817212, and removed the previous relation with 3817188. This relation is still invalid.
	- [ ] Driver Fuzzing Pipeline
		- [ ] Bluetooth 
		- [ ] Camera

##### Maintainer's Task

- [ ] Windows Drivers Workshop on #30-07-2024
- [ ] Start writing Annual Review documents.
- [ ] QPSI Monthly Meeting Presentation, to be held on 14-Aug at 10:30am-12:30pm (IST). Your slides are due Friday, 9-Aug 
- [ ] Fuzz-Vision tool issue
	- [ ] Not able to trace LTE RRC binary the, its ubuntu 22 and previously I have tested it on ubuntu 18.
- [ ] Lab Access training
- [x] Nicolas email about a CR 3847126 ?
	- CR is a clone of 3757119 which was create by Guilan Li (guilli)
	- CR Description : Incorrect action to retrieve input buffer for METHOD_NEITHER buffer mode 
- [ ] QC Trainings : Prevention of Sexual Harassment
- [ ] Build QPIL CTF challenges
	- Deadline is in October 
- [ ] DR Reviewer Test Candidates
	- [x] Alok Kediya
		- Ready for assessment
		- 9 issues are classified in-correctly and KW tuning also incorrect
		- 13 issues in 2nd attempt
	- [ ] Ramanjulu Koneni (Rama)
	- [x] Neeraj Soni
		- Ready to assess.
		- 7 incorrect issue classification and klockwork tuning exercise not performed.
	- [ ] Nitheesh Sekar
	- [ ] Sivasubramanian Ramalingam
	- [x] Mahendran P
	- [ ] Jincheng Liu
	- [ ] Anil Kumar Balanagu
- [ ] Paper work
	- [ ] Fill Internet bills reimbursement since Jan 2024
	- [ ] Gratuity Nominee Form
- [ ] Open sourcing Fuzz-vision tool.

#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #22-07-2024
	- **Title** : Weekly Sync-up
	- *Attendees* : Nagaraju & Fuzzing Team
	- **Meeting Minutes***
		- Alex G visiting in September for 2 days
			- focus on fuzzing output
			- we have our own roadmap.
		- Project K by Nagaraju
		- also bring Murali (Nov or Oct) to improve inter-team collab

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- US RSU Stocks declaration details [link](https://www.youtube.com/watch?v=NO6OlUI1Hz4&ab_channel=FinTaxPro)
- 

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---
### 08/07 - 19/07

#### Work Logs

> Just right one line description about what all things have on one each day

- I will am on leave during this two weeks leave. 
- From 08-July till 19-July.
- WFH was denied during this time span.
 
- **Monday**
- **Tuesday**
- **Wednesday**
- **Thursday**
- **Friday**

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] 

#### Meetings Logs
> Description what all discussion you have with what people about anything important.
- 

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.

- SIL Patch Approval comment "SIL patch [CR3764816-1] reviewed by qpsi -[ok]" where CR3764816-1 is the patch name

#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---
### 01/07 - 05/07

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- Bernard has issue with fuzz-vision tool, the tool is not working for some of the binary in CCE environment.
	- Interview with Divyanjali
	- Shared my Windows Hamao device with Pietro Frigo for 4 weeks
	- visited lab for machine shifting
	- TPG & Req Mngr coverage improvement.
- **Tuesday**
	- Making TPG Device info syzkaller compatible.
	- TPG sample test case refactoring.
	- Syzkaller integration of TPG sub-system sample test case
	- MTP & Windows Device and lab machine movement to lab
	- interview with Lanka
- **Wednesday**
	- Done 2 candidate DR Training test review
	- Reading Project Zero blogpost on Android Driver Research [[Qualcomm/Projects/Camera Driver Fuzzing#^qcom-linux-fuzz-ref]]
	- able to compile and run pseudo syscall for camera driver
		- not getting any coverage
		- able to generate grammar with syz-mutate
- **Thursday**
	- Travelling to Hometown Meta
	- while travelling i meet a very happy family couple with two son.
- **Friday**
	- Monthly Update
	- Had call with *Vinod Kumar Reddy Kothakapu* he can help anything related to syzkaller fuzzing.
	- Had call with Nagaraju, update the conference page for fuzzing overview.
	- Camera Driver Entry pointer analysis - its has 136 ioctl processing command update on [[Qualcomm/Projects/Camera Driver Fuzzing#^c07fb6]]
	- Applied for presentation in *Core Platform & Security Tech Day 2024*.
	- send monthly update to nagaraju

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [ ] syz-extract to extract camera constant
		- [ ] TPG Sub-system
			- [ ] syzkaller integeration
- [ ] [[Sub Project - Video Driver security assessment|WoS Kernel Driver Fuzzing]]
	- [ ] WiFi Driver Fuzzing
		- [ ] Create Axiom Crash Reproducer
		- [ ] Filed CR Discussion
			- sent the reproducer for first 4 CR's on #24-06-2024
			- [ ] Duplicated all the CR's and assigned the rating to low (Discuss it with Ranjana)
				- 3818037 is marked as parent CR of 3817212, 3817188 which is not true.
				- 3818037 is marked as parent CR of 3817212, and removed the previous relation with 3817188. This relation is still invalid.
	- [ ] Driver Fuzzing Pipeline
		- [ ] Bluetooth 
		- [ ] Camera

##### Maintainer's Task

- [ ] Windows Drivers Workshop on #30-07-2024
- [x] Update QPSI Monthly Report
- [ ] Fuzz-Vision tool issue
	- [ ] Not able to trace LTE RRC binary the, its ubuntu 22 and previously I have tested it on ubuntu 18.
- [x] Move your MTP devices and Harvester to Labs.
	- Device Restart Issue
	- Do Lab training
- [x] Nicolas email about a CR 3847126 ?
	- CR is a clone of 3757119 which was create by Guilan Li (guilli)
	- CR Description : Incorrect action to retrieve input buffer for METHOD_NEITHER buffer mode 
- [ ] QC Trainings : Prevention of Sexual Harassment
- [ ] Build QPIL CTF challenges
	- Deadline is in October 
- [ ] DR Reviewer Test Candidates
	- [ ] Alok Kediya
		- Ready for assessment
	- [ ] Ramanjulu Koneni (Rama)
	- [x] Vishu Kumar
		- Ready to assess.
		- Grant the access
	- [ ] Neeraj Soni
		- Ready to assess.
		- 7 incorrect issue classification and klockwork tuning exercise not performed.
	- [ ] Nitheesh Sekar
- [ ] Paper work
	- [x] Fill Internet bills reimbursement since Jan 2024
	- [ ] Gratuity Nominee Form
- [ ] Open sourcing Fuzz-vision tool.
- [x] Interview *Divyanjali*
- [x] Give interview feedback for Madhukar to Murli
- [x] Ranjana Email - Assessment locking missing for the CR 3804383 
	- assessment was already locked when I opened the CR on #01-07-2024

#### Meetings Logs
> Description what all discussion you have with what people about anything important.
- 
- #01-07-2024
	- **Title** : Weekly Sync-up
	- *Attendees* : Nagaraju & Fuzzing Team
	- **Meeting Minutes***
		- Move your MTP devices and Harvester to Labs.
			- You need to do some training and work with Satish to move the device.
		- Hamao Windows Machine is needed for dedicated machine for development, debugging(improve coverage) and continuous fuzzing.
		- Request access for labs.
- #01-07-2024
	- **Title** : Weekly Fuzzing Update
	- *Attendees* : Neeraj & Fuzz team
	- **Meeting Minutes***
		- Neeraj working on KGSL Fuzzing, issue with coverage outside the syzkaller.
		- Nirmal working on windows coverage.
		- Nicolas Attacker look for graphics escape, there are 78 escape Neeraj covered 4.
		- Nicolas
			- Nicolas flashed latest meta.
			- Phase 1 CR not mostly fixed.
			- Focusing on CR fixed.
			- Created full dump for WiFi last week.
			- WiFi card is disabled on Axiom by default, WiFi enabled by WMI command.
			- Created script/command to create full dump.
			- Put Tag on the CR because the tech team are tracking and fixing those tags on Priority. 
				- Tags : QPSI_WOS_FUZZING, HAMOA_WU1, FUZZ
- #02-07-2024
	- **Title** : Central Software Engineering
		- *Attendees* : Leenhart van Doorn
	- **Meeting Minutes***
		- Ubuntu running on lanai on Gunyah hypervisor with Android OS as primary OS.
			- able to run ubuntu desktop on phone and running application
		- Boot profile analyze boot sequence checkout at 
			- go/bootmetrics
			- logs saved on server
		- Instrumentation trace, itrace monitor DSP,
			- Hexagon SDK 
			- go/itrace
		- GPU Demo : Ray Tracing Enabled
- #03-07-2024
	- **Title** : Camera Fuzzing Update
	- *Attendees* : Murali, Nagaraju
	- **Meeting Minutes***
		- create user-space syscall to make call from firmware side
			- later use this to make concurrent trigger issues by making multi-threaded call from both user and firmware side
		- modem gerrit changes monitoring tool for each checking
		- zero bugs in Qualcomm Android Kernel Driver by June 2025 

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- [[syzkaller]] device definition has three parts
	- device description
		- pseudo syscall
		- grammar
	- corpus
		- manually
		- execution trace conversion


#### Blocker
> Things which blocked you from achieving your goals.
- 

#### Review
> Summary of what all things happened in this week.
- 

---

# Quarter #2
## June

### Summary
> Summarize what did you achieve in this month


### 24/06 - 28/06

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- WiFi CR Review and assessment locking
	- understanding CSLEnumerateDevice API to map all the sub-driver to fixed dev type like Sensor, JPEG, etc.
- **Tuesday**
	- understanding syzkaller Camera description driver and also understanding camera code.
		- camera syzkaller description has and the resource interlink
		- device discover is dependent on video/camera IP dependency need to abstract that out.
	- sent the request for code understanding to Suraj
- **Wednesday**
	- Linux media enumeration API to map /dev/v4l-subdev to camera sub-system.
	- started with building camxcsl unittest cases.
		- unittest need seed data and video file
		- driver usually has a UMD sdk which is the consumer of the service which can be used as reference to understand the API usage
	- progress on syzkaller harness at least the device discover
	- python devtest.py for camxcslunittest automation which uploads all the data and configuration. excellent tool and work by camx team.
- **Thursday**
	- Suraj Meeting about camera code, gave overview of code structing and abstraction
	- camera sub-dev enumeration program.
- **Friday**
	- Tested TPG PoC and refracted it into modular code so that it can be integrated into syzkaller.
	- Interview with Madhukar [[Interview Feedback and Notes#^550478]]

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [ ] Camera Driver Harness for Syzkaller
		- [x] pseudo syscall to for camera command buffer
		- [x] pseudo syscall to for camera device discover to create appropriate handler/file descriptor.
		- [x] How is mapping between /dev/v4l-subdev15 and sys-system done?
			- There are API's to enumerate interface identity
			- Linux Video sub-system provides API to enumerate this by using MEDIA_IOC_DEVICE_INFO & MEDIA_IOC_ENUM_ENTITIES.
		- [ ] syz-extract to extract camera consts
- [ ] [[Sub Project - Video Driver security assessment|WoS Kernel Driver Fuzzing]]
	- [ ] WiFi Driver Fuzzing
		- [ ] Create Axiom Crash Reproducer
		- [x] Assign CR Rating to all WiFi CR's
		- [ ] Filed CR Discussion
			- sent the reproducer for first 4 CR's on #24-06-2024
			- [x] Duplicated all the CR's and assigned the rating to low (Discuss it with Ranjana)
				- 3818037 is marked as parent CR of 3817212 3817188 which is not true.
			- 3804383 can't reproduce the CR
				- reproducer was already provided
			- 3817243, 3817216 - don't know how to handle variable size buffer.
				- validate the buffer size at the target function where you are consuming the buffer
	- [ ] Driver Fuzzing Pipeline
		- [ ] Bluetooth 
		- [ ] Camera
- [ ] [[Gunyah Hypervisor Fuzzing]]
	- [ ] Arrange meeting with Neeraj to start the work
	- [ ] Can Hypervisor Team create off-target build for Qemu?

##### Maintainer's Task

- [ ] Windows Drivers Workshop on #02-07-2024
- [ ] Ranjana Email about the CR Assessment locking
- [x] Nicolas email about a CR 3847126 ?
	- CR is a clone of 3757119 which was create by Guilan Li (guilli)
	- CR Description : Incorrect action to retrieve input buffer for METHOD_NEITHER buffer mode 
- [x] Interview *Rathnakar Madhukar Yerraguntla*
	- Compiler guy with design and verification background
- [ ] Interview *Divyanjali*
	- [ ] PhD Student
- [ ] Build QPIL CTF challenges
	- Deadline is in October 
- [ ] DR Reviewer Test Candidates
	- [ ] Alok Kediya
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Vishu Kumar
		- Ready to assess.
	- [ ] Neeraj Soni
		- Ready to assess.
	- [ ] Nitheesh Sekar
- [ ] Paper work
	- [ ] Fill Internet bills reimbursement since Jan 2024
	- [ ] Gratuity Nominee Form
	- [x] Review Form 16
- [ ] Open sourcing Fuzz-vision tool.

#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #24-06-2024
	- **Title** : Weekly Fuzzing Review
	- *Attendees* : Fuzzing Team
	- **Meeting Minutes***
		- Weekly fuzzing update
		- There is no pointer raising 1000 Bugs if tech team is not even fixing 10 Bugs. Fixing Bug is more important.
		- Nicolar working on fuzzing NPU on windows.
			- NPU is build on top of Windows Graphics Sub-system.
- #25-06-2024
	- **Title** : Round Table
	- *Attendees* : Alex Gantman
	- **Meeting Minutes***
		- QVoice survey response, People haven't heard enough of anything directly from leadership team.
		- Asked for Understand License, Alex asked how many more bugs can you find.
		- Dedicated Automation tool team to improve productivity.
			- He tried to make it before but it didn't work.
			- if you want to create automation to solve your problem you will do it more effectively than assigning work to someone else, that could be a bottleneak.
			- Automation capability is still limited by ability of the individual working on the project.
			- Needed VB Expert to automate stuff. But the problem is you have to explain your problem to the person who is not expert in your domain. That too can be time consuming.
			- Organize Hackthon for automation for a week
				- Discoverability of skillset 
- #25-06-2024
	- **Title** : QIF India
	- *Attendees* : 
	- **Meeting Minutes***
		- Binary analysis using ML to find vulnerability and binary diffing
			- binary optimization to make assembly language more useful
			- using VEX IR to make platform independent analysis.
			- Peekhole 
- #26-06-2024
	- **Title** : Weekly Meeting 
	- *Attendees* : Fuzzing Team, [[Surendra]]
	- **Meeting Minutes***
		- No working from WFH anymore. NO Weeks of WFH.
			- Take Leave
		- Weekly meeting too is getting too much
			- Weekly team update
			- Org update by manager
		- Project Delivery manager (PDM) they responsible for prioritizing product features.
		- Weekly update has increased Collaboration has increased within team.
		- CR Details and to create description such that even after opening the issue after long time you should be recollection all the details.
			- Good notes on the issues
			- [[weAudit]] plugin to capture code-path and details.
			- bug [[reproducer]] with software fuzzer or with Axiom.
		- calibration of talent has happened at hyd level
			- execution in india
			- now we are team in hyderabad and credit goes to us only.
		- Qureinum Firmware Fuzzing
			- start from july and close it in 4 weeks
			- code review campaign
			- fuzzing request chances are low
			- tech team has support for code review
			- need a lead
				- Creating understanding of the Architecture of the Project/Product
				- Reporting the vulnerability
					- CR rating / assessment
					- Tracking the fixes
					- 
		- Out of band IP security on Gylmer
- #27-06-2024
	- **Title** : One to One
	- *Attendees* : [[Surendra]]
	- **Meeting Minutes***
		- Denied WFH on vacation one week
		- PCIe & System View in NPTEL course
			- is this the course NoC: Systems Engineering: Theory & Practice
			- NoC - Network On Chip
		- ARM Confidential Computing (ARM CCA)
		- dragoncompiler.qualcomm.com
		- SMMU is to make sure read/write request is directed properly between between System Memory (RAM) and Peripherals
#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.

- syz-execprog and syz-mutate are two important tool which can be used added support for new code section.
- Understanding the camera driver KMD API usage
	- unittest need seed data and video file
	- driver usually has a UMD sdk which is the consumer of the service which can be used as reference to understand the API usage
	- Linux video(v4l) sub-system provides API to enumerate this by using MEDIA_IOC_DEVICE_INFO & MEDIA_IOC_ENUM_ENTITIES.
	- use android-ndk because of camx uapi macro error in normal aarch64 compiler. 
		- We need to figure how this can be fixed in syzkaller integration.
	- camera code register its handler with `struct v4l2_subdev_core_ops` for sub-devices like sensor, jpeg, etc. and `struct v4l2_ioctl_ops` for root devices like request manager and sync devices.
	- TPG Device
		- this device has two command buffer one retrieved in *cam_tpg_packet_parse* function and another in *cam_tpg_validate_cmd_descriptor*
	- Camera IOCTL structure patterns
- Hypervisor Project
	- Security Assurance document(like zephyr) in the open source project
	- Create a fuzzer based on LibAFL and try to open-source it with [[Gunyah Hypervisor Fuzzing|gunyah]]
- Office 45 Leave Day remaining till month of June 2024
	- 1 week leave (5 days) of Feb 2024 not applied
- Good advice on kick starting your career [link](https://docs.google.com/spreadsheets/d/1bvagpYwkqYDj_lv0mHptBu8aHeXZQJ5p/edit?gid=2117350167#gid=2117350167)

#### Blocker
> Things which blocked you from achieving your goals.

- Windows WiFi team has lost interest in fixing the CR's.

#### Review
> Summary of what all things happened in this week.
- 

---

### 17/06 - 21/06

#### Work Logs

> Just right one line description about what all things have on one each day

- **Monday**
	- Working on Pakala setup for syzkaller
	- I am able to flash kcov & kasan build from Pakala build page. A good progress
- **Tuesday**
	- Pakala building continuing
		- had chat with Joey Jiao about Pakala build he suggest latest build from the target page
		- trying another build, the one which Hashir shared didn't work
	- Cleared one DR Trainer
	- WiFi Bugs discussion
- **Wednesday**
	- Able to create Pakala KCOV & KASAN build locally
	- started working on syzkaller setup
- **Thursday**
	- Syzkaller installation cont.
	- syzkaller build completed. Joey suggested dev branch
- **Friday**
	- setting update syzkaller environment for camera fuzzing.
	- Tested IRVR PoC Code. Trying to convert it to syzkaller harness.

#### Task
> What all things you have to do and what all things you want to achieve this week.

- [ ] [[Qualcomm/Projects/Camera Driver Fuzzing]]
	- [x] Pakala KCOV & KASAN Build
	- [x] Pakala Syzkaller Fuzzing Setup
	- [x] Build and run Camera IRVR PoC Code
	- [ ] add camera pseudo syscall to for camera command buffer
	- [ ] camera driver syz-extract
		- [ ] How is mapping between /dev/v4l-subdev15 and sys-system done?
- [ ] [[Sub Project - Video Driver security assessment|WoS Kernel Driver Fuzzing]]
	- [ ] WiFi Driver Fuzzing
		- [ ] Create Axiom Crash Reproducer
		- [ ] Assign CR Rating to all WiFi CR's
		- [ ] Filed CR Discussion
			- Duplicated all the CR's and assigned the rating to low
				- Talk to Ranjana
			- [ ] 3804383 can't reproduce the CR
			- [ ] 3817243, 3817216 - don't know how to handle variable size buffer.
	- [ ] Driver Fuzzing Pipeline
		- [ ] Bluetooth 
		- [ ] Camera
- [ ] [[Gunyah Hypervisor Fuzzing]]
	- [ ] Arrange meeting with Neeraj to start the work

##### Maintainer's Task

- [x] Udhaya Kuma CR Approval
- [ ] Build QPIL CTF challenges
- [ ] Interview *Rathnakar Madhukar Yerraguntla*
	- Compiler guy with design and verification background
- [ ] DR Reviewer Test Candidates
	- [ ] Alok Kediya
	- [ ] Ramanjulu Koneni (Rama)
	- [ ] Vishu Kumar
	- [x] Sourav Poddar
- [ ] Paper work
	- [ ] Fill Internet bills reimbursement since Jan 2024
	- [ ] Gratuity Nominee Form
	- [ ] Review Form 16

#### Meetings Logs
> Description what all discussion you have with what people about anything important.

- #17-06-2024
	- **Title** : Mobile & Compute Product Security Update
	- **Meeting Minutes***
		- Tony has question what is the age of Linux Bugs
		- Not commitment form Microsoft for Kernel KASAN
		- New threat in Windows Products, GPU & NPU will be of prime interest.
		- Sukesh Chandra - we are doing better on UMD, but not better in terms of KMD Security. 2024-25 focus will be on KMD
			- ![[Pasted image 20240617230326.png]]
- #18-06-2024
	- **Title** : Linux kernel fuzzing effort
	- *Attendees* : Nilo
	- **Meeting Minutes***
		- Gave him intro about syzkaller fuzzing and some reading suggestion
- #19-06-2024
	- **Title** : Gunyah Hypervisor Fuzzing
	- *Attendees* : Nagaraju, Akash
	- **Meeting Minutes***
		- Linux Kernel Driver Bridge to call the Hypervisor system calls
		- Tech team already does some test with Parasoft for code coverage and dynamic testing.
		- Neeraj will work on this.

#### Notes
> Anything you discovered or you want to remember it for later should be put in this section.
- AR Get the list of candidates
	- WiFi Driver Bug fix Guy
	- WoS Fuzzing Guys
- Syzkaller Suggestion
	- Dockerize syzkaller build
	- Apply patches to syzkaller rather than merging upstream version in out version.
	- [TinyInst](https://github.com/googleprojectzero/TinyInst) to capture Camera Unit-test case corpus creation.
		- It has syscall hooks
- Loading Android MTP memory dump in Unicorn Emulator Talk by Christopher Wade in QPSI Monthly Meeting 10-04-2024
- Can Hypervisor Team create off-target build for Qemu?
- in weAudit plugin once you have filed CR make the CR ID as the title
- Driver Code [[auditing]] can be divided into three parts
	1. Understanding the connection between parameter of user facing API's like IOCTL, other interface define by device drivers.
	2. the middle layer is the where the state of the device/[[driver]] or process interacting with the driver is maintainted. This is where you have all lots of data structures like linked list, queue, etc are used.
	3. Third is code which is interfacing with the [[hardware]]. This layer mostly abstracted by API which in turns writes through MMIO.

#### Blocker
> Things which blocked you from achieving your goals.

- Developer Review Training Management
	- Create an email to specify all the instruction
		- not to edit klockwork issue on web dashboard
		- how to analyze klockwork issue
		- fill status and KW Tuning exercise
		- KW issues are generic code
		- expectation from the test and why are we doing it
		- Implications of security laps
		- don't copy
	- put this instruction in the excel sheet itself rather than email.

#### Review
> Summary of what all things happened in this week.
- Office task management and note taking in this *Weekly Report* style is working pretty good.

---