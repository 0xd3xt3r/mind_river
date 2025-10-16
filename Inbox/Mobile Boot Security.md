---
up: "[[Knowledge Base MoC]]"
tags:
  - "#type/knowledge"
status: in-progress
created-date: 2025-08-09
related:
summary: Security of Bootloader and other low-level attack surface
---

## Pre-requisites

1. You first need a fastboot driver from the OEM. fastboot will not work without this drivers. You can find these driver [here](https://developer.android.com/studio/run/oem-usb) but it not a sure shot destination, your vendor is the best source the correct driver.
2. Xiaomi Tools is a third party tool which the the bootloader unlocking.
3. QPST(Qualcomm Product Support Tool) is the semi-public tool which help you the use firehose and Sahara protocol. The tools from *Aleph Security* are developed on top of these tools.
4. To unlock the bootloader sometimes the vendor provides the tool to do it but it comes at the risk of erasing all the data from the phone so be sure to back-up the data before doing it.
5. There is a generic windows drivers from google which works with fastboot and this work for my "Nothing 2 Phone".

## Open questions?

1. What does an unlocked bootloader help you the achieve? What privileges does it bring?

## Notes

1. *fastboot oem <command>* - custom commands from OEM.

## Target

Phone : Redmi Note Pro 5
Code Name : whyred

### USB Issue command

Windows 10 issue 

```bash
# Redmi note pro 5 - code name whyred
# execute this command for Redmi note pro 5 on windows 10

@echo off

reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\usbflags\18D1D00D0100" /v "osvc" /t REG_BINARY /d "0000" /f

reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\usbflags\18D1D00D0100" /v "SkipContainerIdQuery" /t REG_BINARY /d "01000000" /f

reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\usbflags\18D1D00D0100" /v "SkipBOSDescriptorQuery" /t REG_BINARY /d "01000000" /f
```

## Ref

1. [Aleph Security - Qualcomm EDL Part 1](https://alephsecurity.com/2018/01/22/qualcomm-edl-1/)
2. [Aleph Security - Qualcomm EDL Part 2](https://alephsecurity.com/2018/01/22/qualcomm-edl-2/)
3. [Aleph Security - Qualcomm EDL Part 3](https://alephsecurity.com/2018/01/22/qualcomm-edl-3/)
4. [Aleph Security - Qualcomm EDL Part 4](https://alephsecurity.com/2018/01/22/qualcomm-edl-4/)
5. [Aleph Security - Qualcomm EDL Part 5](https://alephsecurity.com/2018/01/22/qualcomm-edl-5/)
6. https://blog.quarkslab.com/analysis-of-qualcomm-secure-boot-chains.html