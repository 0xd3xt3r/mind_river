---
up: "[[Project Shaman]]"
tags:
  - type/vibe-specs
  - type/product-feature
created-date: 2026-03-15
related:
  - "[[Vibe Project MoC]]"
summary: Binary analysis and tracing
---

## Problem Statement
(What problem are we solving? For whom? Why now?)

I am trying to solve the problem of binary reversing by dynamic binary instrumentation testing or embedded/IoT Devices which are running Linux OS. We are doing this to do the security assessment in the device. The target devices will be of different architecture like ARM, MIPS, SPARC, Power PC, etc.

I want to start with binary analysis approach of tracing the execution path, and system calls.

I want to solve this problem so security analyst such that he can think about the problem from full device perspective. He should be able to see what process are running, what process are accepting data from network and what data are they sending to the network.

The word tracing/observing is synonymous for the rest of the document.

## Goals
(What success looks like - specific, measurable where possible)

I want to collect binary coverage and system-call trace for processes running in the device which host our agent without too much performance degradation. For now I only want to observer network and file system related system calls.

## Non-Goals
(Explicitly out of scope — prevents scope creep)

We are NOT doing malware analysis we are only interesting in observing the execution trace, system call trace of the program.

## User Stories / Scenarios
(How will this be used? Who are the actors?)

1. Each analysis is tied up with the project and it will be done on one of the devices attach to the project. Same binary/process could be running on multiple devices but the captured data should have the metadata indicating form what device it was captured.
2. The use will come to the project view and the go to the Analysis view to start the binary analysis/tracing.
3. The analyst should be able to start tracing the program for the target device. There are three possible scenarios on who it could be done:
	1. Uploading the binary from existing archive of files.
		1. In this case the analyst should be able to select where this binary will land as not all the file path on the target device are writable or else this might fail.
	2. Attach to a running process in the device. We should check if we can attach the debugger to the running process as it might fail due to permission issue.
	3. Invoking a binary program residing in the device. The agent have the ability to make ptrace system call or it have necessary space disk to write the coverage map file.
	4. To start the analysis use will select form one of the above scenarios to start the analysis.
4. This is going to be a complex multi-stage process with many failure along the way, so it very important to track each stage and log error/success of each stage and option to diagnose and restart the problematic stage. Have a UI displaying the progress of each stage can give confidence to the User about the progress.
5. Regarding the tracing behavior
	1. We should be able to put the target process under-observations for weeks or months.
	2. We should also be able to trace multiple process at once.
	3. We should be able to start, pause, resume and stop the tracing.
	4. We should be able to replay the tracing from time series perspective and overlap multiple traces base on timeline since we can have weeks long traces.
6. There will be multiple execution traces for program we should have a view to show all the traces and on clicking it should give a small overview like
	1. Function covered and basic-block covered count. Time and date it was recorded, and how long was the trace in terms of time.
	2. some notes recorded by the analysis, if any.
7. An analysis should be able to select the process/binary which he thinks is important for his tracing and he should be able to record some notes for the process/binary so to recall later why he was interesting in the program/process.
8. The analyst should be able to visualize the execution traces from multiple view like table, graph, etc. This data will be overlapped on existing call-graph or function graph of the binary. Execution trace will only show what call path have actually been taken.
	1. The execution trace information like order of execution of basic-block/function and now frequently it was executed. 
	2. The UI should visualize the data from different perspective as this might unlock new insight in the applications behavior. It could be table, graph or time series data.
	3. The analyst should be able to record not about the execution trace these things could be insight he has gained by observing certain all function/basic-block call path. The insight should be recorded in the markdown format and rendered on the UI itself.
9. View the details of the binary like ELF file, external dependencies and imports and exports.
10. Device upload configuration management.
	1. The will be configured path on the device where will write needed file and store our agent binary. 
	2. There should able configurable upper limit on how much file/space we can write and show the error/warning if we are approaching the limit and show error if we are crossing the limit. 
	3. Device view should show how much percentage of space already use in that folder. 
	4. This setting will be attached to the device meta data. You have design UI configure this property and store it in the device data models. Also show maximum size of the directory this is so that we don't overwhelm the target device default value should be 50 mb.

## Constraints
(Technical, business, timeline, or resource constraints)

