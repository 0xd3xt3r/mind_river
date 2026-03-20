---
up: "[[Personal Project MoC]]"
aliases:
  - Shaman DBI
created-date: 2024-12-16
status: in-progress
tags:
  - "#type/project/personal"
---

> **Summary**:: Shaman is a Dynamic Binary Analysis framework

## Feature Roadmap

```base
filters:
  and:
- file.hasTag("type/product-feature")
	- file.hasLink(this.file)
properties:
  file.name:
    displayName: File name
views:
- type: table
  name: Map of Knowledge
  order:
	- file.name
		- Summary
			- file.mtime
			rt:
				- property: file.mtime
				  direction: DESC
			lumnSize:
			file.name: 319
			note.summary: 834
			file.mtime: 333

```

## Project Roadmap

### User Stories

1. **Trace only selected function** - Coverage map is very huge area to cover, user have friendly way to select only the function he is interested in tracing.
2. **Visualize the collected trace data**
	1. percentage of what is covered and what is not covered.
	2. select the function and it will show what nodes are covered and what are not.
	3. syscall should be grouped by resource followed by file descriptor of that resource.
		1. first group by file operation then, group by file descriptor, sorted by time. 
3. **Start the trace** - Once we have triggered the attach binary it automatically triggers the analysis.
	1. It should have the feature the start a new a new dynamic trace for already analyzed static binary.
	2. This can be done to open a pop-up windows which pre-populates it with existing setting which you can change and restart the trace.
	3. have state-management for traces
	4. UI rearrangment for analysis - coverage map | ELF Info | Traces
4. **Device on-boarding** - currently this is buggy and in-complete
	1. Boarding can have an optional first step of telnet/ssh which will install our agent on the target device.
	2. Bug - device after on-boarding its still not sure what is going on. Have a state in database to show connection handshake was successful. Hardware info fetching is also buggy.
	3. UI navigation should be tested.

### ToDo

- [ ] #task Data-pipeline verification
	- [ ] Trace ingestion data format.
	- [ ] consuming trace data produced by client tracing
	- [ ] how to show progress for long running binary processing task. Take inspiration from Ghidra indexing UI.
	- [ ] Resume the data-pipeline from the point of failure.
- [ ] #task File system monitoring ➕ 2026-03-18
	- [ ] monitor change in file content with *inotify* kernel API.
		- [ ] this is usually done to monitor data/config files
		- [ ] These file change as an when you interact with the device.
	- [ ] monitor the binary for any change in hash - 
		- [ ] this will usually be done to monitor executable file which don't change dynamically
		- [ ] these file usually changes across version
		- [ ] we will usually monitor for hashes
- [ ] #task [[Feature - Attack Surface discovery and monitoring]] ➕ 2026-03-18
- [ ] #task [[Feature - Dynamic SBOM monitor]] ➕ 2026-03-18
- [ ] #task [[Feature Trace Data]] - What are some interesting way to shape trace data received from test target ?
	- [ ] Popular option - 
- [ ] #task OpenWRT Test Targets ➕ 2026-03-18
	- [x] Test router target for various architectures like MIPS, ARM, etc.
	- [x] https://openwrt.org/docs/guide-user/virtualization/qemu
	- [ ] Lunch 10 machines and put target client on them and execute pipeline


### Completed Task

