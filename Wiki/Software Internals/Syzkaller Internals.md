---
up: "[[Tooling MoC]]"
tags:
  - kernel-fuzzing
  - linux-kernel
  - "#type/tooling"
aliases:
  - syzkaller
  - syzkaller code walk through
  - syzkaller fuzzing
created-date: 2024-12-26
related: 
summary: framework for Fuzzing OS kernel
---



## Notes

- Executor is statically compiled binary
- There is weights assigned based on type of data structs, these weights contribute to priority of syscall selected for fuzzing. The weights are stored in *structs weights*.
- Call to call priority is stored in *struct ChoiceTable*
- Adding pseudo syscall
	- start function name with *syz_<func_name>*
- each psuedo syscall will be a bunch of in&out resource, break down the multiple call into discrete calls which will have.
- create syz program directly manually to send valid parameters.
- reach the syzkaller grammar test case to understand the language better for eg *minimization_test.go* has test cases to minimize the fuzz corpus.
- KVM also has very complex syzcall can perphaps study that dev_kvm.txt, its a mix of psuedo call on *common_kvm_arm64.h* for platform specific files are *common_kvm_\<architecture\>.h* and *common_linux.h* is a common declaration.
- BPF is another complex sub-system with shared buffers mmap, check *sys/linux/bpf.txt, bpf_prog.txt*.
- inspire the syzkaller with hand crafted syz-programs.
- Bits splitting in case of *int32:\<bits\>*. you have a fixed size data type which is later split to different bit size. All the fields should have the same datatype as *int32*.
- **Workflow:**
	- Make list of all the entry point in the driver. I usually do this with bookmarking plugin in vscode called WeAudit.
	- for each ioctl all figure out the resources which are created and consumed by the ioctl.
- **How to improve code coverage?**
	- Make sure the syz-description is updated, you can verify this by

## Core Ideas of the tool

- is converting the syz-lang declaration into in-memory layout of the c-structure for C and CPP language.
	- the declaration is done in syz-lang then the tool parses the structure and create a AST representation for that which is then used to create correct memory layout of the structure.
	- The memory layout of the structure can be different form what we write in the code because of something call structure padding.
		- if the layout is not proper the input structure will be mis-aligned and even though syzkaller is generating test cases to target particular case but since the structure is not aligned the value won't reach the desired location.
		- The memory layout it create and layed out in function *layoutStructFields* in gen.go file in compiler package.
	- by understanding type of the structure syzkaller can create better fuzzing test case or have a better mutation strategy.
	- each field type has a corresponding fuzz generation function which creates data for the field, nicolas fuzz has this as well.
- you can write a syz-prog and run and test it with the executor this is something better than nicolas fuzzer which you have to write everything in the binary from which an impossible task. 
	- This style of writing program is like running code in an interpreter.
- creating resource and placing it in bucket which will be picked-off later by some other fuzz case this is very similar to nicolas fuzzer idea.

---
## Challenges & Opportunities

1. Support for fuzzing Windows target
2. Improve the race condition test case generation algorithm
---
## Commands

```bash
# install go debugger
go install github.com/go-delve/delve/cmd/dlv@master

# original command
bin/syz-db pack test_corpuses/camx_corpus/pakala/ corpus-test.db

# start debuging program under dlv
# most of debugging command for dlv are very similar to gdb & help commands will give 
dlv debug tools/syz-db/syz-db.go -- pack test_corpuses/camx_corpus/pakala/ corpus-test.db
```

- vscode lunch configuration vscode plugin https://github.com/golang/vscode-go
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug SyzDB",
            "type": "go",
            "request": "launch",
            "mode": "debug",
            "program": "syzkaller/tools/syz-db/syz-db.go",
            "args": ["pack", "/local/mnt/workspace/mshelia/gopath/src/github.com/google/syzkaller/test_corpuses/camx_corpus/pakala/", "corpus-test.db"]
        }
    ]
}
```
---
## Docs

- syscall_descriptions_syntax.md
	- detail description of the syzlang, how to 
- syscall_descriptions.md
- program_syntax.md
- [go debugging library](https://github.com/go-delve/delve?tab=readme-ov-file)
---

## SyzTools

### syz-extract

- syz-extract needs to be run for normal build not kasan build
- *-build* is better because I have -config to syz-extract
- *-builddir* means kernel already built
- *-build* means build the kernel

### Syzkaller Debugging Setup

---

## Source Code Notes

- **fuzzer.go** - This file which takes fuzzing related decision are made here
	- **chooseProgram** -  function which chooses the next program to fuzz
	- *Fuzzer* - struct whose all the fields related to fuzzing 
		- *corpusPrios* - priority information about the fuzzing corpus. It maps corpus ID to Priority value
		- *corpusHashes* - stores the mapping to corpus object to corpus ID. The hash is the SHA1 hash of the corpus file
- **prog.go** - this file has data structure related to corpus files. Located in /syscaller/prog/prog.go
	- *Prog*  - this struct represents a program which is a per test case file
		- *Target* field has the information of target architecture on which we are running the fuzzer. The structure is located in *syzkaller/prog/target.go*.
	- *Call* - struct which has system call data and with arguments
	- *Arg* - arguments of the system call
	- *DataArg* - arguments of the system call are represented in this structure along with info type data type and in/out property.
	- Syzkaller description file are modelled using above structs and they are serialized/de-serialized to these data type when loading the corpus.
- **types.go** - core internals data type used by syzkaller.
- **html.go** - program that renders html page
- **prio.go** - calculates the priority of the syscall to fuzz.  It uses two method static and dynamic.
	- *CalculatePriorities* - This is the dynamic method of priority calculation.  The dynamic component is based on frequency of occurrence of a particular pair of syscalls in a single program in corpus.
	- *calcStaticPriorities* - Static method of calculating priority. The static component is based on analysis of argument types. Are two syscall connected by command resource, if they are then they are most likely to generate new coverage.
	- *calcResourceUsage* - this function create resource relation map and assigns priority to based on type of datatype, for example struct pointer will have weight of 10 and other primitive types will have weight of 1. 
- encoding.go - serialize/Deserialize syzkaller database for corpus
	- Deserialize - 
	- parseProg - loads corpus file
- target.go - holds information about the target which I am fuzzing target.
	- init - initialze the syzkaller data loading
	- initTarget - the data related to target
- executor/defs.h - syscall number defination
- Some compiler related project/package in pkg directory
	- ast/ - compiling the syz-lang to ast representatin.
		- format.go
		- parser.go
		- scanner.go
		- ast.go
	- pkg/compiler/gen.go
		- layoutStructFields - this function lays-out the struct in memory by doing all the alignment calculation. This solve the crucial problem of structure padding when compiling a program.
	- prog/expr.go
- 
---
## Porting to Windows OS

```cmake
cmake_minimum_required(VERSION 3.5)

