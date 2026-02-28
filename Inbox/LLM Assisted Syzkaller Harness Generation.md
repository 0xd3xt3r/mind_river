---
up: "[[Qualcomm MoC]]"
related:
  - "[[Android Kernel Driver Fuzzing]]"
  - "[[Vibe Project MoC]]"
tags:
  - "#qpsi/project"
  - type/pinned-docs
  - type/vibe-specs
created-date: 2026-01-14
status: in-progress
summary: Using LLM to generate Syzkaller harnesses
participants:
  - "[[Nilo]]"
  - "[[Hashir]]"
aliases:
  - claude fuzzing
---

## Objective

- We want to have find scalable method of manage security of kernel driver.
- 75% internal bug detection.


## Current Problems / Challenges

1. How do we track the change in the code base, and how does the new change affect the security posture?
	1. How do we track new changes to the kernel driver?
	2. How can we associate the change with an entry-point?
	3. Finding the outdated struct or function grammar.
	4. Parse the structure from the Syzkaller grammar and track changes in source code.
		1. The two way relationship, 
			1. grammar to source - use analyst expertise to track source change
			2. source to grammar
	5. This tool will be called as *Attack Surface Management*.
	6. Revie Goal we should know the risk, may be might not able 
2. How can we improve the corpus and coverage?
	1. Coverage is currently been under-reported?
	2. Syzkaller is not doing depth first search?
	3. Trace the output of current unit-test cases and covert that to syz-prog corpus.
3. How can we scale the Syzkaller grammar change?
	1. This problem relates to 1st problem, find the entry point and see how to 
4. How can we use the reported CR's to enhance our security posture of the driver?
	1. Can we use this to add new pattern detection?
	2. Can we use this to improve coverage?
5. Device Farm management by Claude?
	1. Give some device for experimental configuration.
	2. Manager large farms of device to debug and report.
---

## Project Architecture
### Agent Architecture

1. **Device Farm** - This component will manage the cluster of device we deploying our fuzzer.
	1. Automation for sync, build and flash
2. **Gap Analysis** - this agent will be responsible for documenting the gap in the current fuzzing project. It will analyze the grammar and the source code to understand what all is possible to cover but we are not able to cover.
	1. Download the function/basic-block coverage report and statistics.
	2. Find the gap in the grammar/input-specs.
		1. Trivial heuristic can be employed to figure-out what is the difference between syzkaller grammar and code.
		2. Complex nested structures with pointer type-casting - for such case we can use AI analysis to find these relations.
3. **Code Analysis** - This agent will create the call graph and the data-structure graph understand of the code. This will be updated and improved as new code keeps coming. These understand will be late updated during corpus enrichment phase.
	1. **Initial scan** - figure-out the source graph, this will be later updated with more complex analysis.
		1. Source of truth
	2. **Entry Point analysis** - this is scanning for entry point for target driver
		1. **Heuristic base approach** - we know there is a standard contract between user and kernel
			1. Function calling `copy_from_user`/`copy_to_user` function
			2. `_IOR`, `_IOWR` and `_IO` def in headers. This will give IOCTL command and their corresponding structure which the IOCTL interface processes.
			3. UAPI file is usually share with user-space library.
			4. Standard `ioctl` structures, `procfs`, `sysfs` which are used to register interface for user-space.
			5. Other custom function like `cam_mem_get_buf`.
		2. **AI Base approach** - let the LLM figure-out everything other that what heuristic approach is doing.
			1. weird interface people develop like shared memory page between user-kernel. 
	3. **Pointer Analysis** - drivers have pointers which are hard to trace, this leads to incomplete accurate call graphs.
		1. Figure the all the pointers from the entry-point call-graph which are incomplete.
		2. Use the drivers life-cycle methods to figure-out pointer value.
	4. **Taint tracking** - given the attack source data can how can we create graph relation which connects call relation and the data structure relations.
		1. The call relation can be  function A -> calling function B. But the relation will be create based on the fact that function A has attack controlled data and the data is passed to function B.
	5. **Source Monitor** - periodic monitor of target branch.
	6. **Syscall dependencies** - How are ioctl/syscall related to each other by means of handler/file descriptor.
