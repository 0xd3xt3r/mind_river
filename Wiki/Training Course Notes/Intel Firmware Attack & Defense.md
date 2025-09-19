---
up: "[[Learning MoC]]"
related:
  - "[[Rootkits and Bootkits]]"
  - "[[Intel Chipsec]]"
tags:
  - "#learning/course"
  - "#training/ost2"
created-date: 2024-12-27
title: Architecture 4001 - x86-64 Intel Firmware Attack & Defense
summary: Intel Firmware related attacks
status: on-hold
---

![[thanos-comic-clip-knowledge.png]]

## Reference Notes

1. Firmware runs in Real Mode. Also learn by SMM.
	1. SMM is an independent firmware which is isolated and run independently of other system and also it is very powerful mode of execution.
	2. How is SMM is protected?
2. How to inspect the configuration of the system?
3. Goal is to learn all these components and their relations?
	1. ![[intel-firmware-attack-knowledge-map.png]]
4. Why attack a firmware?
	1. Its a most privileged software on the system.
5. Attacker Motivations & Capabilities
	1. Firmware is stored on non-volatile flash memory which soldered on motherboard. While test of components live on Hard-driver/SSD.
	2. BIOS infection is very important because it very difficult to detect and reset and they can live there for long time. Attacker can infect all the components loaded after bios is loaded.
	3. BIOS can then infect bootloader(can also be in-memory only) which can then infect kernel.
	4. Because the capability to software which run earlier implies a capability to subvert the integrity of subsequent code that will run later.
6. ![[intel-privilege-mode and levels.png]]
7. ![[intel-platorm-attack-path.png]]
	1. CSME - Intel Management Engine / Intel Active Management Engine
		1. it is also hardware debugging capability. 
	2. Intel VT-d / IOMMU - Directed IO, defense DMA attack
		1. one can use this to protect memory region which store critical code like BIOS,SMM and other firmwares.
	3. Microsoft used VT-D to protect until VMM and it uses Intel Trusted-Execution-Tech(TXT)
	4. DMA-capable peripherals - can undo lot of defenses
		1. The maximum level at which RAM is in use, but an IOMMU (i.e. Intel VT-d) is not.
8. Architectural vs Implemented Privilege.
	1. Hardware maker can provide various features to defend against attack but it is upto the vendor like ASUS, HP and OS Makers to integrate those feature.
9. Reset State register value
	1. EDX register has some value set (0x106A1) which tells you about he features support by the CPU, that show bios know what is supported and what not.
	2. CS:IP - 0xF:0xF000  -> 
	3. IDTR Register
	4. GDTR Register
10. Chipsets
	1. (G)MCH = (Graphics and) Memory Controller Hub. (First there was the MCH, then when Intel added integrated graphics, it turned into a GMCH.)
	2. ICH = IO Controller Hub
	3. PCH = Platform Controller Hub
	4. SoC = System on a Chip
	5. DRAM Controller / HOST Port Controller
	6. Platform Controller Hub (PCH)
		1. we are only interested in SPI Flash
		2. PCI Express interface
		3. LPC/ Firmware Hub Interface
	7. Atom are proper SoC chip from intel for embedded systems.

### MMIO / Port IO

1. Memory controller is tasked to route the memory read/write to device/peripherals or DRAM.
2. memory controller decide if something is mapped to RAM or to a peripheral?
	1. With a lookup table incorporating both hardcoded and configurable ranges.
3. Port IO - port address space is completely separate from memory address space
	1. 2^16 1 byte port
	2. 2^15 - 2 byte port
	3. 0x4000 4 byte port
4. Fixed IO Ports - address/peripherals cannot be relocated and sometime cannot be disabled.
	1. Can find from PCH Docs, different on different devices.
5. Variable Port - can be moved around and mapped to particular device by bios or some other type of software configuration.
	1. can be configure with PCIe BARs
	2. hardware doesn't check for overlapping port map range, virtualization attack.([PanSec 2007 Duflot Papier](https://web.archive.org/web/20160408094501/http://www.ssi.gouv.fr/archive/fr/sciences/fichiers/lti/pacsec2007-duflot-papier.pdf))
