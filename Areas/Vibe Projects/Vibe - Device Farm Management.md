---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/vibe-specs"
created-date: 2026-06-06
related:
summary:
---

## Problem Statement
(What problem are we solving? For whom? Why now?)

I have Linux machine lets call it as worker machine, which is has several android devices connected to it via adb which are running fuzzing test cases. While running these test cases the devices can crash and we need to run some scripts to bring devices back-up in proper function and fuzzable state. Fuzzing is done with syzkaller which run on each worker machines.

There will be several such machines which are connected to a central machine called mothership which collects stats like how much time it is spending fuzzing, or in crash state which will be sent by the worker machine to the mothership. The mothership (which is also Linux) will also send command to single machine or all the machines to execute on itself or on all the connected devices which it will collect the response and store it.

There are various repos in this directory which are already doing lots of that work. like
1. build_sync - building, and syncing the android build
2. device_farm - this collect stats by querying other machine and displaying it on TUI.
3. device_monitor - this repository has good idea about to recover, restart the device with fdi.

## User Stories / Scenarios
(How will this be used? Who are the actors?)

1. The use will interact with only the mothership to issue request like execute a command on group of devices or single device.
2. Mothership will collect stats sent by the worker machines like number of device functional
3. There are some specific cases like running program binary on each device and collecting the response for the execution.
	1. this might sometime involved upload the binary from the mothership to all the worker machine and then uploading the binary from worker machine to individual devices.


## Rough Approach
(Initial thinking on how to solve this — not a plan, just direction)

- I want this to be implemented in python service using gRPC, and the worker machine should communicate with the android device using adbutils python library.
- The worker connect with the mothership project with gPRC server and have streaming services. Server will send command to the client
- build_sync and device_monitor repo should go in client slide and device farm should be on the server side.