---
up: "[[Reading MoC]]"
tags:
  - article-notes
  - "#reading/article"
link: https://googleprojectzero.blogspot.com/2024/06/driving-forward-in-android-drivers.html
author: Seth Jenkins
created-date: 2024-12-14
title: Driving forward in Android drivers
summary: Project Zero Article about researching bugs on android driver bugs
---

## Notes

1. find all of the kernel drivers that were accessible from an unprivileged context on each device. There are several different methodologies for discovering these drivers.
	1. the most straightforward technique is to search the associated filesystems looking for exposed driver device files. These files serve as the primary method by which userland can interact with the driver. Normally the "file" is open’d by a userland process, which then uses a combination of read, write, ioctl, or even mmap to interact with the driver. 
		1. Effectively all drivers expose their interfaces through the ProcFS or DevFS filesystems, so I focused on the /proc and /dev directories while searching for viable attack surfaces. Theoretically, evaluating all the userland accessible drivers should be as simple as calling find /dev or find /proc, attempting to open every file discovered, and logging which open attempts were successful.
		2. there is one major roadblock that prevents this approach from being comprehensive - permissions! SELinux and traditional Linux Discretionary Access Control policies can prevent simple filesystem enumeration from discovering all of the accessible device drivers associated on the filesystem. 
		3. A strategy I found helpful in this regard was to examine publicly released kernel source code for the phone model or for similar phone models.
2. */dev* directory is listable from *shell context* which is from debug permission.
3.  there are cases where certain directories are simply not listable from a debugging-accessible non-root context, particularly in */proc*. Device drivers create files in */proc* primarily via the `proc_create()` and `proc_mkdir()` function calls.
	1. It would have otherwise required rooting the phone to discover this driver without analyzing the kernel source code
4. Another useful resource is the SELinux policy itself. This means that the SELinux policy generally reflects what the developers intend to be accessible from an untrusted context.
	1. occasionally a file may not be directly openable via the filesystem, but there may be some alternative method by which an app can ask another more privileged process to open the file on its behalf and hand the associated fd back, after which the app is allowed to read/write/ioctl to the fd itself. One example of this behavior would be the EdgeTPU device on the Pixel 7: