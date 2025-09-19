---
up: "[[Android Kernel Driver Fuzzing]]"
related:
  - "[[Linux kernel coverage reporting using GCOV]]"
  - "[[Project - Variant Analysis]]"
  - "[[Android Build Commands]]"
tags:
  - "#type/sub-project"
  - "#qpsi/project"
  - "#type/meeting"
  - "#qpsi/meeting"
status: in-progress
created-date: 2025-02-12T11:13
summary: Fuzzing Qualcomm Android Kernel Driver with Syzkaller
participants:
---

# Project Management

## Meetings

```dataview
TABLE WITHOUT ID
	file.link as "Session", summary, created-date, participants
FROM !"Templates"
WHERE icontains(related, this.file.link)
AND icontains(tags, "type/meeting")
SORT created-date DESC
```

## Tasks and Questions

### ToDo

```dataview
TASK
WHERE icontains(text, this.file.name)
AND (icontains(tags, "#task") OR icontains(text, "#questions"))
AND !completed
GROUP BY file.name as filename
SORT filename DESC
```

### Completed Task

```dataview
TASK
WHERE icontains(text, this.file.name)
AND (icontains(tags, "#task") OR icontains(text, "#questions"))
AND completed
GROUP BY file.name as filename
SORT filename DESC
```

## Sprints

```dataview
TASK
WHERE icontains(text, this.file.name)
	AND icontains(text, "#log/sprint")
GROUP BY file.name as filename
SORT filename DESC
```

# Project Info

## Frequent Docs

