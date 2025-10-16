---
tags:
  - "#qpsi/project"
  - "#variant-analysis"
  - "#qpsi/irvr"
  - "#type/sub-project"
status: todo
created-date: 2024-12-11
up: "[[WoS Kernel Driver Fuzzing]]"
summary: Analyze the security incident
related: "[[Project - Variant Analysis]]"
---

**Root CRs** : https://orbit/CR/3992803,  https://orbit/CR/3992706

## Objective

## Incident Details

For [https://orbit/CR/3992706](https://orbit/CR/3992706 "https://orbit/CR/3992706"):

This is a use-after-free issue of pFileContext in jpeg hw driver.

While pFileContext was allocated in [**JpegEvtFileCreate**](http://win-0222:8080/source/s?refs=JpegEvtFileCreate&project=SC8380X.WP_HA.1.0 "http://win-0222:8080/source/s?refs=JpegEvtFileCreate&project=SC8380X.WP_HA.1.0"),  it is then used in callback functions JpegHwCallback() and kernel thread will receive message from HW and trigger the callback, hence will also have reference of pFileContext .

But while user could trigger [**JpegEvtFileClose**](http://win-0222:8080/source/s?refs=JpegEvtFileClose&project=SC8380X.WP_HA.1.0 "http://win-0222:8080/source/s?refs=JpegEvtFileClose&project=SC8380X.WP_HA.1.0")() to close device handle,  pFileContext was naturally recycled by MS framework.

At the same time,  our kernel thread still hold reference of pFileContext, which caused use-after-free.

For [https://orbit/CR/3992803](https://orbit/CR/3992803 "https://orbit/CR/3992803"):

This a use-after-free of pFileContext->hJpegCoreDevice.

In JpegDeviceOpen, pFileContext->hJpegCoreDevice was allocated.   pFileContext->hJpegCoreDevice content were put in a queue in [SCAQueueEnqueue](http://win-0222:8080/source/s?defs=SCAQueueEnqueue&project=SC8380X.WP_HA.1.0 "http://win-0222:8080/source/s?defs=SCAQueueEnqueue&project=SC8380X.WP_HA.1.0")().

Then in kernel thread function [**jpeghw_enc_job_scheduler_thread**](http://win-0222:8080/source/s?refs=jpeghw_enc_job_scheduler_thread&project=SC8380X.WP_HA.1.0 "http://win-0222:8080/source/s?refs=jpeghw_enc_job_scheduler_thread&project=SC8380X.WP_HA.1.0"), it will try to fetch some item from the Queue by [SCAQueueDequeue](http://win-0222:8080/source/s?defs=SCAQueueDequeue&project=SC8380X.WP_HA.1.0 "http://win-0222:8080/source/s?defs=SCAQueueDequeue&project=SC8380X.WP_HA.1.0")().

Also, the user space could trigger [jpeghw_enc_device_close](http://win-0222:8080/source/s?defs=jpeghw_enc_device_close&project=SC8380X.WP_HA.1.0 "http://win-0222:8080/source/s?defs=jpeghw_enc_device_close&project=SC8380X.WP_HA.1.0")() and free pFileContext->hJpegCoreDevice.

As the kernel thread still using the pFileContext->hJpegCoreDevice, it is another use-after-free.

You could see both use-after-free are caused by the kernel thread holding some memory while user could free the memory freely.

If we look at the code carefully, we could see that there are more potencial UAF issues, like  pFileContext->InputBuffer, and pFileContext->OutputBuffer  etc.

In order to fix these UAF security issues, changes are needed in jpegHW driver code.

1. We strongly recommend that when we Close the device handle,  we should also make sure that the kernel thread are also ended.
2. We should either add locks to items in pFileContext or introduce reference count to track pFileContext, If pFileContext are in use,  it could not be freed or deleted.

Reference count is better choice than locks,  as too many locks might make the code very complicated and fragile.

We could have a short discussion about this for any concerns.


## Notes


- [x] [[Murali]] #task wants me to do analysis on this CR