4. **Data storage** - this will store all the data generate during this analysis.
	1. **Source Graph** - this is the source graph establish during our analysis.
		1. Store will have metadata like the struct/function connection discovered by AI Agents.
	2. **Code history** - metadata about branch and commits of the fuzzing instance.
	3. **Agent Audit trails** - different types of successful and failed attempts made by AI Agents.
5. **Corpus Enrichment** - Based on grammar and the agent will write a syz-prog to test if the new grammar has added a value by writing a syz-prog which is targeting a particular function. For tracing we will use Linux kernel's ftrace infra to get the feedback.
	1. **sequence compression** - take long sequence of call and covert them in to pseudo call to not fuzz selected syscalls. This is usually done to put that driver in particular state and then fuzz the target code.
	2. directed fuzzing - Weight more function which are deeper.
6. **Agent Feedback** - how do we tell the agent if it did a good job?
	1. **Code Coverage** - Accurate coverage collection with a custom driver.
7. **Operational Environment** - 
	1. Coverage monitored - basic block feedback.
		1. Instrumentation overhead.
	2. Raw mode - no instrumentation, preferably perf build to all the fuzzer to execute on production environment.
8. **Root Cause Analysis** - pick-up the crash produced by syzkaller and try to reproduce the crash and create a Proof-of-Vulnerability(PoV) with syz-prog.

### System Goals

1. SFE uses return value to measure the progress, where as Syzkaller uses coverage metric which is a very accurate metric and comes are a instrumentation overhead.
2. Windows fuzzing is been build from scratch they are starting from zero. AI will generate a very good grammar for scratch but doing incremental improvement is a very different challenge.

### Project Brainstorm - part 1

1. Entry-points identification
	1. **Importance**: Have an automated method to enumerate entry-point can reduce human-error and also have a comprehensive list of attack surface.
	2. [ ] Entry point discovery is done using pre-defined structures and parsing the target code with tree-sitter
		1. [ ] Limitation of this method is it relies solely on source code analysis. This could include code which can be remove from final binary.
			1. The reason we do this is because, integrating with build system can complicate the solution.
				1. We can use other method of entry point validation using binary analysis of the target driver
				2. Validate it by poking the target device.
			2. Pointer analysis - this problem is difficult to deal at source level analysis, be we can then take help of binary analysis done by Bernard's work.
		2. Parse kernel C code to find entry points
			1. Linux kernel has a well define interface to expose kernel driver to the user-space
			2. Use simple C parser library to identify those user-kernel entry points.
			3. One these entry-points may not be valid because we have not extracted this from binary. Integrating build system introduces additional complexity.
	3. **Validate the entry-point** with agent program which can run inside the device.
		1. [ ] This step will only find if the device path exist and 
	4. Combining both the previous step can help you to find the function in the kernel component which will be execute along with the user-space device path.
	5. The **end-goal** of this step is to have a device path which the user-space program can connect to and the corresponding function which will be executed in the kernel driver.
2. [ ] Analyzing IOCTL entry-point to generate Syzkaller grammar
	1. **Importance** : The step will help use understand where is the gap which we need to fill. What is the outdated grammar which corresponding kernel code does the grammar belongs to and what is the gap? Also fill the gap.
	2. Validate the grammar by compiling it Syzkaller and providing feedback to the model if there are any error.
	3. Asking the LLM to generate grammar for specific entry point and validate it. Use generation and validation in the loop.
3. [ ] Generate the syz-prog to validate the grammar.
	1. **Importance**: The reason to have this step is to validate if the new grammar has really added any new coverage?
	2. Generated syz-prog will also help us to have a baseline corpus.
	3. [ ] Model will execute the prog and collect the coverage feedback
		1. Coverage feedback can be collected from
			1. syzprog -cover parameter
			2. adb logcat
			3. ftrace
			4. kprobe

