---
created-date: 2025-01-02T17:02
up: "[[Meeting MoC]]"
presented-by: 
participants: "[[Surendra]]"
related: 
tags:
  - "#type/meeting"
  - "#qpsi/meeting"
---

> **summary**:: Understanding SoC Threat Model which ended up discussing about PCIe thread

- **Context**: I was reading about some research on PCIe and SMM in context of platform security, I had few question in relations to how can I draw parallel on ARM based platform, let me know when can we have the call?

## Meeting Minutes

- Store Password and 
	- Secure Memory 
	- RPMB
- Write only by Root of Trust
	- Verify the request
	- capsule update
- System is cluster of devices, 
	- memory is also one of device/type of device
	- can have multiple memory
	- GPU, Audio, MMC, USB, UART, WiFi
	- Memory Controller LP DDR
	- Flash Controller - Interface with Flash memory
	- MMC Slot - mmc controller interface with MMC
	- Address, Data & Control Bus
		- Address - Master of transaction for eg CPU
			- bit decide max address
		- Control Bus - command of the operation read/write
			- slave will decode the commands
		- Data - actual parameter of the transaction
	- Some entity will decode address and know where to take the request.
		- address can be configured at boot
	- system can have multiple master. 
	- Master - Entity which initiate the transaction.
- PCIe - Root Complex is a PCIe Controller/Master
	- Address Translation Service ATS
	- Access Control Service ACS
- IOSF - > AMBA parallel in intel
- ARM System Components
	- BIOS
	- ATS
	- PCIe - no hot-swap, reduces complexity.
	- https://developer.arm.com/documentation/109242/0100/What-an-SMMU-does
	- https://developer.arm.com/documentation/109242/0100/Operation-of-an-SMMU/Address-Translation-Services

## Action Item

- [ ] BIOS Device memory layout is input for SMMU
	- Understand in details how complex SoC Memory Mapping works #question ?
- [Google PCIe Fuzzing](https://cloud.google.com/blog/products/gcp/fuzzing-pci-express-security-in-plaintext)
- [ ] #log/sprint  Book Recommendation - [[PCI Express System Architecture]]
- [x] #task ask [[Murali]] that I want to work on PCIe related project.