- [Pakala Syzkaller Dashboard](http://apt-lnxsec05:12345/)
- [My Syzkaller Dashboard](http://hyd-lablnx642:12345/)
- [Android Kernel Fuzzing Conf Page](https://confluence.qualcomm.com/confluence/display/QPSIFT/Android+Kernel+Driver+Fuzzing)
- [Camera Fuzzing Confluence Page](https://confluence.qualcomm.com/confluence/display/QPSIFT/Android+Camera+Driver+Fuzzing)
- [Operational Excellence](https://confluence.qualcomm.com/confluence/display/QpsiSecval/Team+Operational+Excellence)
- [Camera Git Repo](https://review-android.quicinc.com/q/project:platform/vendor/opensource/camera-kernel)
- [Graphics kernel repo](https://review-android.quicinc.com/q/project:platform/vendor/qcom/opensource/graphics-kernel)

## Task

- [ ] #task Klockwork rules for camera driver attack surface #qpsi ➕ 2024-12-12
- [ ] cam_private_ioctl - 70 Request manager
	- this function is already has the necessary grammar
- [ ] cam_sync_dev_ioctl - 63
	- [ ] this function is not directly reachable
- [ ] cam_node_handle_ioctl - 18
	- function resolution doesn't make sense
- [ ] cam_subdev_ioctl - not sure why there are so many unresolved function
- [ ] cam_flash_subdev_ioctl - 55
- [x] cam_cpas_subdev_ioctl
- [x] cam_actuator_subdev_ioctl
- [x] cam_csiphy_subdev_ioctl
- [x] cam_eeprom_subdev_ioctl
- [x] cam_ois_subdev_ioctl
- [x] cam_tpg_subdev_ioctl

## Notes

- [Pakala Syzkaller Dashboard](http://apt-lnxsec05:12345/)
- [My Syzkaller Dashboard](http://hyd-lablnx642:12345/)
- [Android Kernel Fuzzing Conf Page](https://confluence.qualcomm.com/confluence/display/QPSIFT/Android+Kernel+Driver+Fuzzing)
- [Internal Docs](https://confluence.qualcomm.com/confluence/display/QPSIFT/Android+Camera+Driver+Fuzzing)

## Conclusion 

There are many moving part in this project like Fuzzing Tool(Syzkaller), Kernel Coverage components (Linux KCOV) and the grammar/harness which we are writing. The harness part we have exhausted the capabilities investing further into this will not yield more results. Instead, we should invest in other parts like Syzkaller and accuracy of coverage collection. This components can now give more ROI.

## Commands

[[Android Build Commands]]

## Build Instruction

Email Group : linux.pakala.image.team@quicinc.com, pakala.target.team@qti.qualcomm.com

### Device Info

| Host             | Comment                        |
| ---------------- | ------------------------------ |
| hyd-lablnxqpsi04 | Primary                        |
| hyd-lablnxqpsi05 | secondary                      |
| qpsi-lablnx01    |                                |
| hyd-lablnx159    | Machine Dev machine for Neeraj | 

## Patches

### GCOV Patches

1. https://android-review.googlesource.com/c/kernel/build/+/2780228
	1. Android patch to add GCOV option to the build system
2. https://review-android.quicinc.com/c/platform/vendor/opensource/eva-kernel/+/5571553
	1. Enable GCOV option
	2. use this as reference to enable gcov in particular driver
 
### KASAN Patches

Below patches enable KASAN on Android kernel build

1. https://review-android.quicinc.com/c/kernel/qcom/+/4965077
	1. bazel enable kasan
2. https://review-android.quicinc.com/c/kernel/qcom/+/4490048
	1. Copy common kernel modules for syzkaller
3. https://review-android.quicinc.com/c/kernel/build/+/4965898
	1. GCOV patch
4. https://review-android.quicinc.com/c/kernel/common/+/5199670
	2. kcov - unique pc patch
5. https://review-android.quicinc.com/c/kernel/common/+/4977553
	1. disable KCOV on certain core Linux kernel components
	2. not to use this anymore

### Recipe

- [Latest Build](https://qwiki.qualcomm.com/quic/Android/pakalabuilds/LA.VENDOR.15.4.0-64602-pakala.1.CONSOLIDATED-3)
	- Meta :  /prj/qct/asw/crmbuilds/crmhyd/nsid-hyd-07/LA.QSSI.15.0-53902-qssi.1-2/LINUX/android
- [New Recipe](https://qwiki.qualcomm.com/quic/Android/pakalabuilds/LA.VENDOR.15.4.0-54002-pakala.1.CONSOLIDATED-2)
	- Meta : /prj/qct/asw/crmbuilds/crmhyd/nsid-hyd-07/Pakala.LA.1.0-01106-PERF.INT-1
- [AU 485](https://qwiki.qualcomm.com/quic/Android/pakalabuilds/LA.VENDOR.15.4.0_Builds/LA.VENDOR.15.4.0-48502-pakala.1.CONSOLIDATED-14)
	- this is working for v1 device post this build not build will be working for v1 only for v2.
	- Host : hyd-lablnxqpsi04
	- Meta : /prj/qct/asw/crmbuilds/crmhyd/nsid-hyd-05/Pakala.LA.1.0-01056-STD.INT-1
- [LA.VENDOR.15.4.0-38202](https://qwiki.qualcomm.com/quic/Android/pakalabuilds/LA.VENDOR.15.4.0_Builds/LA.VENDOR.15.4.0-38202-pakala.1.CONSOLIDATED-4)
	- latest
	- Host : hyd-lablnx642
	- currently flashed on device
	- Meta & KASAN both working fine
	- Meta : /prj/qct/asw/crmbuilds/crmhyd/nsid-hyd-05/Pakala.LA.1.0-00803-STD.INT-1
- Camera SDK Setup Guild [link](https://confluence.qualcomm.com/confluence/display/CamNext/Pakala%20Mainline%20Build%20Recipe)
- Camera NDK Build instruction [link](https://confluence.qualcomm.com/confluence/display/CamNext/CamX+Pakala+Bring-up#CamXPakalaBringup-NDKBuildsetup)
- [All Pakala builds](https://qwiki.qualcomm.com/quic/Android/pakalabuilds/LA.VENDOR.15.4.0_Builds)
- [All Kaanapali Builds](https://qwiki.qualcomm.com/quic/Android/kaanapalibuilds/LA.VENDOR.16.2.0_Builds)

### Notes

- KASAN & KCOV Build patch
	- Must changes for on-target fuzzing, refers to MustChanges4OnTargetFuzzing. (since qcom-20231124)
		- Pakala kasan: `gpull cp 4965077 4490048 4977553 4965898`
		- Pakala kcsan: `gpull cp 4963850 4490048 4977553 4965898 5246106 5246107`
	- Optional changes for on-target fuzzing
		- Pakala: `gpull cp 5199670`
- Release build path Hyderabad : /prj/qct/asw/crmbuilds/crmhyd
- [KCOV patch](https://review-android.quicinc.com/c/kernel/build/+/4965898/8/kleaf/impl/defconfig/kasan_generic_defconfig)
- [Generate Bitcode for Android Kernel](https://confluence.qualcomm.com/confluence/display/LnxSec/How+to+generate+bc+for+Linux+Android+Kernel)
- [Syzkaller Setup instruction](https://github.qualcomm.com/LinuxSecurity/syzkaller#setup--build-syzkaller)
- Verify kernel config on device 
	- `grep KCOV out/msm-kernel-sun-consolidate/dist/.config` this command can be used to verify from build code
	- `zcat /proc/config.gz | grep KCOV` this command can be used to verify from MTP Device
- flash *Meta CRM* build followed by *Kasan Consolidated CRM*
- You can use go/qsbt to sync and build. it's web tool to help sync, build based on ecrm, you need to put these 4 syzkaller gerrits in the hotfix list

### Environment Dependencies

- Alpaca & FTDI requirement has Ubuntu 22.04 as requirement
```bash
sudo apt install -y libxcb-cursor0
sudo apt install -y libpcre2-16-0
sudo apt install -y libusb-0.1-4
~/Alpaca/postinstall.sh
#postinstall.sh` from `/opt/qcom/Alpaca/util`.
sudo adduser $USER dialout
lsusb
```

## Camera Driver

### Syzkaller Config

```json
{
	"syz_init_device_buf",
	
	"openat$camx_req_mgr",
	"ioctl$VIDIOC_CAM_MGR_ALLOC_CMD",
	"ioctl$VIDIOC_CAM_REQ_MGR_SESSION_CREATE_CMD",
	
	"openat$camx_dev_tpg*",
	"ioctl$VIDIOC_CAM_TPG_*",
	"syz_memcpy_off$CAMX_DMA_CMD_COPY",
	"syz_memcpy_off$CAMX_TPG_CMD_COPY",
	
	"openat$camx_dev_actuator*",
	"syz_memcpy_off$CAMX_ACT*",
	"ioctl$VIDIOC_CAM_ACTUATOR_*",

	"openat$camx_dev_eeprom*",
	"ioctl$VIDIOC_CAM_EEPROM_*",
	"syz_memcpy_off$CAMX_EEPROM_*",
	
	"openat$camx_dev_flash*",
	"ioctl$VIDIOC_CAM_FLASH_*",
	"syz_memcpy_off$CAMX_FLASH_*",

	"openat$camx_dev_ois*",
	"ioctl$VIDIOC_CAM_OIS_*",
	"syz_memcpy_off$CAMX_OIS_*",

	"openat$camx_dev_sensor*",
	"ioctl$VIDIOC_CAM_SENSOR_*",
	"syz_memcpy_off$CAMX_SENSOR_*",
	
	"openat$camx_dev_csiphy*",
	"ioctl$VIDIOC_CAM_CSIPHY_*",
	"syz_memcpy_off$CAMX_CSIPHY*",
	
	"openat$camx_dev_cpas*",
	"ioctl$VIDIOC_CAM_CPAS_*",

	"openat$camx_dev_tfe_mc*",
	"syz_memcpy_off$CAMX_TFE*",
	"ioctl$VIDIOC_CAM_TFE_*"
}
```

## Open Challenges

1. The driver is IOCTL call are stateful in nature, the CONFIG_DEV command is there most of them have complex buffer handling is done. So the first thing IOCLT command which need is to configurate the sensor via and IOCTL. For this call to succussed you need provide exact parameter. On successful execution of this IOCTl we can call other ioctl calls. Another limitation is the that these sensor probing IOCTL call is parameter changes for different chipsets.
2. Coverage buffer provided by the kcov sub-system is not enough to accommodate all the coverage generated by system call this lead to undetected coverage and this make feedback coverage not useful.

### Reference Notes

- There is sys-description for 5 devices out of which only two devices are functions present which are JPEG and Request Manager.
- What is the privilege level of the device driver?
- Major Challenges & peculiarity :
	- sub-system device path discover, this solved using medio discovery API's
		- discover this by reading unit-test code
	- shared buffer between user and kernel where the IOCTL command is placed for processing
		- Reading unit-test code and look at how API's are consumed by the SDK or any sort of testing framework.
		- syzkaller [[io_uring]] sub-system has similar patter of ioctl call and taking inspiration from there.
		- [pseudo syscall](https://github.com/google/syzkaller/blob/master/docs/pseudo_syscalls.md) to copy the command into buffer
	- there are different method of interacting with sub-system like jpeg doesn't have standard ioctl calls.
- Understanding the camera driver KMD API usage
	- camx unit-test need seed data and video file
	- driver usually has a UMD sdk which is the consumer of the service which can be used as reference to understand the API usage
	- Linux video(v4l) sub-system provides API to enumerate this by using *MEDIA_IOC_DEVICE_INFO* & *MEDIA_IOC_ENUM_ENTITIES*.
		- since the device path **/dev/v4l-subdev\<numb\>** can map to different sub-system like JPEG, TPG, etc. So this ioctl call help you to discover those mapping.
		- IOCTL return id of the device which then match with ID's defined by **CAM_\<dev\>_DEVICE_TYPE** const value
	- use android-ndk because of camx uapi macro error in normal aarch64 compiler. 
		- We need to figure how this can be fixed in syzkaller integration.
	- camera code register its handler with `struct v4l2_subdev_core_ops` for sub-devices like sensor, jpeg, etc. and `struct v4l2_ioctl_ops` for root devices like request manager and sync devices.
	- TPG Device
		- this device has two command buffer one retrieved in *cam_tpg_packet_parse* function and another in *cam_tpg_validate_cmd_descriptor*
	- Camera IOCTL structure patterns
		- Nested structure which has pointer but the filed type is uint64_t
- Node Manager sub-system is an abstraction for many sub-system like TFE,ISP, etc which have hardware pointer setup during initialization phase which is then call via generic ioctl interface. This pointer are like object-oriented pointer for `struct cam_hw_mgr_intf` which has the generic operation function pointers.
- There are three level of abstraction for the camera driver
	1. Node - this is abstraction for non-sensor top level driver.
	2. Context - maintains the state of top-level sub-system
	3. hardware manager - abstraction for hardware sub-system
	4. each entity is a layer of top of other entity.
- Write syz-prog and execute it with syz-executor to observer the coverage you can look at `dmesg` log or with `adb logcat`.
- hand crafting syzkaller program can to reach deeper part of the driver. Crafting the program to satisfy the top level constrains which helps you to reach the deeper part of the code.
- Debug log enabling commands
	- http://camxsensor/logmask/kmd to create log mask
	- command `adb shell "echo 0xa00002020 > /sys/module/camera/parameters/debug_mdl"`
	- this help when writing manual syz-prog for code path which are easy to reach.
- Camera driver is complicated because of **hierarchy of command buffer** with each buffer have information on how to obtain next level of buffer.
	- The ioctl command buffer has handler which kernel driver uses to find shared buffer between user and kernel.
	- The shared buffer is created using another ioctl command, it assigns a unique handler value which i can pass to other ioctl commands.
		- You can create many such buffer with associated buffer
	- shared buffer can have handler to other shared buffer but in most common case it two level. So for example ioctl command buffer will have level 1 buffer handler and level 1 buffer will have level 2 buffer.
- What are the **metrics** which we can use to present Syzkaller fuzzing progress
	- Code coverage
	- Number of drivers, each driver should have this number
		- Number of attack surface function. In case of android kernel driver fuzzing its IOCTL commands
		- Privilege exposer of the driver for eg kgsl is accessible even to the lowest privilege process. 
	- Bug density of the driver
		- Number of bugs reported so far in a particular
		- Number of Bugs per 1k line-of-code
	- Path saturation - how often a particular code path is fuzzed.
		- This metrics helps us to understand to what extend much a particular grammar has been utilized by syzkaller fuzzer.
		- The problem in syzkaller is that we don't know how well a particular grammar has been tested and is it fully utilized to its full capacity.
		- If a particular code path has been executed very often it means many test cases has been exercised there.
- ISP, MC_TFE - more or less same
	- TFE - Completely different
	- Pakala has only MC_TFE
	- IFE/ISP/MC_TFE is used inter changeably
- JPGE Patch buffer
	- Two kind of buffer - 
		- command buffer 
			- configuration for hardware, also has address of the image, 
			- starting address, 
			- go to different images. 
			- mostly registers
			- mostly from user mode
		- image buffer
		- 3 address, user , kernel & IO address(SMMU)
- for ICP Device you need to have request manager device open before opening ICP device.
- HFI is the queue abstraction API some file and function
	- hfi_write_cmd - function
	- hfi_send_system_cmd
- Entry point function : 
	- cam_packet_util_get_cmd_mem_addr
	- cam_mem_get_cpu_buf
	- cam_mem_get_io_buf
- Interesting thought on buffer arithmetic
	- The problem of buffer size verification arises when the buffer is a variable size buffer. This is usually implemented by having a typed pointer at the end of the structure. The structure will have the count value which will help you to calculate the size of the entire structure.
	- When this structure crosses the privilege boundary and they need to be copied into kernel. The size variable of the structure need to copied in the kernel verify the size is leading of integer overflow and 
	- The tyranny of the Trio (Offset, Size and Pointer)
		- some structure have more that one dynamic size structure which is implemented by have a pair of offset and size fields for each of the sub-structure.
		- offset and size of the structure should be verified before coping or dereferencing it.
		- The sub-structure if they over-lap they the check can be by-passed if there is a write done on one structure and another structure's field is overlapping with verified chunk.
	- sometime you have fixed size array of structure in the structure.
- Constant crashing of device can reduce the exec/min metric since the device spends lot of time in rebooting and coming up to a fuzzable state.
- Using gcov you can also collect coverage of startup function of the driver which are initialize during boot. This way can understand function pointer initialization, since some code is platform dependent.
- the problem with shared buffer is, if some pointer assigned in the buffer and later if the 

### Sub-system Tracking

^c07fb6

- Create a overview of all the device
- Below list has been prepared using media numeration API exposed by linux-v4l sub-system.

**Total IOCLT command - 148**

| Device          | Entry Point                                  | IOCTL Count | Status                  | Comment                                                                          |
| --------------- | -------------------------------------------- | ----------- | ----------------------- | -------------------------------------------------------------------------------- |
| Request Manager | cam_private_ioctl                            | 21          | syz-description exist   | **DOABLE**                                                                       |
| Sync Driver     | cam_sync_dev_ioctl                           | 8           |                         |                                                                                  |
| CPAS            | cam_cpas_subdev_ioctl                        | 5           | **DONE**                |                                                                                  |
| CSIPHY          | cam_csiphy_core_cfg                          | 7           | **DONE**                |                                                                                  |
| EEPROM          | cam_eeprom_driver_cmd                        | 4           | **DONE**                |                                                                                  |
| OIS             | cam_ois_driver_cmd                           | 7           | **DONE**                |                                                                                  |
| TPG             | cam_tpg_subdev_ioctl                         | 6           | **DONE**                |                                                                                  |
| FLASH           | cam_flash_i2c_pkt_parser                     | 6           | **DONE**                |                                                                                  |
| ACTUATOR        | cam_actuator_driver_cmd                      | 6           | **DONE**                |                                                                                  |
| SENSOR          | cam_sensor_driver_cmd                        | 7           | **DONE**                |                                                                                  |
| JPEG            | cam_jpeg_add_command_buffers                 | 6           | **DONE**                |                                                                        |
| LRME            | cam_lrme_mgr_hw_config                       | 6           | syz-description exist   | #node-mngr  this functionality is exposed using function pointer in node manager |
| ICP             | cam_node_handle_ioctl                        | 15          | syz-description exist   | #node-mngr Unable to directly open the device, error in discovery                |
| IFE             | cam_node_handle_ioctl                        | 13          |                         | #node-mngr before pakala part of TFE_MC/TFE/ISP                                  |
| TFE_MC          | cam_node_handle_ioctl, cam_tfe_mgr_config_hw | 13          |                         | **DOABLE**                                                                       |
| TFE             |                                              |             |                         | part of medium chip                                                              |
| OPE             | cam_node_handle_ioctl                        | 10          |                         | #node-mngr not enabled on pakala, part of medium chip                            |
| CRE             | cam_node_handle_ioctl                        | 8           |                         | #node-mngr                                                                       |
| CCI             |                                              |             | syz-description comment | Unable to directly open the device                                               |

### Product Docs
1. [Beginner course](https://degreed.com/pathway/79xxn4yj9k/pathway)
	1. [User to Kernel Interaction ](https://confluence.qualcomm.com/confluence/pages/viewpage.action?pageId=173071273)
	2. [CSL Packet description](https://confluence.qualcomm.com/confluence/display/CamNext/HW+Update+Packet+Interface)
2. [Build camera test cases](https://confluence.qualcomm.com/confluence/pages/viewpage.action?pageId=285442082#SM8450(Waipio)-AU121:LinuxNDK/SparseAUbuildrecipe)
3. [go/camx](go.qualcomm.com/camx)
4. [Syzkaller Guide for Tech Team](https://confluence.qualcomm.com/confluence/display/LnxSec/Syzkaller+guidance+for+tech+team)
5. [Pakala Camera Build](https://confluence.qualcomm.com/confluence/display/CamNext/Pakala%20Mainline%20Build%20Recipe)
6. [go/camxunittest](https://confluence.qualcomm.com/confluence/display/CamNext/CSL%20Unit%20Test)
7. [Android Update](https://confluence.qualcomm.com/confluence/display/QPSIFT/Android+Kernel+Driver+Fuzzing)
8. [Enable GCOV and converting the dat](https://github.qualcomm.com/LinuxSecurity/syzkaller/wiki/Enable-kernel-GCOV)
---

### Bug Tracker

| ID  | CR                                   | Rating | Created On | Comment                                                                                                              |                                                       |     |
| --- | ------------------------------------ | ------ | ---------- | -------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- | --- |
| 1   | https://orbit/CR/3945652             | Medium | 10-10-2024 | CPAS sub-system OOB Write                                                                                            |                                                       |     |
| 2   | https://orbit/CR/3953618             | Medium | 18-10-2024 | CSIPHY sub-system OOB Read/Write                                                                                     |                                                       |     |
| 3   | https://orbit/CR/3957101             | Low    | 23-10-2024 | JPEG Wild pointer free                                                                                               |                                                       |     |
| 4   | https://orbit/CR/3957873             | Medium | 24-10-2024 | Request Manager CAM_REQ_MGR_SCHED_REQ_V3 integer overflow                                                            |                                                       |     |
| 5   | https://orbit/CR/3990693             | medium | 03-12-2024 | Register dumping problem cam_common_user_dump_helper oob write                                                       |                                                       |     |
| 6   | https://orbit/CR/3990727             | medium | 03-12-2024 | Register dumping problem cam_soc_util_dump_dmi_ctxt_reg_range_user_buf and cam_soc_util_dump_cont_reg_range_user_buf |                                                       |     |
| 7   | https://orbit/CR/3991144             | medium | 04-12-2024 | OPE Manager cam_ope_mgr_process_cmd_io_buf_req                                                                       |                                                       |     |
| 8   | https://orbit/CR/4021514             | Low    | 2025-01-09 | OPE cam_ope_packet_generic_blob_handler                                                                              |                                                       |     |
| 9   | https://orbit/CR/4021530             | Medium | 2025-01-09 | cam_packet_util_dump_patch_info in ICP Sub-system                                                                    |                                                       |     |
| 10  | https://orbit/CR/4042291 ~~4022403~~ | Medium | 2025-01-10 | This is a dense CR lots of fields need to be validated in OPE sub-system                                             |                                                       |     |
| 11  | https://orbit/CR/4042296 ~~4022644~~ | Medium | 2025-01-10 | ope_create_frame_cmd_batch in OPE Sub-system                                                                         |                                                       |     |
| 12  | https://orbit/CR/4042285 ~~4022860~~ | Medium | 2025-01-10 | ope_create_stripe_cmd in OPE                                                                                         |                                                       |     |
| 13  | https://orbit/CR/4056304             | Medium | 2025-02-14 | eeprom parse_memory_map signed to unsigned comparision                                                               |                                                       |     |
| 14  | https://orbit/CR/4056616             | Medium | 2025-02-14 | cam_sensor_update_power_settings out of bound read                                                                   |                                                       |     |
| 15  | https://orbit/CR/4056681             | Low    | 2025-02-14 | Infinite loop, functional bug not security issue                                                                     |                                                       |     |
| 16  | https://orbit/CR/4070376             | Medium | 2025-02-27 | TFE sub-system                                                                                                       |                                                       |     |
| 17  | https://orbit/CR/4070420             | Medium | 2025-02-27 | IFE Sub-system                                                                                                       |                                                       |     |
| 18  | https://orbit/CR/4070436             | Medium | 2025-02-27 | cam_tfe_cam_cdm_callback in TFE sub-system                                                                           |                                                       |     |
| 19  | https://orbit/CR/4070441             | Low    | 2025-02-27 | cam_custom_mgr_prepare_hw_update                                                                                     |                                                       |     |
| 20  | https://orbit/CR/4070467             | Medium | 2025-02-27 | "cam_sensor_i2c_command_parser" "header.count" field needs to be validated                                           |                                                       |     |
| 21  | https://orbit/CR/4070498             | Medium | 2025-02-27 | "cam_ois_fw_info_pkt_parser" memcpy in the can lead to out of bound read                                             |                                                       |     |
| 22  | https://orbit/CR/4071376             | Medium | 2025-02-27 | "cam_tpg_validate_cmd_descriptor" TOCTOU                                                                             |                                                       |     |
| 23  | https://orbit/CR/4092273             | Medium | 2025-03-21 | offset validation in "cam_icp_process_stream_settings"                                                               |                                                       |     |
| 24  | https://orbit/CR/4103733             | Low    | Fuzzing    | 02 Apr 2025                                                                                                          | Firmware Crash in ICP sub-system will calling aquire. |     |

---

### Camera Android Driver IRVR Ticket Analysis
[[Camera Driver Incident Analysis]]

---
## Tech Team PoC

| PoC Name | Expertise |
|---|---|
| Suraj Dongre | Camera Driver Architect |
| Kishore Srirambhatla | Mobile Security PoC |
| Jong Guk Im (Jon) | Camx CSL Unitest PoC |
| Sri Ranganath Reddy (Sri) (Temp) | Camera SDK Setup |
| Cherukuri Harika | Pakala Camera SDK Setup |
| Joey Jiao | China syzkaller team |
| Biswajit Roy | Alpaca |
| Anmol Jaiswal | Alpaca |
| Daksha Gouri Patolia | Camera Senior Staff Manager |

---
## Event Logs

| Date | Event |
|---|---|
| #17-06-2024 | Started project setup |
| #21-06-2024 | Syzkaller setup is up and running |
| #21-06-2024 | Started working on Camera Driver |
| #28-06-2024 | Camera Driver TPG Device |
| #27-09-2024 | Started sensor sub-system (Camera) |
| #01-10-2024 | Integrated completely in the QCOM Upstream for 5 sub-system|

## Related Research Papers or Articles

^qcom-linux-fuzz-ref
1. Project Zero Blog on Android Kernel Driver exploitation: [[Android Kernel Driver Bug]]

## Archived Task

- [x] #task #qpsi Setup new pakala v2 device with source code ➕ 2024-12-06 ✅ 2024-12-08 🔒 [[2024-12-09 QPSI All Hands]] 🕸️ Task
- [x] #task #qpsi code review "cam_mem_get_cpu_buf" 🔼 ➕ 2024-12-05 ⏳ 2024-12-06 ✅ 2024-12-10 🔒 [[2025-01-10]] 🕸️ Project Info > Task
- [x] #task #qpsi gcov code report [[Camera Driver Fuzzing]] 🔺 ➕ 2024-12-05 🛫 2024-12-11 📅 2024-12-14 ✅ 2024-12-19 🔒 [[2025-01-10]] 🕸️ Project Info > Task
- [x] #task #qpsi code review "cam_mem_cpu_io_buf" ➕ 2024-12-05 📅 2024-12-13 ✅ 2025-03-19 🔒 [[2025-03-19]] 🕸️ Project Info > Task
- [x] #task #qpsi create pull request for jpeg grammar ➕ 2024-12-05 🛫 2024-12-12 ✅ 2025-03-19 🔒 [[2025-03-19]] 🕸️ Project Info > Task
- [x] #task #qpsi code review function "cam_packet_util_get_cmd_mem_addr" ➕ 2024-12-10 ✅ 2025-03-19 🔒 [[2025-03-19]] 🕸️ Project Info > Task
- [x] #task Check the new rating calculator to re-rate the CR ➕ 2025-04-17 ✅ 2025-07-11 🔒 [[2025-07-11]] 🕸️ Project Info > Task
	- Attacker are now attacking the Kernel driver via daemon services this can change the threat rating.
- [x] #task In the CR table the status of the CR which are fixed should be shown ➕ 2025-04-17 ✅ 2025-07-11 🔒 [[2025-07-11]] 🕸️ Project Info > Task
- [x] #task Action Item ✅ 2025-07-11 🔒 [[2025-07-11]] 🕸️ Project Info > Task
	- Project all possible entry points in collaboration with tech team and conclude the scope of work for fuzzing
	- Add Number of CRs fixed as part of the KPIs
	- Investigate other CRs filed and not found through the fuzzing
	- Follow up with new CR rating guidelines and re-evaluate the Camera driver CRs
