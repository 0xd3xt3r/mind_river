---
up: "[[Writing MoC]]"
tags:
  - "#type/writing/article"
created-date: 2024-12-25
status: todo
---

I have been fascinated by debuggers and instrumentation framework since i discovered Intel Pintool. Because they give you remarkable capability to reverse engineering complex software since I was obsessed with reverse engineering. 

But as I worked more on more and more projects on vulnerability research I was forced to dive into new software components and develop and understanding of the software as quickly as possible. Usually these project were not well documented and digging into the code was only course of action which was easy to reach.  

As the systems grew complex we were losing the ability to cope-up with those systems and the traditional ways. To observe the code execution that spans across different process and sub-system like co-processor we need another piece of engineering. The tool like *ARM Coresight* and *Intel PT* and other type of JTAG is the some the example which is develop to solve this problem.

When the software are executing problems are race condition occurs when there is a parallelism dependency between parallel threads/process. This class of problem are very difficult to even spot in very complex software pieces like kernel. Imagine where you have kernel and a firmware operating on a different part of SoC. You simply cannot have put debugger on different hardware and expect the halt and observe things.

Metric like code coverage is also a interesting metric that help you understand the 

The problem is not only restricted to observing code but also the data produced and consumed by the programs. Tracing the code from data point of view can also yield interesting insights. For example when you give a file to processed by a program think about all the lines of code which will be reading the bytes of the program and also think about the bytes which are untouched. Similarly when a data is written into a buffer from the file after that you are interested all the instruction that are reading and writing into that buffer. The size of read/write can help you understand the structure of the data.

As the use-cases got complex and debugging them was also complex tools like Hardware assisted debugging helps you to minimize the software execution delay.

Debugging a software very as crucial to software develop as a Disassembly tool is to a Reverse Engineer.