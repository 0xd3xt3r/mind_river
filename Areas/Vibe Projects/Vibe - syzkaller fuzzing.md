---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/vibe-specs"
created-date: 2026-08-12
related:
summary:
---

## syzkaller fuzzing

### Description

Syzkaller is grammar base fuzzer for Linux kernel.

### Pre-requisite

1. Android source code is on path '/local/mnt/workspace/mtp_builds/c70c583f-85c1-48b3-bf35-1d2b95d2f193'
	1. 'vendor/qcom/opensource' - this folder has all the source kernel driver source code.
2. Documentation for syzkaller can be found in '/local/mnt/workspace/mshelia/syz-exp/syzkaller'
	1. This folder has following files which can help you to understand the tool much better way.
		1. 'docs/syscall_descriptions_syntax.md' - Pseudo-formal grammar of syscall description.
		2. 'docs/syscall_descriptions.md' - syzkaller uses declarative description of syscall interfaces to manipulate programs (sequences of syscalls)
		3. 'docs/program_syntax.md' - Syzkaller uses a compact domain-specific language (DSL) for programs to log executed programs, test its code, and persist programs in the corpus. This page provides a brief description of the corresponding syntax. Some useful information can also be found in the existing examples and in the program deserialization code.
		4. 'docs/pseudo_syscalls.md' - Besides regular system calls, a syscall description file can also contain pseudo-syscalls. These are C functions defined in the executor. When a test program uses a pseudo-syscall, the executor will generate the pseudo-syscall function code in the resulting C program. This allows a test program to have specific code blocks to perform certain actions, they may also be used as more test-friendly wrappers for primitive syscalls. Use of pseudo-syscalls is generally discouraged because they ruin all advantages of the declarative descriptions.
3. When the device crashes you can go to  '/local/mnt/workspace/mothership/device_monitor' directory and use `uv run ftdi_device.py` to recover the device.
4.  '/local/mnt/workspace/mshelia/syz-exp/syz-fuzz-v2' - this directory has pre-build corpus in 'syz-corpus' which you can run or as an inspiration.
5. camx_src - 

### Syzkaller 

```bash
# Syzkaller build
./tools/syz-env make generate

# compile everything
./tools/syz-env make TARGETOS=linux TARGETARCH=arm64 CFLAGS=-Iexecutor/qcom_src/include

# compile only the executor, don't forget to push these binareis to target device
./tools/syz-env make TARGETOS=linux TARGETARCH=arm64 CFLAGS=-Iexecutor/qcom_src/include executor

# execute the program in the device
./syz-execprog -cover -vv 5 -debug -threaded=0 -procs 1 <test program>
```

#### Syzkaller build commands

- compile the full project `./tools/syz-env make generate`
- compile only executor `./tools/syz-env make TARGETOS=linux TARGETARCH=arm64 CFLAGS=-Iexecutor/qcom_src/include executor`, this will save time as its a small program compared to full project.
- push the binary to the target device `adb push ./bin/linux_arm64/* /data/local/tmp`

### Work flow

1. The goal of the this workflow is to improve coverage of the corpus.
2. There are two way you can improve the coverage:
	1. first is by improving the grammar which is in 'msm_<driver>.txt' in the syzkaller directory.
		1. To do this you have to understand the target driver code and make corresponding change in grammar file.
		2. after making the changes re-compile the executor and push the change to the device
	2. Crafting the corpus manually - this you will done once you are confidence you have given a good grammar and the area you want to reach is has to go through various state transition and validation, you can manually craft a corpus and run it on device and verify the logs with adb logcat -e CAM` and match it with source code.
		1. You can generate a program using 'syz-mutate' 

