---
tags:
  - "#firmware-security"
  - "#qpsi/project"
  - "#type/sub-project"
status: todo
aliases:
  - gunyah fuzzing
  - hypervisor fuzzing
  - gunyah
up: "[[On Target Firmware Fuzzing]]"
related: 
created-date: 2024-12-15
summary: Fuzzing Guniya Hypervisor
---

> **Summary**:: 


## Meetings

```dataview
TABLE WITHOUT ID
	file.link as "Session",
	up, created-date, participants, summary, tags
FROM !"Templates"
WHERE icontains(related, this.file.link)
AND icontains(tags, "type/meeting")
SORT file.link DESC
```

## Tasks and Questions

```dataview
TASK
WHERE icontains(text, this.file.name)
AND (icontains(tags, "#task") OR icontains(text, "#questions"))
AND !completed
GROUP BY file.name as filename
SORT filename DESC
```

## Docs

1. Gunyah Hypervisor Internal Documents
2. https://qualcomm-hypx.readthedocs.io
3. https://lwn.net/Articles/836693/
4. https://x.com/zhuowei/status/1330675286149574659

## Source
[https://tigerseye.qualcomm.com](https://tigerseye.qualcomm.com:41723/source/xref/TZ.XF.5.0_TZ.FU.5.0/core.tz/2.0/kernel/hypervisor-rm/resource-manager/platform/qcom/src/platform_qcom.c#126 "https://tigerseye.qualcomm.com:41723/source/xref/tz.xf.5.0_tz.fu.5.0/core.tz/2.0/kernel/hypervisor-rm/resource-manager/platform/qcom/src/platform_qcom.c#126")

## Notes
- Existing CR's raise are tracked on this [jira ticket](https://jira-cstm.qualcomm.com/jira/browse/QPSIRAFT-1931)
- Open source campaign review [docs](https://confluence.qualcomm.com/confluence/display/GSC/Overall+reivew)
- [ ] [[Surendra]] security assurance documents like Zephyr to show the quality test done for this software.

## Gunyah Hypervisor Tech Workshop (LeMans)

- Type 1 hypervisor - it work independent of any kernel, it implements its own micro kernel, no paravirtualization support
- ARM v8.2 support only since its support hardware virtualization extension
- Gunyah is only responsible to lunch RootVM which has the Resource manager
	- ![[Pasted image 20240129144754.png]]
	- Resource Manager is a different program which is compiled into elf but later embedded in to hypervisor and signed
	- Resource manager has the knowledge how to lunch Android VM it has info about UEFI location in the memory
	- Can HLOS lunch a new VM
		- Hypervisor has the API to lunch New VM and Resource Manager can give permission to other VMs to use those API to lunch the VM
	- QSHEE manages which Memory Ranges are assigned to which VM this is used to share memory between VMs
	- PSCI - Power State Coordination interface
	- RM - Resource Manager, its the first VM which gets launch and has access to lots of thing. Very critical interface
	- VCPU - its virtual CPU assigned to run EL1 and EL0 code and its just like process in normal OS they are context switched in and out of CPU hardware.
	- Hypervisor Controls Stage 2 Page table and also offers some protections like never disable SELinux
	- Gunyah also emulates Interrupt Controller, it does this by enabling flag which traps any request that go to the MMIO register of the Devices
	- Hypervisor has to take care of following components
		- Timer
		- Watchdog
		- Interrupt controller
		- Power Management
		- Memory Management

## Entry point / Attack surfaces:
- .hvc file is an interface definition language one example would be 
	```C
	define msgqueue_send hypercall {
		call_num	0x1b;
		msgqueue	input type cap_id_t;
		size		input size;
		data		input type user_ptr_t;
		send_flags	input uregister;
		_res0		input uregister;
		error		output enumeration error;
		not_full	output bool;
	};
	```
- above message get translated to C code with following prototype
	```C
	hypercall_msgqueue_send_result_t 
	hypercall_msgqueue_send(
		cap_id_t msgqueue_cap,
		size_t size,
		user_ptr_t data,
		uint64_t send_flags)
	```
- the general format is hypercall_<func_name> here the func name is as per the .hvc file name

### Code ref
- [do_recv](https://tigerseye.qualcomm.com:41723/source/xref/TZ.XF.5.0_TZ.FU.5.0/core.tz/2.0/kernel/hypervisor-rm/resource-manager/src/rpc/rm-rpc.c#209)
- [platform_msg_callback](https://tigerseye.qualcomm.com:41723/source/s?refs=platform_msg_callback&project=TZ.XF.5.0_TZ.FU.5.0)
- [msg_callback](https://tigerseye.qualcomm.com:41723/source/s?refs=msg_callback&project=TZ.XF.5.0_TZ.FU.5.0)

## Ozy Scan

- Waipio ELF debug files is here: **\\snowcone\builds774\PROD\TZ.XF.5.18-00146-SPF.WAIPIO1\trustzone_images\core\bsp\gunyah-hypervisor\build\WAPIONAA**

## Ref

- [Overview in depth](https://qualcomm.sharepoint.com/teams/cphypervisor/Shared%20Documents/Forms/AllItems.aspx?FolderCTID=0x01200013B452C0165CB14A89C26415A70E214A&id=%2Fteams%2Fcphypervisor%2FShared%20Documents%2FAllAccessDocuments%2FGunyah%20Hypervisor%2FRecordings%2FGunyah%20Hypervisor%20Tech%20Workshop%20%28LeMans%29&viewid=6f5fbfa2%2Dcef4%2D413c%2D9044%2D8ac8e6c81615)
- [CrosVM Manager](https://qualcomm.sharepoint.com/teams/APSS/automotivesw/Shared%20Documents/Forms/AllItems.aspx?RootFolder=%2Fteams%2FAPSS%2Fautomotivesw%2FShared%20Documents%2FAutomotive%20Virtual%20Platform%2FTech%20Workshops%2F2023%20%2D%20Feb%20Planning%2FCrosVM&FolderCTID=0x0120000DDC86F3C97C254EA90091B407B2753B)
- [All Resources](https://qualcomm.sharepoint.com/teams/cphypervisor/Shared%20Documents/Forms/AllItems.aspx?FolderCTID=0x01200013B452C0165CB14A89C26415A70E214A&id=%2Fteams%2Fcphypervisor%2FShared%20Documents%2FAllAccessDocuments%2FGunyah%20Hypervisor&viewid=6f5fbfa2%2Dcef4%2D413c%2D9044%2D8ac8e6c81615)