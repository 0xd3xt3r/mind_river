---
up: "[[Knowledge Base MoC]]"
tags:
  - "#type/app_sec/auditing"
  - "#type/os/windows"
aliases:
  - windows kernel auditing
status:
  - todo
created-date: 2024-12-15
summary: Kernel IOCTL Interface
---



## Security Boundary of the IOCTL Buffer

- IOCTL is very important as this is the entry point of the driver and the data is from user-mode is crossing a security boundary to kernel space
- There are two way to secure the buffer
	- Copy the input from the Userspace into the kernel buffer and copy it back after doing the processing. Lets call this a *copy-approach*.
	- Lock the user-space buffer this way when the kernel reads/write to it then you user cannot cannot change its content. Lets call this as *lock-approach*
- This [link](https://learn.microsoft.com/en-us/windows-hardware/drivers/wdf/accessing-data-buffers-in-wdf-drivers) give a very good overview of different kind of buffer access and how to use them safely.
	- Three type of buffers
		- **Buffered IO** - Kernel create a copy of the user-space buffer in kernel and copies input from the user and copies back the output to the output buffer.
			- copying the data back and forth can heart the performance
		- **Direct IO** - locks the buffer space into physical memory, and then provides the driver with direct access to the buffer space.
		- **Neither** - this is most unsafe and you need to use it with causing. The kernel code is provided with user-space buffer and you need to use WdfRequestProbeAndLockUserBufferFor(Read/Write) or WdfRequestRetrieveUnsafeUser(Input/Output)Buffer for using the buffer safely.
	- Another issue is when coping buffer pointing to userspace buffer you can use above approach to use it safely.
	- Another issue is when doing pointer arithmetic with these buffer, so essentially even if you obtain this buffer you need to audit any pointer arithmetic to see if this is not crossing pointer boundry.
	- WdfRequestRetrieveInputBuffer/WdfRequestRetrieveOutputBuffer can be used in insecure manner and her is how
		```C
		NTSTATUS WdfRequestRetrieveInputBuffer( 
			[in] WDFREQUEST Request, 
			size_t MinimumRequiredLength, // number of byte we want to read
			[out] PVOID *Buffer, // output will be Kernel Pointer
			[out, optional] size_t *Length // number of bytes read
		);
		
		NTSTATUS WdfRequestRetrieveOutputBuffer( 
			[in] WDFREQUEST Request,
			[in] size_t MinimumRequiredSize,
			[out] PVOID *Buffer,
			[out, optional] size_t *Length
		);
		```
		if the `MinimumRequiredLength` is controlled by user and not validated the OOB/OOR can occur 
- Coping the Buffer
	```C
	void ProbeForRead( 
		[in] const volatile VOID *Address, 
		[in] SIZE_T Length, 
		[in] ULONG Alignment
	);
	
	void ProbeForWrite( 
		[in, out] volatile VOID *Address, 
		[in] SIZE_T Length, 
		[in] ULONG Alignment 
	);
	```
- Managing IO Request Queue [link](https://learn.microsoft.com/en-us/windows-hardware/drivers/wdf/managing-i-o-queues)
- Dealing with Variable Length Data struct
- Requeue of the request or forwarding of the request to another queue
- IO Request can be processed by registering callback or by manually call with If a driver specifies the manual or sequential dispatching method, it can obtain requests by calling *WdfIoQueueRetrieveNextRequest or WdfIoQueueRetrieveRequestByFileObject.*
- _EvtIoInCallerContext_ callback function so that the driver can examine each I/O request, and possibly perform preliminary processing on the request, before the framework places it in an I/O queue. To register an _EvtIoInCallerContext_ callback function for a device,
	- WdfDeviceInitSetIoInCallerContextCallback will register the callback
	- forward the request or reject the request
	- Middleware for request
- How is return value given for IO Request response

## Attack Surface

1. User space to Kernel
2. Kernel to Kernel
3. Shared Buffer
4. Device IOCTL - parsing custom command

## Driver Workflow

- `SetupDiEnumDeviceInterfaces`, to enumerate the interfaces present for a particular GUID and to obtain the names of the symbolic links it can use to open the device objects.
- the application executes `SetupDiGetDeviceInterfaceDetail` to obtain additional information about the device, such as its auto-generated name. the application can execute the Windows function `CreateFile` or `CreateFile2` to open the device and obtain a handle.
 - PnP drivers expose one or more interfaces by calling the `IoRegisterDeviceInterface` function,
 - it must create a symbolic link in the \GLOBAL?? directory to the device object’s name in the \Device directory (The `IoCreateSymbolicLink` function accomplishes this.)
 - `IoRegisterDeviceInterface` generates the symbolic link associated with a device instance.

## Function Audit List

- WdfDeviceEnqueueRequest 
- WdfDeviceCreateSymbolicLink
- WdfDeviceCreateDeviceInterface
-[WdfIoQueueCreate](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdfio/nf-wdfio-wdfioqueuecreate)
- WdfRequestRetrieveInputBuffer
- WdfRequestRetrieveOutputBuffer
- WdfRequestComplete
- WdfMemoryCopyFromBuffer
- WdfMemoryCopyToBuffer
- WdfMemoryGetBuffer
- WdfRequestCompleteWithInformation
- WdfRequestRetrieveInputWdmMdl
- WdfRequestRetrieveOutputWdmMdl
- [EvtIoDeviceControl](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdfio/nc-wdfio-evt_wdf_io_queue_io_device_control)
	- EvtCleanupCallback 
		- A driver can support a cleanup callback for any object. The framework calls the cleanup callback before it deletes the object, the object’s parent, or any of the object’s more distant ancestors. The cleanup callback should release any outstanding references that the object has taken, free any resources that the driver allocated on behalf of the object, and perform any other related tasks that are required before the object is deleted.
	- EvtDestroyCallback 
		- callback, which the driver registers in the object attributes structure at object creation. The framework calls the destroy function after the object’s reference count reaches zero and after the cleanup callbacks of the object, its parent, and any further ancestors—grandparents and so forth—have returned. The framework actually deletes the object after the destroy callback returns.

## Reference

https://learn.microsoft.com/en-us/windows-hardware/drivers/wdf/accessing-data-buffers-in-wdf-drivers