# Project name
project(WinExecutor C CXX)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)

# Set source files
set(SOURCES
  executor.cc
)

add_definitions(-DGOOS_windows=1)
add_definitions(-DSYZ_HAVE_FEATURES=1)

# Set include directories
#target_include_directories(${PROJECT_NAME} ${CMAKE_CURRENT_SOURCE_DIR})

# Add executable
add_executable(${PROJECT_NAME} ${SOURCES})

target_include_directories(${PROJECT_NAME} PRIVATE "D:/qcom-dev/syzkaller" "_include/")

# DX12 libraries
target_link_libraries(${PROJECT_NAME} PRIVATE d3d12.lib dxgi.lib dxguid.lib)
```
- you need to port following files
	- subprocess.h - spawning sub-process to 
	- shmem.h
	- files.h - directory enumeration and traversal
	- conn.h - socket connection
	- executor_runner.h
		- file related system calls
		- pipe syscalls.
		- signals

## Use Case Research

1. how is syz-lang definition converted into struct
	1. Why is this important?
		1. This is done to address something three separate but related issue called data alignment, data structure padding, and packing. Data structures often have members with different alignment requirements. To maintain proper alignment the translator normally inserts additional unnamed data members so that each member is properly aligned. In addition, the data structure as a whole may be padded with a final unnamed member.
		2. structure fields are not rearranged in c/cpp. This part of language specs and this is considered to be breach of trust with the develper by the compile.
		3. padding added to satisfy alignment constraints
	2. *sys/linux/gen/arm64.go*
		1. different type have go struct `StructType` which is in */prog/types.go*
			1. UnionType
			2. StructType
			3. Type - base type for different data types
			4. Syscall - representing syscall
2. how is syzkaller syz-prog is interpreted and executed on the target device/vm.
3. How is threaded fuzzing done in syz-executor program
4. This is a pull request which adds support for new syz-lang feature [link](https://github.com/google/syzkaller/commit/4dfba277487a7023ab9f5783302da4a9b5e9bef8#diff-f98137124e6ece7b7ba09f66571b638ad34521ba13f7596b042ed5c38fcf2e57)
	1. This is interesting because you get to know what all files you need to make the change to add language feature
---
# Papers

1. [Demystifying Dependency Challenges in Kernel Fuzzing](https://github.com/ZHYfeng/Dependency/blob/master/Paper.pdf) implementation [Github link](https://github.com/ZHYfeng/Dependency)
	1. One known challenge is that much of the kernel code is locked under specific kernel states and current kernel fuzzers are not effective in exploring such an enormous state space. We refer to this problem as the dependency challenge.
2. [SyzVegaS: Beating Kernel Fuzzing Odds with Reinforcement Learning](https://www.usenix.org/system/files/sec21-wang-daimeng.pdf) implementation [github link](https://github.com/SoveraNia/SyzVegas)
3. [Syzkaller Research](https://github.com/google/syzkaller/blob/master/docs/research.md)
4. SyzDescribe : Principled, Automated, Static Generation of Syscall Descriptions for Kernel Drivers
	1. [project source link](https://github.com/seclab-ucr/SyzDescribe/)
	2. [paper link](https://www.cs.ucr.edu/~zhiyunq/pub/oakland23_syzdescribe.pdf)
5. [difuze: Fuzzer for Linux Kernel Drivers](https://github.com/ucsb-seclab/difuze#difuze-fuzzer-for-linux-kernel-drivers)
	1. Paper [Link](https://acmccs.github.io/papers/p2123-corinaA.pdf)

---

### Ref
- [syz-execprog explaination](https://mudongliang.github.io/2021/04/16/some-explanation-of-syz-execprog.html)
- [Fuzzing Network Stack with syzkaller](https://xairy.io/articles/syzkaller-external-network#-about-tuntap)
- https://blog.senyuuri.info/2020/04/16/fuzzing-a-pixel-3a-kernel-with-syzkaller/
- https://www.fatalerrors.org/a/installing-syzkaller-on-wsl2.html
- [fuzzing pixel kernel](https://blog.senyuuri.info/posts/2020-04-16-fuzzing-a-pixel-3a-kernel-with-syzkaller/)
- [Encyclopedia of graphics file formats](https://archive.org/details/mac_Graphics_File_Formats_Second_Edition_1996/page/n191/mode/2up)