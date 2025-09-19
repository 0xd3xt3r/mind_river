---
tags:
  - "#type/writing/social-media"
status: published
created-date: 2024-12-15
---
## Post #3 - A Powerful tracing engine based on Qemu

Dynamic Tracing engines are crucial tools in Reverse Engineering. By executing a desired use-case and collecting code coverage, you can effectively narrow down the sections of the binary to refine your understanding of the program. While dealing with a MIPS binary reversing challenge, I came across a tool called Cannoli, which provides tracing capability in Qemu User-mode. It allows you to write plugins to trace execution paths and memory operations like read and write. What’s most fascinating about this tool is not just what it does (as there are other tools that also do this), but how quickly and elegantly it accomplishes its tasks. In other words, I was captivated by its engineering.  
  
The tool’s author patched Qemu to expose some of its internal functions, allowing you to inject your own code into the JIT code emitted by Qemu for execution. This is achieved by providing two callbacks: one before an instruction is lifted and another before an instruction performing a memory operation is lifted in Qemu. The real work is done by the code you inject into the JIT code. This custom code exposes execution trace and memory operation data via IPC to another process, which then post-processes this data.  
  
Essentially, you’ll be writing the data consumer library that is sent via IPC. The IPC design is also interesting. It uses shared memory-based IPC, where you allocate a large block of memory that is divided into smaller chunks. The idea is to use chunk sizes that match your CPU cache size to avoid cache misses, thereby improving performance. The design supports a single producer and multiple consumers. A single write-only chunk is available to the producer, and once the producer is done, it releases the buffer to be consumed. The consumers then post-process the data, clear it, and release the memory chunk to be reused by the producer.  
  
One important thing to note is that this tool doesn’t allow you to modify the behavior of the executing program; it only allows you to observe the program’s behavior. Despite this, it’s still a very powerful tool. All of this is achieved by introducing about ~200 lines of code into QEMU. There’s a lot more to discuss about this tool that can’t fit into this small post. I would recommend checking out the project link and the blog post that discusses these tools in depth.

Project link : https://github.com/MarginResearch/cannoli

https://margin.re/2022/05/cannoli-the-fast-qemu-tracer/
https://margin.re/2023/02/harness-the-power-of-cannoli/