---
up: "[[Android Kernel Driver Fuzzing]]"
aliases:
  - Fuzzing Farm
  - Scalable Fuzzing
tags:
  - "#qpsi/project"
  - "#fuzzing"
  - "#type/sub-project"
  - type/pinned-docs
status: in-progress
related:
created-date: 2024-09-25
summary: Setup a continues fuzzing farm with all variety of devices
participants:
link: https://qualcomm-confluence.atlassian.net/wiki/spaces/QPSIFT/pages/3201042122/Continuous+Fuzzing+of+Android+Targets
---

## Requirements

- [ ] Cluster setup to scale fuzzing across different machine and have central reporting dashboard
- [ ] Focused testing for specific drivers, for e.g. dedicating 2 device for fuzzing KGSL, or camera driver for 2 weeks.
	- [ ] this will help use to enrich corpus.
	- [ ] syncing all the corpus to be distributed to full farm.
- [ ] Build refresh stability testing process
	- [ ] new build should be tested on one device before been flashed on full farm.
	- [ ] create the list of test that will be performed before flashing it to all devices.
- [ ] Cluster Heath monitoring dashboard
	- [ ] what is the uptime of the devices.
- [ ] Focused fuzzing.
	- [ ] focus on specific code section to yield good results
	- [ ] enriching corpus by parsing test cases.
- [ ] Having a debug build from source is important because we need code coverage reports which very important part of our project KPI.
- [ ] The base system will depend on the target we are fuzzing.
	- [ ] Base system : the system to which we will connect remotely and through which we will have access to device, this system will also have the code cloned and build on it, which will flash it to the device.
	- [ ] For Android Kernel Fuzzing target we need Base systems as Linux to run the syzkaller fuzzer.
		- [ ] We can install Linux on virtual environment solution in windows base system and somehow connect it with the target.
			- [ ] We will also have to connect syzkaller and the target code for symbol resolution.
	- [ ] For Windows Kernel Fuzzing base system can be Windows since our fuzzer is in windows so that won't be much of problem.
	- [ ] Also most of the open-source fuzzer are meant to run on Linux so have a base system as Linux is very important.

## Machine Requirement

1. docker - to on-board with QSBT
2. claude

## Objective

> What were you trying to achieve with this project.

1. [ ] Scalable Fuzzing setup for Qualcomm Android kernel Driver
2. [ ] Fuzzing with Pakala device 
	1. [ ] Corpus enrichment - to expand the breath of baseline corpus.
		1. [ ] we can use the unittest code to find the good parameter value
	2. [ ] Fuzzing without any instrumentation to increase the speed and accuracy of race-condition fuzzing, with hardware ASAN running
	3. [ ] Also fuzzing to uncover Race condition issues KCSAN.
3. [ ] Continuously fuzzing the Android devices through out their life in the market
	1. [ ] Recycle old device in QCOM to fuzz it while it is still in life in market
	2. [ ] two more year of fuzzing while device is in market, next year Kanapali
	3. [ ] always fuzz three generation of devices
4. [ ] Central dashboard to track all the parallel fuzzing device in-progress
5. [ ] Testing the driver code as part of CI/CD, so when there is a new code in the driver it should be sent for fuzzing.
6. [ ] Monitor the attack surface of the kernel drivers.
	1. [ ] Static analysis tools like syz-describe can help use to get the denominator value, numerator will be the syz-description.
	2. [ ] We don't know new function added to the code base, grammar only addresses existing syskalls.
7. [ ] Focused fuzzing
	1. [ ] create deployment which focuses on specific driver for example kgsl, camera, etc.
	2. [ ] It can also have special configuration to improve the fuzzing like
		1. [ ] camera has procs:1 requirement
		2. [ ] KCSAN instrumented build or build we no instrumentation
---

## Project Execution

### Phase 1 : Setup

1. [x] #task Collect all the devices in Hyderabad with [[Nagaraju]] help
2. [x] #task Hardware Requirement
	1. [ ] USB Hub
	2. [x] Lab Machines to attach MTP Devices
	3. [ ] MTP Devices which will be sent to Hyderabad
	4. [ ] Configure the syzkaller setup for multiple devices
3. [ ] Start with Camera driver fuzzing
4. [ ] Then add support for other drivers

### Phase 2 :

- #task Docker container for syzkaller execution 
	- Challenges
		- APT uses axiom, not sure it supports docker as they need to attach device into axiom too
		- pcat to collect ramdump too
		- here were issues using qualcomm tools (quts service or so)
		- qpm installation issue
- #task MPT Build image automation 

---

## Notes

---

## Important Docs
> Some of the important document which would you often need to refer very often.

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