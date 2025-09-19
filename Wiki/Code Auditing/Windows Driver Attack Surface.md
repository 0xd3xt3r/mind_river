---
up: "[[Knowledge Base MoC]]"
tags:
  - "#type/os/windows"
status:
  - todo
created-date: 2024-12-15
related: 
summary: Copying data in and out of driver
---


## Device IOCTL

We can look at the DeviceIoControl() API as being similar to an ioctl() call on UNIX-like systems, such as we discussed in the preceding chapter. This function sends a control code directly to a specific device driver to perform a corresponding operation. Usually, along with the control code, a process will also send custom data that the driver handler must interpret correctly. This is the DeviceIoControl() prototype:
```C
BOOL WINAPI DeviceIoControl(
	HANDLE hDevice,
	DWORD dwIoControlCode,
	LPVOID lpInBuffer,
	DWORD nInBufferSize,
	LPVOID lpOutputBuffer,
	DWORD nOutBufferSize,
	LPDWORD lpBytesReturned,
	LPOVERLAPPED lpOverlapped
);
```
The function takes a few parameters, the most important ones being the device driver HANDLE, the I/O control code, and the addresses of the input and output buffers. When the function returns, a synchronous operation takes place in which the DWORD addressed by the lpBytesReturned pointer will hold the size of the data stored in the output buffer. Finally, lpOverlapped holds the address of an OVERLAPPED structure that is to be used during asynchronous requests; according to the dwIoControlcode parameter, the input and output buffers addressed by lpInBuffer and lpOutBuffer could be NULL.

When the user mode issues a call through the DeviceIoControl() API, the I/O Manager (which is within the Kernel Executive module) creates an IRP and delivers it to the device driver. An IRP, a structure that encapsulates the I/O request and maintains a request status, is then passed through the driver's stack until a driver can fully or partially handle it; it can be processed synchronously or asynchronously, and can be sent to a lower driver or even cancelled during its processing. The I/O Manager can automatically create an IRP in response to a user-mode process operation (such as a call to the DeviceIoControl() routine), or a high-level driver can create it within the kernel to be able to communicate with a lower-level driver.

By assuming that the I/O Manager has generated the I/O Request Packet during a DeviceIoControl() from a user-mode process, we can simplify the description—provided, of course, that the addresses of memory pages passed within the IRP will always belong to the user-mode address space.

```C
#include <windows.h>
#include <winioctl.h>
#include <stdio.h>

#define wszDrive L"\\\\.\\PhysicalDrive0"

BOOL GetDriveGeometry(LPWSTR wszPath, DISK_GEOMETRY *pdg)
{
  HANDLE hDevice = INVALID_HANDLE_VALUE;  // handle to the drive to be examined 
  BOOL bResult   = FALSE;                 // results flag
  DWORD junk     = 0;                     // discard results

  hDevice = CreateFileW(wszPath,          // drive to open
                        0,                // no access to the drive
                        FILE_SHARE_READ | // share mode
                        FILE_SHARE_WRITE, 
                        NULL,             // default security attributes
                        OPEN_EXISTING,    // disposition
                        0,                // file attributes
                        NULL);            // do not copy file attributes

  if (hDevice == INVALID_HANDLE_VALUE)    // cannot open the drive
  {
    return (FALSE);
  }

  bResult = DeviceIoControl(hDevice,                       // device to be queried
                            IOCTL_DISK_GET_DRIVE_GEOMETRY, // operation to perform
                            NULL, 0,                       // no input buffer
                            pdg, sizeof(*pdg),            // output buffer
                            &junk,                         // # bytes returned
                            (LPOVERLAPPED) NULL);          // synchronous I/O

  CloseHandle(hDevice);

  return (bResult);
}

int wmain(int argc, wchar_t *argv[])
{
  DISK_GEOMETRY pdg = { 0 }; // disk drive geometry structure
  BOOL bResult = FALSE;      // generic results flag
  ULONGLONG DiskSize = 0;    // size of the drive, in bytes

  bResult = GetDriveGeometry (wszDrive, &pdg);

  if (bResult) 
  {
	wprintf (L"GetDriveGeometry success.");
  } 
  else 
  {
    wprintf (L"GetDriveGeometry failed. Error %ld.\n", GetLastError ());
  }

  return ((int)bResult);
}
```

