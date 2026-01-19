---
up: "[[WoS Kernel Driver Fuzzing]]"
tags:
  - "#type/qcom"
related:
status: todo
created-date: 2025-04-07
summary: Setup Glymur Device
---

## Notes
1. CRD Device - Laptop device

## Commands

```bash
emmcdl.exe -p COM9 -f xbl_s_devprg_ns.melf -memoryname SPINOR -x erase0.xml
 
emmcdl.exe -p COM4 -f xbl_s_devprg_ns.melf -memoryname SPINOR -x rawprogram0.xml

emmcdl.exe -p COM5 -f xbl_s_devprg_ns.melf -memoryname spinor -b CDT \\anusat\ProductSW\Glymur\CDT\CRD\GLYMUR_CRD_0.1.0.bin

emmcdl.exe -p com14 -f xbl_s_devprg_ns.melf -memoryname nvme -x WDFlash.xml
 
```
## Tech Team Docs

1. https://qualcomm.sharepoint.com/teams/GlymurTarget/SitePages/RPMB-provisioning-steps.aspx