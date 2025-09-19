---
tags:
  - "#qpsi/project"
  - "#type/sub-project"
status: archived
up: "[[Android Kernel Driver Fuzzing]]"
related: 
created-date: 2024-12-15
summary:
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

# KGSL Fuzzing

## Bug Report

### Bug 1

# Linux syzkaller fuzzing - Camera

## Objective
1. The objective of this project is to improve the code coverage of the Waipio and Kailua. 
1. New mutation strategy which explores more 	in-depth of state machine drivers like camera and wifi.

## Notes
1. Understanding of [[Syzkaller Internals]] internals [link](https://github.com/hardenedlinux/Debian-GNU-Linux-Profiles/blob/master/docs/harbian_qa/fuzz_testing/syz_analysis.md)
2. Extract grammar for simple drivers
3. For [[stateful]] thing we need to understand the module
4. Kernel code coverage is block based coverage which won't generate line to line coverage report like ASAN which is a PC-based coverage. Block-based coverage give you better performance which is good for fuzzing.
5. Try clubbing system calls in syz and linux.
6. Existing Fuzzing Hub instance [link](http://hyd-lablnx030:10000/)
7. **warn_on_once** to print stack trace of function point
8. When you say hardware communication! where is the target hardware? is it in different domain?
9. How does the kernel communicate with the hardware using what interface?
10. This command will enable camera driver debug logs in dmsg - `adb wait-for-device shell "echo 0x1000012 > /sys/module/camera/parameters/debug_mdl"`
11. dlopen & dlsym in not same in android and linux, android have obfuscate the pointers for some linker security protection
	1. https://blog.quarkslab.com/android-runtime-restrictions-bypass.html
12. [lldb debugging on android](https://github.com/dotnet/diagnostics/blob/main/documentation/building/android.md)

adb wait-for-device shell "echo 0xx101a23a > /sys/module/camera/parameters/debug_mdl"
## Docs
1. [Beginner course](https://degreed.com/pathway/79xxn4yj9k/pathway)
	1. [User to Kernel Interaction ](https://confluence.qualcomm.com/confluence/pages/viewpage.action?pageId=173071273)
	2. [CSL Packet description](https://confluence.qualcomm.com/confluence/display/CamNext/HW+Update+Packet+Interface)
2. [Build camera test cases](https://confluence.qualcomm.com/confluence/pages/viewpage.action?pageId=285442082#SM8450(Waipio)-AU121:LinuxNDK/SparseAUbuildrecipe)
3. go/camx

## Entry Point

### Camera Driver

These are the various entry point which have zero coverage. I have categorized them into the category due which they are not covered.

#### Bind/unbind function

These category of functions initiailzing/deinitialzing of device and not in scope of fuzzer. Syzkaller is not aware of the init/dinit funcition and it also doesn't have the ability to schedule them.

1. cam_cpas_dev_component_bind
2. cam_sensor_component_unbind
3. cam_sensor_i2c_component_unbind
4. cam_hw_cdm_component_bind
5. cam_vfe_component_bind
6. cam_ois_i2c_component_unbind
7. cam_ois_component_unbind
8. cam_eeprom_i2c_component_unbind
9. cam_eeprom_component_unbind
10. cam_actuator_platform_component_unbind
11. cam_actuator_i2c_component_unbind
12. cam_req_mgr_component_master_bind
13. cam_hw_cdm_component_unbind
14. cam_sensor_component_bind
15. cam_sfe_component_bind
16. cam_flash_i2c_component_bind
17. cam_csiphy_component_bind
18. cam_eeprom_spi_driver_probe
19. cam_eeprom_spi_driver_remove

#### Async Function

These function are deferred function.Deferred work is a class of kernel facilities that allows one to schedule code to be executed at a later timer. These functions will be triggered in the background and will have be difficult to collect coverage as they will have no assoicated context i.e. for which process is the code executing for.

1. cam_cci_write_async_helper
2. cam_req_mgr_process_error
3. cam_req_mgr_process_trigger

#### Disabled due to configuration options

This is compile time configuration so the code might not be there in the final binary based on configuration.

These function are enabled only in compatibility configuration mode.
    1. cam_csiphy_subdev_compat_ioctl
    2. cam_tpg_subdev_compat_ioctl
    3. cam_sensor_init_subdev_do_ioctl
    4. cam_ois_init_subdev_do_ioctl
    5. cam_eeprom_init_subdev_do_ioctl
    6. cam_actuator_init_subdev_do_ioctl

This function is disabled because of configuration during bind/unbind
    cam_flash_i2c_power_ops

#### Not found in code

cam_sensor_process_evt - Didin't find this function in the code

#### Function table for handling IRQ callbacks

These function process hardware interrupt request. This will require special efforts to trigger and not in the current scope of syzkaller fuzzer.

1. __cam_isp_ctx_fs2_buf_done_in_epoch
2. __cam_isp_ctx_fs2_buf_done_in_applied
3. __cam_isp_ctx_buf_done_in_bubble_applied
4. __cam_isp_ctx_buf_done_in_bubble
5. __cam_isp_ctx_buf_done_in_applied
6. __cam_isp_ctx_buf_done_in_epoch
7. __cam_isp_ctx_buf_done_in_sof

#### Misc

These function need further investigation

1. cam_sensor_apply_request
2. cam_sensor_notify_frame_skip
3. cam_actuator_apply_request
4. cam_sensor_power
5. cam_icp_v1_process_cmd
6. cam_mem_mgr_request_mem - init triggered function should be triggered
7. cam_cpas_hw_process_cmd - hardware processed interface


## Build Notes
1. Use this patches for stable reload modules
	1. `gpull cp 3795285 3663493 3885848 3676500/3 3621256 3796492 3824574 3962335 3778944 3796859`
2. `mount -t debugfs debugfs /sys/kernel/debug`
3. Working build receipt by Archana [link](https://qwiki.qualcomm.com/quic/Android/waipiobuilds/LA.VENDOR.1.0_APT_Builds/LA.VENDOR.1.0-28302-WAIPIO.1.CONSOLIDATED-1)
4. Flash meta build image `python <meta_root>\common\build\fastboot_complete.py`
	1. Meta build is image which is clean image build by the team, it has very high possibility of executing the image successfully.
5. Flash Build - To flash the boot put the device in fastboot mode by running the command `adb reboot bootloader`. Then go to directory which has following image file \*.elf and \*.img to and run the below command
	```bash
	fastboot flash abl abl.elf
	fastboot flash boot boot.img
	fastboot flash dtbo dtbo.img
	fastboot flash metadata metadata.img
	fastboot flash persist persist.img
	fastboot flash vendor_boot vendor_boot.img
	fastboot flash vbmeta vbmeta.img
	fastboot flash vbmeta_system vbmeta_system.img
	fastboot flash super super.img
	fastboot flash userdata userdata.img
	fastboot reboot
	```
6. Collect [[Strace]] for application
	1. get package list `adb shell pm list packages -f`
	2. `adb shell setprop wrap.com.android.calendar '"logwrapper strace -f -o /data/local/tmp/strace/strace.com.android.calendar.txt"'` [link](https://source.android.com/devices/tech/debug/strace)
	3. lunch an application `adb shell monkey -p com.estrongs.android.pop -v 500`
		1. `adb shell monkey -p your.app.package.name -c android.intent.category.LAUNCHER 1`
	4. capture the screen  
		1. screenshot `adb exec-out screencap -p > screen.png`
		2. video recording `adb shell screenrecord /sdcard/example.mp4`
	5. Get name of application on the top `adb shell dumpsys window windows | grep -E 'mFocusedApp'| cut -d / -f 1 | cut -d " " -f 7`
	6. Wakeup screen `adb shell input keyevent KEYCODE_POWER` and `adb shell input touchscreen swipe 1000 500 0 0 1000`
7. Disabled camera service before running it
	1. `adb shell "ps -A | grep camera.p"` - to get info about running camera service
8. [How to build Bitcode file for project](https://confluence.qualcomm.com/confluence/display/LnxSec/How+to+generate+bc+for+Linux+Android+Kernel)
	1. [wllvm](https://github.com/travitch/whole-program-llvm)

## Challenges
- [x] Coverage report is not good
- [x] Device restart changes module address
- [ ] Generate Mutation of system calls
	- [x] write python script to this
	- [x] filter out disconnected node from graph
	- [ ] filter out invalid calls to get only valid calls
- [x] syzkaller pseudo call for initializing each state
- [x] run program in device can collect coverage using syz-execprog
- [ ] Visualize the coverage as function graph
- [ ] Make code code coverage in ghidra
- [ ] Convert the existing unit test binary into syzkaller integrated library
	- [ ] Collect kernel coverage for existing unit test cases

- Creating syzkaller mutation for selected set of sub-sytem
	- Create python script to run syz-mutate on selected set of syscalls
	- visualize the corpus file in dot file
- Linux Kernel Coverage Collection library using kcov sub-system.
	- Create C program for this & Write blog post [[Linux Kernel Coverage with kcov]]
	- created python code for processing this coverage along with /proc/module coverage
- Convert the existing unit test program too fuzzing the driver and also to integrate the program into syzkaller


## Papers


## Patents

### Existing patents
- https://patents.google.com/patent/JP2018005890A/en
- [Program execution coverage expansion by selective data capture](https://patents.google.com/patent/US10713151B1/en)
	- Microsoft Patent
- https://patents.google.com/patent/CN106649075A/en 
	- this patent is talking about AFL
- https://patents.google.com/patent/US20160239407A1/en?q=(fuzzing+unittest)&oq=fuzzing+unittest&page=2

### Our Claim

- We can use existing code base to harvest new test cases for fuzzy testing in environment where there is too much hardware dependencies or the code is very stateful. 
- In these scenarios unit/integration test case can be used to extract that knowledge of function/API class that has to be made to module to bring the device/process in the state from which we have to fuzz it.
- Lot of code doesn't get covered because there are lot of validation are put to reach that code these validation can have state validation which get set if certain other code but the device in that state.
- We need to study the code to understand how can we reach that state so that code behind that state can be executed. But if we can hook the functions which is sending untrusted data mutate that then we can have reach deeper parts of the code.
- In stateful application use cases are like AFL's input corpus


---


## Ref

1. [syzkaller Internals](https://github.com/hardenedlinux/Debian-GNU-Linux-Profiles/blob/master/docs/harbian_qa/fuzz_testing/syz_analysis.md)
2. [How to use KCOV interface to collect kernel coverage](https://01.org/linuxgraphics/gfx-docs/drm/dev-tools/kcov.html)
3. https://zhyfeng.github.io/
