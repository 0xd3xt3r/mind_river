---
up: "[[Knowledge Base MoC]]"
tags:
  - "#type/knowledge"
status: todo
created-date: 2025-03-27
related: 
summary: Crash data collection tool
---

## Notes

There are two software which will help you with this 
1. pcat - this tool will help you to collect 

## Pcat

![[pcat-menu.png]]

1. First connect the device while it is in working condition.
2. Then trigger the crash.
3. Once the crash is trigger, you will see some output in the "Activity Log". Then you can press the "collect" button.

### Crash Scope

![[crashscope-menu.png]]

1. Meta build - windows path to the meta build of your system
2. Log folder - path of the folder with has pcat dump
3. Output folder - output folder where of the crash scope analysis

one the analysis is completed there will be a link to ".csr" file on the log window and once you click the file you will see something like below.

![[crashscope-output.png]]