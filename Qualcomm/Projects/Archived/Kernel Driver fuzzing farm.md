---
tags:
  - "#type/sub-project"
  - "#qpsi/project"
status: archived
up: "[[Android Kernel Driver Fuzzing]]"
created-date: 2024-12-14
---

> **Summary**:: The goal of this project is to scale the fuzzing effort to multiple device.

## Requirement

- Device Procurement and Setup Requirement
- Having a debug build from source is important because we need code coverage reports which very important part of our project KPI.
- The base system will depend on the target we are fuzzing.
	- Base system : the system to which we will connect remotely and through which we will have access to device, this system will also have the code cloned and build on it, which will flash it to the device.
	- For Android Kernel Fuzzing target we need Base systems as Linux to run the syzkaller fuzzer.
		- We can install Linux on virtual environment solution in windows base system and somehow connect it with the target.
			- We will also have to connect syzkaller and the target code for symbol resolution.
	- For Windows Kernel Fuzzing base system can be Windows since our fuzzer is in windows so that won't be much of problem.
	- Also most of the open-source fuzzer are meant to run on Linux so have a base system as Linux is very important.
- Our requirement falls into three categories:
	1. Where we can plan device? We want x num of device for security assessment project.
		1. what is device plans?
		2. what are different version? and variations?
	2. When we decide to do security assessment project,
		1. we should have infra and device requirement?
		2. can farm provider help use to meet our requirement?
		3. Axiom will provide support for our requirement?
	3. IRVR generated requirement? Adhoc requirement?
		1. get device from storage?
		2. Lead time will be high
		3. they can setup old device which are