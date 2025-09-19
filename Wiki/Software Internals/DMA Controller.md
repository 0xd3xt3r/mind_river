# Direct Memory Access

Direct memory access, or DMA , is the advanced topic that completes our overview of memory issues. DMA is the hardware mechanism that allows peripheral components to transfer their I/O data directly to and from main memory without the need to involve the system processor. Use of this mechanism can greatly increase throughput to and from a device, because a great deal of computational overhead is eliminated.

![[Pasted image 20231109154307.png]]

## Overview of a DMA Data Transfer

Before introducing the programming details, let’s review how a DMA transfer takes place, considering only input transfers to simplify the discussion.

Data transfer can be triggered in two ways: either the software asks for data (via a function such as _read_) or the hardware asynchronously pushes data to the system.

In the first case, the steps involved can be summarized as follows:

1. When a process calls _read_, the driver method allocates a DMA buffer and instructs the hardware to transfer its data into that buffer. The process is put to sleep.
2. The hardware writes data to the DMA buffer and raises an interrupt when it’s done.
3. The interrupt handler gets the input data, acknowledges the interrupt, and awakens the process, which is now able to read data.

The second case comes about when DMA is used asynchronously. This happens, for example, with data acquisition devices that go on pushing data even if nobody is reading them. In this case, the driver should maintain a buffer so that a subsequent _read_ call will return all the accumulated data to user space. The steps involved in this kind of transfer are slightly different:

1. The hardware raises an interrupt to announce that new data has arrived.
2. The interrupt handler allocates a buffer and tells the hardware where to transfer its data.
3. The peripheral device writes the data to the buffer and raises another interrupt when it’s done.
4. The handler dispatches the new data, wakes any relevant process, and takes care of housekeeping.

A variant of the asynchronous approach is often seen with network cards. These cards often expect to see a circular buffer (often called a _DMA ring buffer_) established in memory shared with the processor; each incoming packet is placed in the next available buffer in the ring, and an interrupt is signaled. The driver then passes the network packets to the rest of the kernel and places a new DMA buffer in the ring.

The processing steps in all of these cases emphasize that efficient DMA handling relies on interrupt reporting. While it is possible to implement DMA with a polling driver, it wouldn’t make sense, because a polling driver would waste the performance benefits that DMA offers over the easier processor-driven I/O.

Another relevant item introduced here is the DMA buffer. DMA requires device drivers to allocate one or more special buffers suited to DMA. Note that many drivers allocate their buffers at initialization time and use them until shutdown - the word _allocate_ in the previous lists, therefore, means “get hold of a previously allocated buffer.”

> Above piece of text is form - Linux Device Driver Development



- Direct Memory Access (DMA) is a technique to transfer the data from I/O to memory and from memory to I/O without the intervention of the CPU. For this purpose, a special chip, named DMA controller, is used to control all activities and synchronization of data. As result, compare to other data transfer techniques, DMA is much faster. This was a piece of hardware that set up by the CPU and initiated transfers to move data from a device's I/O address to memory, or vice versa. Because the I/O address is the same, the DMA controller is doing the exact same operations as a CPU would, but a little more efficiently and allowing some freedom to keep running in the background (though possibly not for long as it can't talk to memory).

- DMA allows hardware to directly read and write memory _without_ involving the CPU. Usually, this would be used for high-bandwidth operations such as disk I/O or camera video input.

Here is a paper has a thorough comparison between MMIO and DMA. [Design Guidelines for High Performance RDMA Systems](https://www.usenix.org/system/files/conference/atc16/atc16_paper-kalia.pdf)


## Memory Mapped IO

- Memory-mapped I/O allows the CPU to control hardware by reading and writing specific memory addresses. Usually, this would be used for low-bandwidth operations such as changing control bits.
- Memory-mapped IO means that the device registers are mapped into the machine's memory space - when those memory regions are read or written by the CPU, it's reading from or writing to the device, rather than real memory. To transfer data from the device to an actual memory buffer, the CPU has to read the data from the memory-mapped device registers and write it to the buffer (and the converse for transferring data to the device).
https://rashandeepsingh.medium.com/differences-between-memory-mapped-i-o-and-dma-mode-of-data-transfer-471ddb5424f2