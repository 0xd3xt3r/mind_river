---
up: "[[Learning MoC]]"
tags:
  - "#learning/course"
created-date: 2024-12-27
status: todo
title: Intermediate Intel x86- Architecture, Assembly, Applications, & Alliteration
summary:
---

## Topics

1.  Segmentation
2.  SMM

## Day 1

Explains on segmentation and pagination

### Part 1 (ignore)
1.  Intro and class content
2.  skip not important

### Part 2
1.  Review, CRUID, Begin segmentation
2.  32 Bit segmentation
3.  skip not important

### Part 3
1.  what is segmentation
2.  deep explanation of segment register GDTR and LDTR
3.  GDT and LDT
4.  how logical(code address) address is converted into linear address (virtual)

### Part 4
1.  Windbg exercise on GDT and LDT description
2.  various permission of code and data of GDT and LDT
3.  CALL GATE : inter segment jump. ??? ( need clarity)

### Part 5
1.  number of reason why segmentation is not used upto it's full potential
2.  Paging and control register
3.  description of various control register CR0-CR4
4.  CR3 stores address of the page table directory for that process
5.  each process has its own page table
6.  for all process kernel page map is to same page table directory
7.  CR4 register stores the address that has caused page fault.

### Part 6

## Day 2

### Part 1

previous day revision

### Part 2

how to see paging in action in windows kernel debugging

### Part 3

TLB (skipped for now visit later)

### Part 4

Interupts

## Segmentation

1.  all the code access is considered from segment pointed by CS register
2.  all data access is read by GDTR entry pointed by DS register
3.  all the stack data access like push, pop and call table access is done by SS register.

when the address is accessed by code it is called logical address which is then translated into linear address by consulting GDT/LDT table pointed by GDTR/LDTR later the logical translated into a physical address by the process of paging by consulting page table directory and other page tables which are pointed by register from CR0-CR4

## Paging

### Terms

1.  Memory Page : section of program/data which can be mapped into memory
2.  Page frame : the memory page which is in physical memory.