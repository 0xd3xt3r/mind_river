
# ARM Processor

![[Pasted image 20231215120419.png]]
![[Pasted image 20231215120403.png]]

## Notes

- There are three family of ARM 
	1. [[ Cortex R]] - this is designed for Real Time requirement 
	2. [[Cortex M]] - Micro-controller requirement. this is the most popular ARM processor
	3. [[Cortex A]] - Very powerful process used like smartphone
- there is hardware based interrupt vector which has different handler for different interrupt handled by hardware
- there is single interrupt handler which then redirect to different handler so it can be called as software vector interrupt
- classify the register into processing, context information
- Minimum registers for context saving, [[Program Counter]] , [[Link Register]], [[CPSR]] status register and, Stack Register
- ARM Architecture Reference Manual, [[Technical Reference Manual]], Programmers Guide.
- VTOR -> Vector Table Offset Register is the register where the interrupt table offset are place.
	- At boot time, the VTOR is only the stack pointer and the Program counter pointing to Reset Interrupt Handler
	- After boot all other interrupt are configured.
- SRAM -> needs power to work. If the power is off the data is wiped.
- Clock is the heart beat of the SoC. It [[Synchronizes]] everything.
	- There  will be internal and external oscillator on every SoC.
	- Device start with internal oscillator, as its always reliable.
	- peripherals gets clocked with after clock register is said
	- there will be register telling which is the default system clock (internal/external)
	- speed of communication in the CPU.
- Datasheet of the [[SoC]]
	- Datasheet is the explanation of how SoC in Human understandable manner.
	- How Do you find the [[datasheet]] of the SoC?
		- Go to the SoC Vendor website and search
	- Boot configuration tells you from where you will fetch the firmware and boot the system.
		- there well be pin on the board which the primary bootloader will read and decide from where to fetch the firmware. For eg you can fetch firmware from SRAM, or Flash Memory or system memory
		- Based on this Configuration your [[VTOR]]


## Cortex-M4

![[Pasted image 20231215120657.png]]

- The Red components are optional and Green are always there.
- Cortex-M4 is based on ARM v7-M architecture
- WIC - can bring system to wake-up form deepest state
	- Wakeup Interrupt Controller
- NVIC - nested vector interrupt conto
- Debug Access Port
	- JTAG
	- SWD - serial wire debugger
- Thread Mode and Handler Mode
	- Handler Mode - is like kernel mode which has complete control of the system
		- Handler Mode is used for executing critical system tasks, such as interrupt service routines and operating system kernels.
	- Thread Mode - is like user mode it has some hardware feature disabled.
		- Thread Mode is used for executing non-critical code and application tasks
- Systick - 24 bit down counting timer is Watch Dog, very much needed in embedded system.
- All the above components are MMIO and the addresses remain the same.
- Bus Matrix - is sure the separate the request base on memory. For example if you want to access certain thing in certain range then the bus matrix will divert the request to certain components. This is basically handling MMIO request.

### Register
![[Pasted image 20231215123828.png]]

# Bare-Metal Reversing

# Linux Base Firmware Reversing

# External Resource

- [ARM Cortex-M Playlist](https://www.youtube.com/watch?v=rDvLaOe5_Ws&list=PLFt5JBAXXlQrAIGdKMq8mxdR_ciC4wlFk&index=2&ab_channel=EmbeddedSystems%2CinPyjama%21)


# Hardware Requirement
- Raspberry Pi 4b 
	- SoC Broadcom BCM2711 [datasheet](https://datasheets.raspberrypi.com/bcm2711/bcm2711-peripherals.pdf)
Follow list is the hardware you can use for ARM development

- https://www.digikey.in/en/articles/how-to-use-trustzone-to-secure-iot-devices
- Raspberry Pi  Bare Metal Programming
- https://www.youtube.com/watch?v=0Z4SIzgHGwk&list=PLDqMkB5cbBA5oDg8VXM110GKc-CmvUqEZ&index=2&ab_channel=HunterAdams