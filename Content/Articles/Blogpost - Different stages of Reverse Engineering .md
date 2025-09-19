---
up: "[[Writing MoC]]"
tags:
  - "#type/writing/article"
status: todo
created-date: 2025-04-27
---

I have been reverse engineering for quite some time now.

## Phase one - Grasping the basics

When I first heard about the dark art of reversing I was intrigued and wanted to learn if magic so that I could proudly call myself the binary shaman. The journey was rough than I expected it to be.

You should first start with learning assembly language this will expose you to the mental model required to understand how CPU exposes it functionality. How memory is allocated and laid out. I am assuming you have some expose to C/C++ program, C language is very near to the assembly that you even decompiler generate pseudo code which reassembles C code. Program written is assembly are in no-mans land you can create memory layout the way you want it!.

But ain't we reversing from binary to C program? Yes, but you need to have a good grasp of underlying assembly language, you might as well implement some high-level programming primitive like function call, loops and conditional statements. And these are the basic primitive and which all other primitives are built on, like classes and inheritance.

A thing about primitive is very prominent in programming language is it is everywhere, in real life too! I will take the analogy of constructing a house. The we look at mansion it look gigantic and beautiful but try to deconstruct start taking it apart it boils down to simple primitive for example A big house i made of multiple floors, each floor is combination of different kind of rooms. Each room is made out of walls which is then put to gather by laying bricks glued together by bricks. Bricks are themselves made of soil. When the whole structure is plan the engineer designs the structure so that it can take a lot of load and how the user of the building will be navigating once they are inside. I want to put aside all the aesthetically feature like paints, flooring and electric and water layouts! I hope you can see the building from the Mud to Mansion. We will be using this analogy for the rest of the discussion so have it loaded in your mind.

Then start writing small programs and C and disassemble these binaries. Start with a simple programs where you just have print statement, later start introducing small complexity like creating variable and passing then around functions and see how it is handled in function. Later have complex data structure likes structs and linked list, tree and function which process these data structures.

Any binary program is a combination following choices:
1. Executable file format, ELF, mach-o, PE (Windows), etc.
2. Different ISA like ARM, x86, etc.
3. Compilers gcc, clang.
4. Language like Rust, Golang, C and CPP.

## Phase two - Hypothesis Validation

When you are reversing what you are essentially doing is looking for patters. Like what does a function call looks like, for example how are function parameter passed to the function and how is the return value passed to the callee. There are many different pattern for this for example a very straight forward thing will be to return the value from the function. This is a very simple use-case, but one the function parameter is a point to a struct or any other data type and return value is written to that value. Dis-assembler engines have identified this pattern easily and have engines turned to find and flag such patterns this is how call-graphs and inter-procedure graphs are created by these engines.

## Phase three - Automation to Scale

1. Debugger
2. DBI

## Outline

Points to consider
1. co-relating source-code compiled binary
	1. co-relating the binary with and w/o symbols
2. Reversing library interface
	1. reversing documented interface with the program which is consuming the library.
	2. compiling and open source with the tool-chain matching existing program and creating signature using Ghidra / IDA.
3. Library of Ideas
	1. read the code of open-source program of the type of binary you are reversing this help you understand common design patterns. This will help you to create ideas and exposing yourself to different kind of programs broaden your perspective about software and you will be able to create good hypothesis while reversing.

## Reference

