---
up: "[[Qualcomm MoC]]"
tags:
  - "#knowledge/linux-kernel"
  - "#type/qcom"
summary: Linux kernel training in qualcomm
---
# Auditing Linux Kernel Code

[Course Link](https://learning.qualcomm.com/course/view.php?id=23391)

## Concepts

1. **Taint** is a important concept. A data is said to be **tainted** if it comes from untrusted source i.e. user controlled.
2. If validation on the tainted data is done its called **sanitized input**.
3. The tainted data has to be sanitized before it is safe to use.

## Sites of Interest

These are some of the potential location at which bug could occur. You can backtrack from here and figure-out if the that input can be controlled by user.  

1. Array indexing
2. Copy size
3. Loop variable
4. Memory allocation expression
5. Pointer de-references
6. Other

## Information Leak

1. Kernel data can be exports from kernel to user from two location, *copy_to_user()* and kernel debug output.
2. You can leak addresses, stack canary or crypto keys.
3. Accessing an uninitialized data like local variable.
4. Data could also be leaked via C-struct padding. Padding is place between or end of the struct to make CPU aligned memory for performance reasons. These padding are not accessible directly by in programming which could lead to data leak.
	1. 'Poke-a-Hole' pahole is a tool to debub struct padding. It can emit debug information about the padding which is placed between the struct elements. It can also help you figure-out arrangement in which padding can be minimized or eliminated.
	2. Use *memset()* to fully initialized stack/heap allocation.
5. don't print pointer in debug information. instead use *%pK* to print pointer to kernel virtual address only if you are root user.

## Stack Overflow
1. Each process has fixed sized stack which is architectural dependent.
2. In stack overflow you can over-ride thread_info struct to archive interesting results.
3. There are bunch of stack based attack called stack jacking.

## Reference

1. https://en.wikipedia.org/wiki/Data_structure_alignment