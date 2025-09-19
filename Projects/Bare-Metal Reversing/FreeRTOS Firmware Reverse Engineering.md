# [[FreeRTOS]] Firmware Reverse Engineering

> FreeRTOS is ideally suited to deeply embedded real-time applications that use
microcontrollers or small microprocessors. This type of application normally includes a mix of both hard and soft real-time requirements.

Tags : #firmware-reversing , #bare-metal-firmware , #reverse-engineering 

## Objective

1. Look for common function which are used by core framework and create Ghidra signatures for functions for all the architecture it supports.
2. Look into heap implementation and see what all heap attack that can be done. This is configurable feature so not all binaries will have dynamic allocation enabled.
3. Figure-out all the important data structure from code and try to relate it with the firmware binary. Some common Structures are
	1.  Job Schedule - this could have link to all task which are getting executed
	2.  Heap is already covered.
	3.  Trace recording - this could also be useful for recovering data.
	4.  Task Runtime statics structure - this could again be useful for debugging the running device.
	5.  Queue
	6.  Timer
	7.  Event Groups

## Signatures
1. Some of the common function will are present are *IDLE Task* search for strings




## Heap Management
Dynamic allocation prior to v9.0.0 is mandatory and post that version this feature can be enable/disabled from the configuration file. 


## Reference
1. https://blog.zimperium.com/freertos-tcpip-stack-vulnerabilities-details/
2. https://blog.3or.de/starting-embedded-reverse-engineering-freertos-libopencm3-on-stm32f103c8t6.html