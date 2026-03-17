## Problem Statement
(What problem are we solving? For whom? Why now?)

I am trying to solve the problem of binary reversing by dynamic binary instrumentation testing.

## Goals
(What success looks like — specific, measurable where possible)

I want to collect binary coverage trace for processes running in the device which host our agent.
  

## Non-Goals
(Explicitly out of scope — prevents scope creep)

  
## User Stories / Scenarios
(How will this be used? Who are the actors?)

The analyst will be able to select a running process to which it has to attach shaman-tracer or lunch a program under its tracing.

## Constraints
(Technical, business, timeline, or resource constraints)
The memory of the device is very limited so were collect trace in shared memory and stream it to the gateway via agent for further processing.

## Open Questions
(Things you already know are unknown — the review will find more)

  

## Rough Approach
(Initial thinking on how to solve this — not a plan, just direction)

I want to start binary analysis on the binaries we download. You need to design analysis tab for that in project page. Once you download file in archive you can select binary analysis phase. There is an option in action drop down menu.

Once you press analysis this should trigger a background job and it is computationally intensive so you might have to use 'celery' task manager with redis backend and the jobs results will be stored in sqlalchemy orm database and i also want a way to show the job progress. results in the project tab.

Binary analysis will be done in multiple phases, this phases is handled in the celery :

1. this will stored the binary metadata like sha-256, last analysis run on, created on. This is phase on and should be relatively quick. Next.
2. start the ghidra analysis which extracts the function and basic block address. These address will be stored in the backend and also serialized in the form of file format which will be sent to the device, lets call this coverage map file. You will find the ghidra script in /home/munawwar-hussain/pdev/shaman-core folder. understand how the function and basic address are stored and design data model for it.
3. The goal of this feature is to collect execute traces for device and show display what has been executed. The execution traces will be most like sub-set of coverage map file.

Other Changes are:

1. The location on the device where any file upload is done should be configured for the device. The setting will be attached to the device meta data. You have design UI configure this property and store it in the device data models. Also show maxium size of the directory this is so that we dont overwhelm the target device default value should be 50 mb.
2. Do a simple read write test on the configure device path. Then upload the file on that path, also store this path in the database. This is to improve the robustness.

## Design

1. The execution trace will be streamed from device to gateway and then to the job queue for further processing.
2. From the analysis phase we can select a binary from our file archives for analysis, then start the analysis.
3. When the analysis tab is selected it should show the list of existing binaries for analysis. Once we click that we get a detail analysis page which has things like coverage map file UI, execution trace record and ELF file rendering. This view should be tab panel just like project page.

## What should be the UI/UX

1. The progress of the each of the phase should be show in the analysis tab which will have list of binaries give for analysis. when the analysis is clicked the open the analysis detail page. The page should open only if analysis has completed all its phase.
2. The progress should be streamed via websocket 
3. Design a UI for coverage map file.