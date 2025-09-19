---
up: "[[Qualcomm MoC]]"
tags:
  - "#type/qcom"
summary: Inter processor communication internals
---

# IPC in Qualcomm Chipset


**Video**

[Part 1](https://web.microsoftstream.com/video/a5c21236-6744-4593-a38e-1844dbd81732)

**Progress** : 50 min

**Notes**

- In the session when we say IPC it means Inter-process communication between sub-system/processor like [[HLOS]] -> [[DSP]]/[[Modem]] of the [[SoC]].
- [[IPC]] is the common attack surface as the information flows in and out. Data is crossing [[security boundaries]].
- IPC Consideration?
	- max size of message
	- system throughput
	- message reliability
	- security guarantees at the low level
	- how all has access to IPC? 

	![[Pasted image 20210811190540.png]]

	![[Pasted image 20210811191403.png]]

**Physical Layer IPC**

- **Protocols** - SMEM, DDR, PCIe, UART, HSIC, SPI, I2C


**Data Link Layer**

- Protocol - GLINK, SMD(Deprecated)

