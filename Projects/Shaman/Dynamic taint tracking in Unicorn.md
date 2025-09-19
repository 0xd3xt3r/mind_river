
## Problem Statement

Give a memory buffer can you give all the instruction which are doing read/write operation on that buffer.

## Terminology

1. Machine State - the value of register and Memory of process

## Solution
Using unicorn emulator by emulating very focused section of code we can register a callback function for of any memory(READ, WRITE & FETCH) access done by code. In this callback we get following information
1. this memory is being READ, or WRITE
1. Address where the code is being executed - this we can record
1. size of data being read or written
1. value of data being written to memory, or irrelevant if type = READ.

This way we can track what all instruction are manipulating(read/write) what all data. This can help you to do following
1. Reversing data format - by tracking what data are you reading and writing you can try guessing the data structure of the buffer. This buffer data can come from network/file read/write. This is will help you to do better manual analysis.
2. Fuzzing on focused function - by tracking the bytes are access by which instruction we can bucket the instruction by the function they belong. Then in the fuzzer we can give high priority to files which reach those section and also in the file we give high priority to bytes which are influencing the function. we have not accounted for the indirect influence where a byte is deciding which function to execute.

This is a dynamic analysis technique where are have to run the target process. 

Since this not a fully automated analysis

Since this not a fully automated analysis what we have to do is take snapshot of process using tool like gcore and load the snapshot in the unicorn and place memory callback hook do the analysis and mark the buffer which you are interest to track and run the analysis.

We need analyst to intervene for following information [link](https://github.com/unicorn-engine/unicorn/blob/7b8c63dfe650b5d4d2bf684526161971925e6350/include/unicorn/unicorn.h#LL408C65-L408C65)
1. give memory buffer he is interested in tracking
1. Load the process in unicorn emulator
1. figure-out on where to halt the process to take snapshot which has to be loaded in emulator.

You can all write this operation in any DBI tool but doing this in unicorn is that this analysis can be used with any arbitrary architecture. Then all you have to do is bring the process code and data in unicorn and run the analysis.

Next stage of this idea could be to add taint tracking, so basically the problem with above approach could be that the buffer is copied in another buffer and then the operation are done on that buffer which can hide instruction which are operating on the data. What we can do is identify the memory copy instruction and add the newly copied buffer to the tracking list and relate the copied buffer to parent buffer this way we can establish the instruction operation relation with main buffer we are trying to track. The tracking might have to be done at recursive manager and the parent buffer might be copied in other buffer which is then copied to other buffer and so on. The data could also be copied in register we might also have to make the register as tainted.

## What is Taint analysis

Taint analysis usually allocate shadow memory which is essentially the tracks memory at various granularity. So a 1 bit in the shadow memory corresponds to 1 bytes in the target process and if the bit is marked then the corresponding byte in target process is tracked and when ever you are doing operation and you encounter the marked bit is copied in from then the target byte is also marked as tainted. Along the way registers are also marked as tainted if they hold the tainted data and if they write the tainted data to memory then the destination memory is also marked as tainted.

The above logic only deals with memory operations is coping of data from memory to memory or register to memory and vice-versa. But what will you do about arithmetical operation or logical operations? How is taint propagate in those case? This can be a configuration able setting in which you have to decide at what granularity you want to do the taint-tracking so for example if in arithmetic/logical operation if any of the parameter is tainted resulting data will also be tainted this can be a configuration. The tainting can also be done at different granularity level like for example 4 byte in target process could correspond to 1 bit in the shadow memory.


- [Taint analysis lecture](https://www.youtube.com/watch?v=FOckYE6KwbQ)
	- This explain various concepts of taint analysis
- [Valgrind paper on dynamic taint analysis](https://valgrind.org/docs/newsome2005.pdf)
- [All You Ever Wanted to Know About Dynamic Taint Analysis and Forward Symbolic Execution - but might have been afraid to ask]( https://users.ece.cmu.edu/~aavgerin/papers/Oakland10.pdf)