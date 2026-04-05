---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/product-feature"
created-date: 2026-03-18
related:
summary:
---

## Problem Statement
(What problem are we solving? For whom? Why now?)

I have linux machine lets call it as worker machine, which is has several android devices connected to it via adb which are running fuzzing test cases. While running these test cases the devices can crash and we need to run some scripts to bring devices back-up in proper function and fuzzable state. Fuzzing is done with syzkaller which run on each worker machines.

There will be several such machines which are connected to a central machine called mothership which collects stats like how much time it is spending fuzzing, or in crash state which will be sent by the worker machine to the mothership. The mothership (which is also linux) will also send command to single machine or all the machines to execute on itself or on all the connected devices which it will collect the response and store it.


## Goals
(What success looks like — specific, measurable where possible)

## Non-Goals
(Explicitly out of scope — prevents scope creep)

## User Stories / Scenarios
(How will this be used? Who are the actors?)

1. The use will interact with only the mothership to issue request like execute a command on group of devices or single device.
2. Mothership will collect stats sent by the worker machines like number of device functional, fuzzing stats by reading it from syzkaller services.
3. Mothership will send command to pull the latest syzkaller code from git repo, followed by building it and running it.
4. we can map a shared network path on all the worker machine and the mothership this will optimize large file transfer.
5. There are some specific cases like running program binary on each device and collecting the response for the execution.
	1. this might sometime involved upload the binary from the mothership to all the worker machine and then uploading the binary from worker machine to individual devices.

## Constraints
(Technical, business, timeline, or resource constraints)

## Open Questions
(Things you already know are unknown — the review will find more)

## Rough Approach
(Initial thinking on how to solve this — not a plan, just direction)

- I want this to be implemented in python service using gRPC, and the worker machine should communicate with the android device using adbutils python library.