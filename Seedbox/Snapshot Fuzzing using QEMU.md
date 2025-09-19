---
tags:
  - "#qpsi/project"
  - "#type/qcom"
status: done
summary: QEMU snapshot fuzzing idea to implement in firmware fuzzing
related:
  - "[[On Target Firmware Fuzzing|QCOM Firmware Fuzzing]]"
---
## Notes

1. The idea is to use qemu to fuzz complex device setup like android device. Take snapshot of memory of device with JTAG and dump it on disk and then continue the execution on QEMU.
2. The current challenge is how to handle interaction with external peripherals
	1. this current solution which i have in mind is we can write qemu plugins to handle that. 
	2. The device interaction may have specific format like FastRPC, we can develop plugin that supports this format.
3. how the results from the taint analysis can be tied back to process names and non-randomized program counters

## Sub-project

1. Collecting execution traces with Qemu for continued execution and symbolize the traces
2. Memory plugin for emulating FastRPC
3. Trait tracking

##  Notes
1. Used for vulnerability management when creating PoC Exploit
2. take snapshot on every **ioctl call** and create dump and then scale the fuzzing
3. This idea is from [[Gamozo Stream Annotation#^d0c01f]] project
4. Use volatility memory forensic tool to get additional kernel information
5. Use Qemu I/O plugin to get information about what all device are been querie duing fuzzing to debugging purposed. Use the chipset I/O mapping the enrich this information.


## Ref
1. [An Architecture for Exploiting Native User-Land Checkpoint-Restart to Improve Fuzzing](https://ui.adsabs.harvard.edu/abs/2021arXiv211210100S/abstract)
2. Lightbranch: Binary Fuzzing with Snapshot-Assisted-Driven Comparison Branches Analysis
3. Nyx fuzzing
4. [Cannoli - QEMU fast tracer](https://margin.re/blog/cannoli-the-fast-qemu-tracer.aspx)