[DeviceIoControl](https://learn.microsoft.com/en-us/windows/win32/api/ioapiset/nf-ioapiset-deviceiocontrol)

## Copying Data in and out of Kernel

1. The preceding code simply copies a user-land buffer into a kernel-space buffer. All of the code is enclosed within a try/except block, which is used to manage software exceptions. The try/except blocks are mandatory when dealing with user-land pointers. (We will discuss the implementation of exception blocks and the exception dispatching mechanism in the section “Practical Windows Exploitation,” later in this chapter). Moving on to the code within the try/except block, pointers that address hypothetical user-mode address space (such as userBuffer in the preceding example) must always be checked—otherwise, it would be possible for an evil user-mode process to pass an invalid pointer capable of addressing kernel pages.

	```CPP
	__try {
		ProbeForRead(userBuffer, len, TYPE_ALIGNMENT(char));
		RtlCopyMemory(kernelBuffer, userBuffer, len);
	} __except(EXCEPTION_EXECUTE_HANDLER) {
		ret = GetExceptionCode();
	}
	```

1. Windows provides two kernel function primitives that we can use to validate the user-mode-supplied buffers: `ProbeForRead()` and `ProbeForWrite()`.
2. The prototype of `ProbeForRead()` is as follows: `VOID ProbeForRead(CONST VOID *Address, SIZE_T Length, ULONG Alignment)`
3. The Address specifies the beginning of the user-mode buffer, the Length parameter specifies the length in bytes, and the Alignment is the required address alignment. This function verifies that the buffer is actually confined within the user address space.
4. As we can see, the ProbeForRead() function is placed inside a *try/except exception block*. The function, in fact, will return successfully only if the buffer is actually confined within the user address space; if it falls outside this area, an exception is triggered and the already mentioned except block must intercept it. There are two important matters that we need to address about this function. The first matter is related to the access check implementation. This function does not access the user-mode buffer at all it merely verifies that the buffer is within the correct range and that the supplied pointer is correctly aligned. What happens if the buffer is valid but the user-land range is not fully mapped? Any such buffers would successfully be able to pass the test, since an exception wouldn't be triggered until later, when the driver reads the buffer. Passing a partially invalid buffer to the kernel, however, is not the only way to trigger the exception; an evil thread is always capable of deleting, substituting, or changing the protection of the user address space even after the probe call.
5. The other interesting matter regards the Length parameter. If a zero-length parameter is passed to the function, it will return immediately without ever checking the source buffer. Although this behavior may at first seem logical, it can be abused—and sometimes exploited—if an integer overflow or an integer wraparound occurs during the length calculation. Take a look at the following piece of code:
	```cpp
	__try {
	
		ProbeForWrite(
			user_controlled_ptr,
			sizeof(DWORD) + controlled_len, [1]
			TYPE_ALIGEMENT(char)
		);
		
		*((DWORD *)user_controlled_ptr) = 0xdeadbeaf; [2]
		user_controlled_ptr += sizeof(DWORD);
		
		for(i=0; i<controlled_element; i++) {
			VOID *dest = user_controlled_ptr + sizeof(Object)*i;
	[...]
	```
1. In this example, the kernel needs to validate the user-supplied parameter user_controlled_ptr. Let us assume we are working in a 32-bit kernel environment. Provided we can also somehow arbitrarily control the controlled_len variable, the check executed at [1] can be bypassed using a value of 0xFFFFFFFC. Since sizeof(DWORD) is equal to 4, the final length is 0 (taking into account the unsigned integer wraparound). The ProbeForWrite() function will then immediately return without performing any further checks on the user_controlled_ptr address. What would happen if user_controlled_ptr were to hold a kernel-space address? The answer is straightforward: a partially controlled memory corruption (at [2]) would occur. This is a particularly common error that third-party drivers make often when dealing with user-mode buffer size. We will see in the section “Practical Windows Exploitation” how built-in exception handling is implemented and how we can abuse its inner logic to bypass stack overflow protections.

> NOTE The **ProbeForWrite** function is not recommended for use in current software, and is included for historical compatibility only. Use [**ProbeForRead**](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-probeforread) instead when validating user buffers. See Remarks for more info.

## Registry Keys

Related Function
1. [ZwEnumerateKey](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdm/nf-wdm-zwenumeratekey)
2. ZwOpenKey
3. IoOpenDeviceRegistryKey
4. ZwQueryKey
5. [WdfRequestRetrieveInputBuffer](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdfrequest/nf-wdfrequest-wdfrequestretrieveinputbuffer)
6. [WdfRequestRetrieveOutputBuffer](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdfrequest/nf-wdfrequest-wdfrequestretrieveoutputbuffer)
7. [WdfRequestRetrieveInputMemory](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdfrequest/nf-wdfrequest-wdfrequestretrieveinputmemory)
8. [WdfRequestRetrieveUnsafeUserInputBuffer](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdfrequest/nf-wdfrequest-wdfrequestretrieveunsafeuserinputbuffer) safe usage example [link](https://learn.microsoft.com/en-us/windows-hardware/drivers/ddi/wdfrequest/nf-wdfrequest-wdfrequestprobeandlockuserbufferforread)
9. WdfRequestRetrieveUnsafeUserOutputBuffer
10. Buffer management between user and kernel [link](https://learn.microsoft.com/en-us/windows-hardware/drivers/wdf/accessing-data-buffers-in-wdf-drivers)