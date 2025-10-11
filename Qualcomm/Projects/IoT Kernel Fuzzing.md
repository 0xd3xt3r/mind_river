---
up: "[[Qualcomm Project MoC]]"
aliases: QCLinux Kernel fuzzing
tags:
  - "#type/project"
  - "#qpsi/project"
status: on-hold
created-date: 2025-06-10
completed-date: 2025-06-10
---

> **summary**:: The goal of this project is to fuzz the QCLinux kernel. We are trying to achieve this my porting Android harness to IoT kernel.

## Objective
> What were you trying to achieve with this project.

The goal of this project is to fuzz the QCLinux kernel. We are trying to achieve this my porting Android harness to IoT kernel.

---

## Thread Model/Attack Surface
> How can we attack the System?

1. Kernel driver - our leads believe we have to upstream our code as much as possible and we should propagate the fixes as much as possible.
2. Network services - its developer board, there is not much for out end to target.

---
## Commands
> Commands which you important or used very frequently while working on the project

### Build Command

```bash
# MACHINE=qcs9100-ride-sx
# MACHINE=qcs6490-rb3gen2-core-kit

# Sharing details of QCS9100 sync and build command.  

# AU : 522
python <Path for lint_tools>/src/sync_scripts/sync.py <workspace_path> -p AU_LINUX_EMBEDDED_LE.QCLINUX.1.0_TARGET_ALL.01.013.522 -t lkp --hotfixes 5997246/4 5930346/2 6010278/5 5865412/9 6036925/1 5965432/15 5926676/1 6033655/1 6034379/1 6030399/1 6022793/1 6025038/3 5956078/8 6003007/2 6037147/1 6037433/1 6037435/1 6035321/1 6026810/1 6026683/7 6035171/2 2>&1 | tee sync_logs  

# Base BSP
MACHINE=qcs9100-ride-sx DISTRO=qcom-wayland QCOM_SELECTED_BSP=base source setup-environment
bitbake qcom-console-image

# Custom BSP
MACHINE=qcs9100-ride-sx DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment

bitbake qcom-console-image

MACHINE=qcs6490-rb3gen2-core-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=base source setup-environment
# meta: \\crmhyd\nsid-hyd-05\QCS9100.LE.1.0-00230-ADV.INT.DBG-2

cat /proc/config.gz | gunzip > running.config

MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment build-qcom-wayland_cust
bitbake qcom-multimedia-image
```

#### Config 

Kernel configuration are based on this location

```bash
# Kernel configuration
mshelia@hyd-lablnxqpsi05:/local/mnt/workspace/qcom-dev/pakala_build/iot_code/iot_code$ vi layers/meta-qti-bsp/recipes-kernel/linux/linux-qcom-custom/selinux.cfg

# all the configuration
mshelia@hyd-lablnxqpsi05:/local/mnt/workspace/qcom-dev/pakala_build/iot_code/iot_code$ vi layers/meta-qti-bsp/recipes-kernel/linux/linux-qcom-custom/
0001-QCLINUX-Add-support-to-compile-msm_display.ko.patch  smack.cfg
selinux.cfg                                               smack_debug.cfg
selinux_debug.cfg

# default configutaion
mshelia@hyd-lablnxqpsi05:/local/mnt/workspace/qcom-dev/pakala_build/iot_code/iot_code$ vi layers/meta-qti-bsp/recipes-kernel/linux/linux-qcom-custom_6.6.bb

# Meta path
/prj/qct/asw/crmbuilds/crmhyd/builds629/PROD/LE.QCLINUX.1.0-52803-K2C.1-1/apps_proc/custom/build-qcom-wayland/tmp-glibc/deploy/images/
```

#### KASAN Build

The enable KASAN there were no exclusive support from tech team to enable it. So i tried to figure it out how to do it!

```bash
# Qualcomm Linux config
./sources/kernel/kernel_platform/kernel/arch/arm64/configs/qcom_defconfig

CONFIG_KASAN_INLINE=y  
CONFIG_TEST_KASAN=y  
CONFIG_KCOV=y  
CONFIG_SLUB=y  
CONFIG_SLUB_DEBUG=y  
CONFIG_CC_OPTIMIZE_FOR_SIZE=y
```
---

## Notes
> Random interesting information, observation or anecdotes you observed while working on the projects.

- Dependencies should be installed from this [link](https://docs.yoctoproject.org/5.0.10/ref-manual/system-requirements.html#ubuntu-and-debian).
- Kernel Configuration
	- *Gopikishan Thawre* - memory related issues.
	- Default params : line 17-19
		![[Pasted image 20250612152732.png]]
	- line 29 - 30 : conditional
		![[Pasted image 20250612152756.png]]

---

## Challenges & Opportunities
> While working on a project you might encounter challenges which could potentially be solved to further the project.

- The project started a way to port the Android harness to IoT target.
- SElinux policy should be enforced strictly, right now we can one can simple disable is from command line by `setenforce 0`.
- Since we can load a custom driver and we have shell access, it defeats the purpose of have a kernel integrity. 
- Attack surface of the product is different from Android which has a huge user-space attack surface.
- IoT camera IPs is mid-tier chipset for which we haven't written any grammar, since we only prioritize premium tier chipsets.

---
## Hardware

Some hardware on which we tried to work

1. QCS9100 - Lemans
2. QCM6490 - Kodiak RB3-Gen2

## Product Docs
> Some of the important internal documents which would you often need to refer very often.

- [QCLinux Build list](https://confluence.qualcomm.com/confluence/display/IITQ/QCM6490.LA.3.2)
- [Public Docs link](https://docs.qualcomm.com/bundle/publicresource/topics/80-70017-254/flash_images.html#flash-software-using-qdl) for the product we were fuzzing
- [k2cbuilds](http://go.qualcomm.com/k2cbuilds)

---
## CR Raised
> Some of the bugs which you created while working on the project

### Bug Tracker

| ID | CR | Rating | Date | Comment |
|---|---|---|---|---|
| 1 | 01 | High | 2025-06-10| What happened?|


---
## Meeting Notes
> All the important meeting you have done for the project

- New
	- Hello Team,
		1. Kaanapali kernel is being ported to Kodiak (LA.6.0) but it would be taking some time to bring CSS features to be integrated.
		2. Kodiak (QCM6450.LA.3.2) is another strong contender because it has all IoT specific features and has EOL until end of 2026. This enables us to run the fuzzer on CSS specific features. We need to evaluate running of current fuzzer on older kernel @Munawwar Hussain Shelia.
		3. QLI being critical for QCOM IoT, we can try running the fuzzer which has multiplier effect as it is supported by multiple devices.
		4. Ais:
			1. QPSI to evaluate the fuzzer running older Kodiak SP (LA.3.2)
			2. Coordinate with Sai to arrange few devices of Kodiak and Lemans
 
---

## Event Logs

> A brief timeline of the project

1. 2025-06-10 - Start project discussion

## Tech Team PoC

| PoC Name                   | Expertise    |
| -------------------------- | ------------ |
| Hanumantha Reddy Naradla   | Senior Staff |
| Nirmesh Kumar Singh (Temp) |              |
| Kaushik Huzuri             | Build expert | 

---

## Related Research Papers or Articles
> Any research paper or articles which address some the issues which we are facing
- External Research
- Existing IRVR Tickets
---