1. The target device is a very MEMORY and CPU constrained, so the collect trace in stored in memory and stream it to the gateway via agent for further processing without writing it to the disk. So it is important to do only the most important task that can only be done on the target device should be executed on the device and the rest of the operation/processing should be done on the external program and agent sitting on the device will provide the necessary information.
2. Tracing the execution path, and system calls should be configurable at the start of the tracing process. This is important because either of the two option has performance overhead and doing both at same time will have even more overhead and can significantly downgrade the target programs performance ultimately leading to change in program original behavior.
3. The security analysis should have access to the system shell via telnet/ssh or any other means and should be able to run our agent on the device. Also the agent have the necessary permission to trace the process via ptrace system call. This is extremely important and very important assumption.
4. We will be usually dealing with older version of Linux kernel like 3 and above and toolchain as old as gcc version 4.6.3 or Buildroot dated around 2015.

## Open Questions
(Things you already know are unknown — the review will find more)

 1. A system under the test has many process running, how do you select the important one? Sometime the user is already focus on what program he want to trace/test but there are also time we have to help them discover what is interesting.
 2. What is some good coverage trace visualization we could use to unlock new insight in the program behavior, I have already considered data table, function/basic-block graph. Do you think we can do something from time-series perspective?
 
## Rough Approach
(Initial thinking on how to solve this — not a plan, just direction)

1. To collect the execution trace of the process we need the all the address of the function and basic-blocks we will use ghidra script to extract this information from the binary and store it in serialization format which will will send it to device via gateway to the agent which will later use it to invoke shaman-cli program with file name as the parameter.
2. Coverage map file is required for execution trace, system call tracing doesn't need it.
3. For visualization you should use d3js it is very capable, stable and popular library.
4. For binary analysis or disassembly related activity we can use ghidra. We start the ghidra analysis which extracts the function and basic block address. These address will be stored in the backend and also serialized in the form of file format which will be sent to the device. lets call the extracted file as coverage map file. You will find the ghidra script in /home/munawwar-hussain/pdev/shaman-core folder. Understand how the function and basic address are stored and design data model for it. Ghidra will be downloaded and in our setup and the path will be configured via configuration file. Current script doesn't capture basic block edges, copy the script and update it with the new functionality.
5. for serialization and de-serialization use nanopb library which we are already using in our current project.
6. All the communication to/from the device will be done via gateway. For example send file, receiving trace data, etc. The execution traces will be streamed from device to gateway and then to the job queue for further processing.
	1. Trace file should have meta data which stores following information like
		1. Binary sha-256 for which the trace was collected. We should match multiple trace for same binary based on this information.
		2. Name and path of the binary, and device on which it was collected.
		3. Total number of function, basic blocks executed and total number of data-points.
		4. Start and end time of the trace, and the total time for which the trace was collected for example 1h and 10 minutes.
7. There are many task which will require asynchronous task queue for that we will use celery with redis backend. These task will be as follows
	1. Executing Ghidra script to extract coverage map this could sometime take 30 min to 60 min complete also this is computationally intensive task.
	2. The result of the task should be stored in database which uses via sqlalchemy.
	3. Binary analysis and trace processing could involve multiple phases it would be best to break them down into multiple task so that if they fail we could try execute it again and continue the the analysis. I am listing some of the phases.
		1. Start with basic sanity checks like can we write to the target path. Does it have sufficient space as per our configuration.
		2. Does the target program support by use i.e. it and executable file(ELF file).
		3. To start program tracing it is important to have that binary first in the Backend to create coverage map file. So if the binary is not present we will start by downloading it, this usually happen when we are tracing a running process or invoking a binary residing in the device.
		4. Next, Store binary meta-data like sha-256 has value, binary size, etc, ELF binary info(section program headers, dependencies, etc.
		5. Extract coverage map with ghidra analysis
		6. push the coverage map to the device.
8. trace files should have retention policy like 1 year, which configured for each project. 
9. The progress of the analysis should be streamed to the UI via websocket, and if the user is not online there is no need to do that simply collect data via gateway forward it to the celery. 
10. Backend, Task queue and gateway share the file path which it will use to read/write files for a particular project. We already have that feature call file archives. We should use that or purpose some update if it is not sufficient. We will use this to write/share coverage trace, target binary, coverage map, markdown notes, etc.
11. When the analysis tab is selected it should show the list of existing binaries for analysis. Once we click that we get a detail analysis page which has things like coverage map file UI, execution trace record and ELF file rendering. This view should be tab panel just like project page.
12. The first phase of the project plan should start with UI development, this will give us and idea how everything thing will play out and the we can start implementing the rest of the system.