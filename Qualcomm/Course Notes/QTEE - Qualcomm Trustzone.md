---
up: "[[Qualcomm MoC]]"
tags:
  - "#type/qcom"
summary: Qualcomm trustzone training
---

# Qualcomm Trusted Execution Environment (QTEE)



## Introduction

The Secure Code: QTEE online tutorial is designed to raise awareness of security issues commonly encountered in the design and implementation of TrustZone applications and services. You'll learn how to apply the QTEE Common Security Validations checklist when writing or reviewing TrustZone applications and services. You must complete this course to have access to check in code to Perforce TrustZone repositories.

> This course is for QTEE 4.0 and higher

![[Pasted image 20210709191803.png]]

1. **Qualcomm Secure Execution Environment (QSEE)** is the old name for **Qualcomm Trusted Execution Environment (QTEE)**
2. [[QTEE - Qualcomm Trustzone]] is Qualcomm's [[TrustZone]] Implementation.
3. The communication between secure work and non-secure world is done via secure monitor system calls. SMC calls are done at high privilege level.

## Basic Terminology

1. HLOS - non-secure world OS.
1. QTEE Kernel - Secure world OS kernel. This kernel is isolated from non-secure world kernel
1. QTEE Applications - Application running in TrustZone user-level privilege they are isolated from non secure world OS and from each other inside.

## Memory Blacklisting

1. When a system call is done from non-secure world to secure world request and response buffer's are passed to send data to service a request and data is return to non-secure world into response buffer. Attacker who has full control of the non-secure world kernel can craft malicious request and pass arbitrary  pointer to achieve read write in secure world.
2. When dealing with buffer provided by HLOS for request/response always do validation if the buffer memory range is in non-secure world only. Attackers may pass pointers in secure world memory space and may corrupt critical kernel data-structures.
3. Validation of the pointer is done with **tzbsp_(ptr, size)** function.

## Cryptographic Primitives

## Listener Services

## QTEE Application API's

## Mink Services

## Secure Programming Guidelines for TrustZone [[Qualcomm]]

### Input Validation
- Validate pointers to top-level request and response buffers.  
- Validate pointers to members of top-level request and response buffers.  
- Validate length, index and offset parameters.  
- Pay attention when calculating length: *sizeof(ptr)* vs. *sizeof(buffer)**.

### Race Condition
- Lock-down (xPU-protect) shared buffers and their structure members before accessing them.  
- Alternatively, you may make a local copy of shared buffers in Secure Memory.  
- DO NOT forget to unlock on errors, abort or object destruction.  
Race Conditions

### Expose Sensitive Functionality
- DO NOT expose generic, low-level functionality to the HLOS.  
- DO NOT expose provisioning functionality in commercial devices.  
- DO NOT expose debug functionality or logging functionality in commercial devices.  
Exposing Sensitive Functionality

### Using Cryptography
- DO NOT invent “home-cooked” cryptographic schemes.  
- DO NOT share key material between multiple TAs or QTEE services.  
- Generate random numbers securely, always use hardware random number generators when possible.  
- Use hardware crypto engines if available.  
- Securely erase cryptographic key material from memory, e.g., via secure_memset().  
- Use timesafe functions such as timesafe_memcmp()to avoid timing side channels.  
- Pay attention to data dependent branching to avoid cache side channels.  

### Listener Security  
- Validate pointers to request and response parameters from QTEE applications.  
- Validate pointers, length, and offset parameters in listener responses.  
- Check that the listener buffer is large enough to contain the listener request or response.  
- Securely erase listener request structures to prevent the leakage of padding bytes.  
- DO NOT return pointers to kernel memory as service “handles”.  

### Mink Services Security
- Use the mink_invoke() interface to implement QTEE services.  
- Validate pointers to request and response parameters from QTEE applications.  
- DO NOT return pointers to kernel memory as service “handles”.  
QTEE API Security  
- DO NOT pass pointers within Mink buffer arguments.  
- DO NOT access memory beyond the size of a buffer argument.  
- Minimize the functionality exposed by a single Mink service.  

### Trusted Application Security

- Follow the principle of least privilege.  
- Minimize the number of services each TA can access.  
- DO NOT share the derived TA key with any other application or the HLOS.  
- DO NOT confuse a (dynamic) process ID with the unique application ID.  
- Recall that TAs may be unloaded at any time. If persistence is required, the usage of SFS or RPMB is recommended.  
- Recall that TAs may be 32- or 64-bit --> pay attention to sizeof().

## Reference
1. [ARM trustzone Archi](https://confluence.qualcomm.com/confluence/display/QPSIPT/01+ARM+TrustZone+Architecture)
2. [Course Link](https://learning.qualcomm.com/course/view.php?id=23799)
3. [Github Code Repo](https://github.qualcomm.com/qpsi/QTEE-Training)
4. [Internal Docs](https://confluence.qualcomm.com/confluence/display/QPSIPROJECTS/QPSI+Guidance+on+HYP+vs.+TZ)