6. Its a legacy method and not recommended by intel anymore
7. vendor map the fixed IO to custom peripherals and not document it which makes it hardware to track it down.
8. SMM related port - 0xB2 which is related to power-management
9. ACPI - Advanced Configuration and Power Interface
10. TCOBASE - Total Cost Of Ownership
11. How do we know what device or peripheral a assembly instruction is interacting with?
	1. Chipset docs will have port information and using the port encoding style you can extract the port number and then do some digging in the docs to figure out which device corresponds to this port number.
		1. port is used to setup MMIO operations which is much efficient form of access and also recommended form.
		2. PCIe does this style of access first it configuration with ports(Port 0xCF8/0xCFC) and does MMIO setup and later uses it full-fledges access.
		3. if you see two ports being accessed which seem to be nearby each other, then it’s probably Address/Data access style. Whereas if there doesn’t seem to be any relation between the ports, they’re probably just unrelated Poke/Peeks.
	2. MMIO is pretty straight forward.
12. [The 2 Port Access Styles](https://www.youtube.com/watch?v=2KVB81cAWQ0)
	1. [00:04]() - Address/Data Style : Pair of ports where you, one port specifies the address and other port specifies the data to read/write. (Very common method)
	2. [01:07]() - Poke/Peek : sending command to the port and reading the return value as a response to the command. This is done on a single port, it could be 1 byte or 4 byte port.
	3. [02:03]() - Dell Optiplex 7010 BIOS dump analysis
	4. [04:57]() - MacBook Bios dump analysis
	5. [06:07]() - Default Simics BIOS
	6. [06:20]() - Inferences
		1. port 128(0x80) is important, as it is used by POST(Power-on Self-Test)
		2. Port 0xCF8/0xCFC - PCIe related Ports. this has to be used before using MMIO
		3. PCIe is the view inside the guts of PCH thats why its important.
	7. [08:05]() - Identifying IO Port
		1. Googling
		2. Documentation
		3. Check if any port was reallocated in the assembly

## PCIe

1. DRAM Controller and LPC Device
	1. We will be concerted with these two device when attacking
	2. DRM Controller device id : Bus 0, Device 0 , Function 0
	3. LPC Device id : Bus 0, Device 0x1F , Function 0
	4. code name for chips and how to find right datasheet for your systems.[Link](https://en.wikipedia.org/wiki/Intel_Core)
	5. `python3 chipsec_util.py pci read 0 0 0 0x00 4`
2. **Redmi Book Pro** - Device Info
	1. DRAM Controller and LPC Device is on Same CPU Silicon 
	2. DRAM Controller info is on CPU Datasheet
	3. LPC Device info is on "On Package Data Sheet"
	4. ICiS 11th Generation 2020/2021
	5. Tiger Lake
	6. PCH 500 Series
	7. Rocket Lake
3. PCIe - Peripherals Component interconnect
	1. 256 buses which each can have 32 devices with each having 8 function.
	2. bus 0 is reserved for CPU.
4. PCI Address Space
	1. Configuration Space
	2. MMIO Space
	3. PIO Space
5. PCI Configuration Address Space
	1. CONFIG_ADDRESS - offset - 0xCF8 -> used to setup the address where R/W will be done
	2. CONFIG_DATA - 0xCFC -> read/write is done on this address.
	3. Intel vendor id 0x8086
6. PCI Base Address Registers (BARs)
	1. 5, 32 bit address 
	2. base address is written in base address register
	3. size write 1 
	4. bios ask the device for the size of MMIO/PIO 
7. "Root Complex" - PCIe Device network data routing 
	1. ![[pcie-network-routing.png]]
8. Accessing PCIe configuration via MMIO it has different encoding/decoding compared to port io
	1. Chipsec has problem with decoding via MMIO because of some physical memory reading issue
	2. 256 MB of PCIe configuration.
9. Option-ROM Attack (Brief History)
	1. Talk - Implementing and Detecting PIC Rootkit
	2. Paper - Thunderbolt: Expose and Mitigation (thundergate.io) https://github.com/sstjohn/thundergate
	3. Thunderstrike (Apple platform)
	4. Thunderstrike 2 - by Xeno Kovah, and Corey.
10. PCIe Option ROM - arbitrary piece of code which bios reads in and run. Not very PCIe device has it, it is only present on device which need to run the code on the boot.
	1. The code is usually located on the Flash chip on hardware from which it is read the code and execute it.
	2. BIOS check for header signature before running it. "0xAA55".
	3. At offset 3 get the offset where it should it run.
	4. Why there is a need?
		1. there are myriad of device and bios will not necessarily have all the driver at boot. So it will ask the device for the driver and run that code to do the initialization and make it runnable.
	5. The attacker should have the capability to infect the Option-ROM Storage to do the attack. Attacker should be method to run the malicious code.
	6. Defense? UEFI secure boot systems should have capability to verify the digital signature option-rom before running it. Attacker shouldn't be able to arbitrarily update the code.
		1. Many systems don't have signed option rom and bios.
		2. Jailing Option-ROM or de-privileging the Option-ROM running context.

### SPI Flash
1. [SPI Intro & Supported Operation Modes](https://www.youtube.com/watch?v=mIwoDjtnJSA)
	1. [00:10]() - Attacker's interest in infecting BIOS
		1. not easy to detect by defenders since it run on very high privilage it can hide itself effectively
		2. updating it also difficult because attack can prevent firmware updates
	2. [00:49]() - What is SPI Protocol?
		1. SPI Flash chip is using SPI protocol to communicate with the Flash storage chip
		2. SPI Flash chip stores BIOS code which run on reset, it also used to store info for peripheral processor like Intel Management Engine, Intel Embedded Controller.
		3. It uses 24 bit addressing(2^24 = 16 MB), intel support two SPI Flash chip you can store 32 MB code.
		4. Its slow 20 MHz and 66 MHz.
		5. Intel provide a nice MMIO interface to interact with the chip
	3. [03:08]() - SPI operation Mode
		1. ICH8 added Descriptor Mode modern systems and less the ICH7 non-descriptor
	4. [04:22]() - Memory Mapping - Non-Descriptor Mode
		1. high 16 Mb region maps full flash chip if the chips 4 mb then 4 such mapping of 4 mb will be done.
	5. [04:59]() - Descriptor Mode 1
		1. it has different data structure which give more details about how different region of chip will be used.
		2. Divides SPI Flash into regions - restricts the writing capabilities of different modes like SMM and Management engine
	6. [05:55]() - Descriptor Mode 2 (Soft Strap)
	7. [06:39]() - Memory Mapping - Descriptor Mode
		1. only BIOS region is mapped and other region of Flash is not mapped due to which IME and other firmwares are not visible.
		2. if the data structure is corrupted the SPI Flash will resort to Non-Descriptor Mode 
	8. [08:18]() - Direct vs Register access
		1. Direct Access - directly read the mapped region from memory, read-only there is no write
		2. Register Access - Access the region via MMIO Registers
2. SPI Flash Programming Interface
	1. [SPIBAR MMIO](https://www.youtube.com/watch?v=gGtjj2fzZ54)
		1. [00:51]() - PCIe access 
		2. [01:48]() - Access method (Hardware sequencing)
		3. [02:35]() - Software sequencing, sending command
		4. [03:04]() - Finding SPIBAR 1
			1. RCBA holds RCRBBA which then you can use to access SPIBAR
			2. ICH 9-10 and PCH 5-9 at offset 0x3800
			3. [03:29]() - Finding SPIBAR 2
			4. [03:58]() - Finding SPIBAR 3
		5. [05:02]() - Finding it on newer systems on 100 series PCH and new Systems
			1. PCH mapping B0:D31:F5 offset 0x10 bits [31:12] SPI_BAR0 MMIO then BIOS_SPI_BAR0.MEMBAR
			2. also called SPIBAR MMIO Registers
	2. [MMIO Interface and Registers](https://www.youtube.com/watch?v=HjZBOMSk8vU)
		1. [00:15]() - Hardware Sequencing
		2. [00:24]() - Registers
			1. HSFS/HSFSTS - status HSFCTL/HSFC - control
			2. 
		3. [01:28]() - PCH >= 100 Series Register
		4. [01:52]() - Flash Read Sequence
			1. FDATA0/FDATAN - data register
		5. [02:37]() - Flash Write Register
		6. [03:26]() - Read Address Details - 5 series chipset (Register: FADDR->FLA)
			1. [04:31]() - Read Address Details - for 100 Chipset (Register: BIOS_FADDR->FLA)
		7. [05:10]() - Read Size - Registers HSFCTL, FDBC 13:8 bits
			1. [05:50]() - Read Size 100 Chipsets FDBC
		8. [06:21]() - Flash cycle type to read(Register HSFC/HSFCTL -> FCYCLE) it has 2 bits
			1. to write some you have to first erase the block by setting it to all ones and the flip only the zero to one.
			2. [07:50]() - 100 series chipset
				1. more feature are added and it now has 4 bits 
				2. 4 kb block erase.
		9. [08:38]() - Execute the transactions HSFC/HSFCTL->FGO (Flash Cycle GO Register)
			1. [09:14]() - 100 series chipset
		10. [09:44]() - check for status of request
			1. Register HSFS/STS->FDONE (Flash Cycle Done) also check flash cycle error register FCERR.
		11. [10:07]() - HSFS->SCIP - check is the hardware is in use or operation is in-progress
			1. [10:40]() - 100 series chipset
		12. [11:04]() - Where the data is stored once the read operation is executed? (Register-> FDATA0-FDATA*N*)
			1. [11:56]() - 100 series chipset
		13. [12:29]() - write will be done very similarly by setting cycle type to write and putting data into FDATA*N* register before write.
	3. [Intel Flash Descriptor and the SPI Flash Layout](https://www.youtube.com/watch?v=IlcYMdHZeWA)
		1. [00:18]() - Flash Storage Layout
		2. [01:09]() - Layout in details
		3. [01:31]() - Layout about other details like Management engine, gigabit, platform data, Embedded controller, etc.
		4. [01:59]() - Regions can move around
		5. [02:23]() - Region details, Firmware Interface Table (FIT) / firmware layout.
		6. [03:04]() - UEFI Firmware file-system.
		7. [03:21]() - Coreboot Systems, non UEFI, OST2 Arch4032
		8. [03:51]() - CSME FS/Management Engine. Positive Tech did a Blackhat talk about it.
		9. [04:06]() - Intel Gigabit Ethernet (Research Opportunity)
		10. [04:55]() - Platform Data Region (Research opportunity)
	4. [Optional: Intel Flash Descriptor Nitty-Gritty Deep-Dive]()
		1. [In-Flash Registers vs. In-Memory Copies](https://www.youtube.com/watch?v=yHcmvLV3oxE)
			1. [00:05]() - Different Regions of Flash Descriptor Layout
			2. [00:32]() - Layout Details Docs location
			3. [01:28]() - Start signature/Magic Number.
				1. If signature value is corrector it will run in descriptor more and if it is invalid it will run in non-descriptor mode
			4. [02:12]() - layout evolution
			5. [03:15]() - Layout Reversing
			6. [03:55]() - FLREG/FREG, Flash Descriptor Copy in MMIO
		2. [Flash Descriptor Sections](https://www.youtube.com/watch?v=sHOWQ85Gt0k)
			1. [00:00]() - Signature Map
			2. [00:10]() - Descriptor Map
			3. [00:24]() - Components Map
			4. [00:44]() - Region Map
			5. [01:21]() - FLREG0-5 and FREG0-5
			6. [01:39]() - Master
				1. SPI Bus master access control
				2. Write access control
			7. [02:41]() - Soft Strap - reconfigure the hardware functionality without changing hardware
			8. [03:10]() - OEM Section
			9. [03:27]() - Upper map/management engine table
			10. [03:43]() - management engine VSCC table
			11. [04:23]() - PCH Datasheet
			12. [05:06]() - Lab: ICH10 Datasheet
	5. Flash Protection Threat Tree: Moves and Counter-Moves
		1. [Protected Range Registers](https://www.youtube.com/watch?v=WN_1eutaxjo)
			1. [00:56]() - Protection Mechanism #1 - PRR
				1. BIOS Range Protection - protects range, resets on override
				2. strongest mechanism
				3. [01:50]() - Protected Range Registers.
				4. protection from SMM write
			2. [02:27]() - Protected Range Register Details
				1. base give start address
				2. range register give end address, its not the size but the end address
			3. [04:16]() - Example of register value and decoding
			4. [05:02]() - 100 series datasheet
				1. larger flash linear address range 
				2. D31:F5 BIOS_FPR0
			5. https://github.com/LongSoft/UEFITool to check unprotected range
		2. [Flash Lockdown: FLOCKDN](https://www.youtube.com/watch?v=bLjy-8YJlj8)
			1. Attacking the Previous 
			2. [00:26]() - Attack plan
			3. [00:41]() - PRR Register
			4. [01:15]() - FLOCKDN Register (Counter Move)
				1. make PRR non-writeable
			5. [02:23]() - Affected Registers, bunch of config about about the register
			6. [04:14]() - Gigabit ethernet region FLOCKDN register
		3. [Other ways to attack PRRs](https://www.youtube.com/watch?v=8YozumoXVAw)
			1. Vendor may protect the Flash with PRR Register but if the range is no applied properly then that can become a vulnerability.
			2. ![[PRR-register-attack-tree.png]]
			3. [00:25]() - Thunderstrike
			4. [01:51]() - Research Opportunity
			5. [02:25]() - Another type of Attack
			6. [02:42]() - Attack: Memory corruption vulnerability before FLOCKDN is set. Attacker can then bypass the locking code.
			7. [03:46]() - Summary 1, of PRR Attack.
			8. [04:30]() - Summary 2, flash update attack surface
				1. [05:29]() - bios update is attack controlled data.
				2. [06:02]() - Research Opportunity.
		4. [BIOS Lock Enable BLE](https://www.youtube.com/watch?v=XgWLbJXvopk)
			1. [00:00]() - Protection Mechanism #2 - protects entire range
			2. [00:33]() - BIOS Lock Enable Register overview
				1. B0:D31:F0 in LPC Device BIOS_CNTL
				2. [01:48]() - 100 series chipset
			3. [02:40]() - Flash Write Sequence
			4. [03:08]() - BLE Behavior
		5. Attacking BLE: SMI Suppression 1

## System Management Mode (SMM)

1. [Introduction](https://www.youtube.com/watch?v=yt5sMzuCGG4)
	1. [00:21]() - Overall picture
	2. [00:28]() - Overview
		1. most privileged execution mode
		2. Intel trying to deprivilege it but vendor are yet to agree and catch-up with it.
		3. VM to SMM escalation will be very serious. Then attack can attack all other VM using SMM's privilage.
	3. [02:06]() - Overview 2
		1. [02:22]() - whole system stops
		2. SMI interrupt to enter SMM
		3. return with RSM(resume) instruction
	4. [03:02]() - state diagram
	5. [03:32]() - Intel Docs
	6. [04:22]() - Overview 3
		1. Features like power-management, security.
		3. OS independent critical functionality related to hardware/OEM
		4. setup by BIOS but not easy to find an reverse engineer.
		5. Infecting BIOS can infect SMM but not vice versa but not always(if protected by PRR).
		6. SMI doesn't have physical wire
	7. [07:53]() - SMI Interrupt
2. **Highlighted Causes of SMI 1**: BIOS Lock Enable
3. How SMI Handler Knows **What to Handle**? ( some bits in TCOBASE)
	1. this video tells you how to find those register.
	2. BIOSWR_STS -> Status Bits
	3. interesting how writing somewhere in MMIO raises an interrupt in SMI.
	4. TCOBASE -> PMBASE + 0x60
		1. TCO1_STS
	5. PMBASE - ACPI Base Address Register
	6. port 
4. **Highlighted Causes of SMI 2**: Advanced Power Management (APM) port 0xB2
	1. APM_CNT
	2. APM_STS
	3. PMBASE + 0x30 -> APMC_EN
5. **Highlighted Causes of SMI 3: FSMIE bit**
	1. Fired when Flash cycle is done
	2. 



## Logs

```dataview
TASK
WHERE (icontains(text, this.file.name) AND icontains(text, "#log/sprint")) 
	AND !(icontains(text, "#task") OR icontains(text, "#question"))
	AND !completed
GROUP BY file.name as Filename
SORT fows.file.ctime DESC
LIMIT 20
```