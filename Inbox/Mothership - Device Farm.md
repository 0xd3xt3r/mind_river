---
tags:
  - "#type/sub-project"
  - "#status/active"
status: todo
up: "[[Android Kernel Driver Fuzzing]]"
related:
  - "[[Android Build Commands]]"
  - "[[Syzkaller Fuzzing Farm]]"
created-date: 2026-07-20
summary: Manage Device farm
link: https://qualcomm-confluence.atlassian.net/wiki/spaces/QPSIFT/pages/3201042122/Continuous+Fuzzing+of+Android+Targets
---

> **Summary**:: 

## Build info


## Requirements

- [ ] **Latest build** - 860 : /prj/qct/asw/crmbuilds/crmhyd/nsid-hyd-05/Hawi.LA.1.0-00860-STD.INT-1
- [ ] [Nikko build](https://qwiki.qualcomm.com/quic/Android/hawibuilds/LA.VENDOR.17.6.0_Builds/LA.VENDOR.17.6.0-30302-hawi.1.CONSOLIDATED-3)
- [ ] Cluster setup to scale fuzzing across different machine and have central reporting dashboard
- [ ] Focused testing for specific drivers, for e.g. dedicating 2 device for fuzzing KGSL, or camera driver for 2 weeks.
	- [ ] this will help use to enrich corpus.
	- [ ] syncing all the corpus to be distributed to full farm.
- [ ] Build refresh stability testing process
	- [x] new build should be tested on one device before been flashed on full farm.
	- [ ] create the list of test that will be performed before flashing it to all devices.
- [ ] Cluster Heath monitoring dashboard
	- [ ] what is the uptime of the devices.
- [ ] Focused fuzzing.
	- [ ] focus on specific code section to yield good results
	- [ ] enriching corpus by parsing test cases.
- [ ] Having a debug build from source is important because we need code coverage reports which very important part of our project KPI.
- [ ] The base system will depend on the target we are fuzzing.
	- [ ] Base system : the system to which we will connect remotely and through which we will have access to device, this system will also have the code cloned and build on it, which will flash it to the device.
	- [ ] For Android Kernel Fuzzing target we need Base systems as Linux to run the syzkaller fuzzer.
		- [ ] We can install Linux on virtual environment solution in windows base system and somehow connect it with the target.
			- [ ] We will also have to connect syzkaller and the target code for symbol resolution.
	- [ ] For Windows Kernel Fuzzing base system can be Windows since our fuzzer is in windows so that won't be much of problem.
	- [ ] Also most of the open-source fuzzer are meant to run on Linux so have a base system as Linux is very important.

## Features

1. **Device Identification** - uniquely identify the device such that we can remotely recover and control them.
	1. unique identify is the combination of ftdi serial id and adb id. Together they  will be use to gain access and reboot the device.
2. **Build Flashing** - 
	1. Process of flashing 
		1. first flash the meta on the device and reboot the device to see if the meta is stable and in working condition.
		2. Then flash the kasan build from the qwiki page.
3. **Build management** - Devices has to be flash with meta which we have build with source synced locally.
	1. We need to track following information
		1. meta build id
		2. machine on which the source was build
		3. devices on which this build is flash successfully.
		4. date on which all this activity was carried out
	2. Process
		1. check if the meta path is accessible. 
		2. start with sanity check if the number of ftdi and adb devices are matching
		3. run a simple 'id' command on all the devices and check if they all work fine.
		4. which device could and couldn't be rebooted to fastboot mode.
		5. track the progress of each stage and failure of each stage like, putting the device to fastboot mode, then flashing command of each file. which failed and which file passed.
4. **Fuzzing Metadata**
	1. What fuzzer is run of the device? SFE or Syzkaller
	2. How much time the device spending fuzzing?
		1. how much time the device is online?
		2. how much time is it offline?
5. Error Handling
	1. ERR1 - Flashing might fail with error "Volume Full", this is because partition size of various files have changes to fix this you need to flash the device via firehose and Sahara process which is only possible via Windows OS (XPcatApp).
		1. Nicos has figure a way todo this via [Linux](https://github.qualcomm.com/QPFI/Mobile_Builder)


## Notes
> What did you learn?
- 

## Task
> What needs to be done!
- 

## Project Management

### Meetings

```dataview
TABLE WITHOUT ID
	file.link as "Session",
	up, created-date, participants, summary, tags
FROM !"Templates"
WHERE icontains(related, this.file.link)
AND icontains(tags, "type/meeting")
SORT file.link DESC
```

### Tasks and Questions

```dataview
TASK
WHERE icontains(text, this.file.name)
AND (icontains(tags, "#task") OR icontains(text, "#questions"))
AND !completed
GROUP BY file.name as filename
SORT filename DESC
```

### Archived Task