---

### Project proposal

[Android fuzzing Roadmap](https://qualcomm-my.sharepoint.com/:w:/r/personal/nredini_qti_qualcomm_com1/Documents/Documents/Android%20Kernel%20Security%20Projects%20and%20Roadmap.docx?d=wa7f3b6319b784154a2afcf5439628493&csf=1&web=1&e=gJXgiO)

### P1 - Attack surface management

- The goal of this project is to pin-point the changes that are coming the kernel driver and how does that affect the entry-point.
- For this we need to clone the git repo and build the symbol index for all the branches and store the data in either json file or sqlite-db. The stored file should have branch name and the git commit at which the scan was done.
- Input to the system will be git url
- Spec
	- All the code should be written in python
	- the code we will be scanning will be strictly C code for Linux kernel.
	- The index should store symbol and cross-reference information
	- update to a symbol will notified to a users
	- When there is a new commit try to map in which symbol the change has occured, like funcition 
Break down the task and store the work plan in 'beads' in the form of epic and task

- **Symbol Tracking**
	- I want to re-design template feature, and rename it to 'Symbol Tracking'.
	- the input will be all the type which are currently available tracking in the symbol database.
	- In the new design I want the input from to be a new page instead of a model. Input file will be the type of symbol & the name. For example type can be function and the value will be name of the function. There will be multiple of such tacking, so when the add button is list it will be added to the list of variable which will be track.

#### Task

- [ ] Change management
	- [ ] Change wrt to Syzkaller metadata
		- [ ] Structure change
			- [ ] new field
			- [ ] change data type
	- [ ] Change wrt to commit-version
		- [ ] store the snapshot of the current scan
		- [ ] diff the change
	- [ ] link the change to the entry point via call graph
	- [ ] This type of structure
		- [ ] kernel has standard way of defining IOCTL command patterns. parsing that will help use extracting low hanging fruits
		  ```C
		#define IOCTL_KGSL_SUBMIT_COMMANDS _IOWR(KGSL_IOC_TYPE, 0x3D, struct kgsl_submit_commands)
		  ```
- [ ] Entry point management
	- [x] scan for all IOCTL methods
- [ ] Mapping the Product with branch

### P2 - Device Fuzzing Farm

**Owner**:[[Hashir]]

#### Task

- [ ] Baseline setup 
	- [ ] Create a dev environment
	- [ ] Setup 8 devices
	- [ ] Connect the all the device to a single syzkaller instance
- [ ] We should refresh the build every two week and 
	- [ ] Capture the commit of all the drivers we are fuzzing.
	- [ ] Once we have refreshed the build we need the check the new commits and see what has changed
	- [ ] Change will be analyzed to see if we are fuzzing the new change.


### P3 - Coverage gap analysis

**Goal** - The goal of the project is to improve the current lag in the fuzzing coverage?
**Approach:**
1. Analyze the IRVR tickets
2. Understand the current gap in the Syzkaller grammar.
3. If the grammar is complete, investigate why is the Syzkaller not exploring the code path?

### Task

- [ ] Drivers for Ashish
	- [ ] FastRPC
- [ ] Drivers for Hashir
	- [ ] Camera
- [ ] Driver for Munawwar
	- [ ] KGSL

### P4 - Agentic code review

1. Create workflow that help you review the command bug patterns.
2. Review for function like `copy_to_user` and `copy_from_user`.
3. Create pattern by analyzing IRVR incidents.
4. Domain specific review
	1. In-case of camera function like `cam_mem_get_cpu_buf` and `cam_mem_get_io_buf`

---

1. Syzkaller to SFE Bridge
2. WoS Fuzzing Farm
3. UEFI Fuzzing
4. Nicolas
	1. Automation
	2. RCE for Android and Windows
		1. Automated Patch and verification (Require PoV)
	3. Infrastructure should be up and running
