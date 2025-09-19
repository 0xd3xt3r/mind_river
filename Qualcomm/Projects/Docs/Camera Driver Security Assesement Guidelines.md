---
tags:
  - "#type/sub-project"
  - "#status/active"
status: todo
up: "[[Android Kernel Driver Fuzzing]]"
related: 
created-date: 2025-07-21
summary: Details understanding of camera driver attack surface
---

> **Summary**:: 

## Abstract

This document assumes that the reader has a working knowledge of the C programming language, has completed Qualcomm’s mandatory secure coding training, and possesses a basic understanding of kernel internals. By the end of this document, you should be able to audit the camera driver code, identify its attack surface, and locate user-space entry points interfacing with the kernel.

Throughout the document, you’ll find references to **Syzkaller**, a fuzzing framework specifically designed for testing kernel drivers. Syzkaller is Qualcomm’s primary tool for kernel fuzzing and has been widely adopted internally due to its effectiveness. It continues to yield excellent results.

## Implication of attacks

1. Compromise the Kernel - drivers are loaded in the kernel and compromising the drivers means that the kernel is open to manipulation.
2. Fake Frame Injection – This can undermine the integrity of applications relying on camera input, such as banking or authentication apps.

## Attack surface

This section explains how user-space applications can potentially exploit the kernel via the camera driver.

On Android, the camera driver is heavily protected by **SELinux policies**, which restrict access to the driver node. Only a single process—**Camera Service**—is permitted to open this node. This makes exploitation significantly harder, as an attacker would need root access and would also have to terminate the Camera Service to interact with the driver directly.

Due to these constraints, vulnerabilities in this module are typically rated as **medium to low severity**. However, this creates a challenge: patches may not be propagated across all targets. Devices with weaker SELinux policies may be more vulnerable, and a medium-severity issue in one branch could be a high-severity issue in another.

### IOCTL Interface

This is primary mean by which user-space program communicate with the driver. But the camera driver doesn't simply register the interface via `struct file_operations` which is a the standard way of exposing the driver ioctl interface.

The primary communication channel between user-space and the driver is through **IOCTL** calls. However, the camera driver does not expose IOCTLs via the standard `struct file_operations`. The structure of `file_operations` would look something like this:

```C

static const struct file_operations fops = {
    .owner = THIS_MODULE,
    .unlocked_ioctl = my_ioctl,
    // .compat_ioctl = my_compat_ioctl, // for 32-bit compatibility
    // other file ops like open, release, read, write
};

// Callback function definition.
static long my_ioctl(struct file *file, unsigned int cmd, unsigned long arg);
```

Instead, it uses the **V4L2 (Video4Linux2)** subsystem. Which exposes ioctl interface via `struct v4l2_subdev_core_ops` and `struct v4l2_ioctl_ops` structure. The definition of structure and callback function looks as shown below.

```C
# Method 1 - to expose IOCTL Interface
static struct v4l2_subdev_core_ops cam_actuator_subdev_core_ops = {
	.ioctl = cam_actuator_subdev_ioctl,
#ifdef CONFIG_COMPAT
	.compat_ioctl32 = cam_actuator_init_subdev_do_ioctl,
#endif
};

# Signature of the IOCTL Interface function via Method 1
static long cam_actuator_subdev_ioctl(struct v4l2_subdev *sd, unsigned int cmd, void *arg)

# Method 2 - to expose IOCTL Interface
static const struct v4l2_ioctl_ops g_cam_ioctl_ops = {
	.vidioc_subscribe_event = cam_subscribe_event,
	.vidioc_unsubscribe_event = cam_unsubscribe_event,
	.vidioc_default = cam_private_ioctl, // callback for the exposed IOCTL Interface
};

# Signature of the IOCTL Interface function via Method 2
static long cam_private_ioctl(struct file *file, void *fh, bool valid_prio, unsigned int cmd, void *arg)
```

While auditing this interface we need to take care of nested structure with points and memory handlers. While the top-most ioctl functions have *arg* parameter which is "void \*" is usually copied to local memory via *copy_from_user* but its not need in this case since it is already copied by v4l2 sub-system and thus ensuring some basic security measure.

By grepping above mentioned two structures you can find all sub-systems exposed by camera driver which is roughly 8-10 depending upon which chipset the driver is compiled for. While some of the sub-system is used my mid-tier chipset and some of them are only enable for premium tier chipset. This changing interface create a problem when porting the interface to different chipset by extension to this problem to porting fuzzing harness to different chipsets. The solution to this problem is v4l2 sub-system's interface which we will discuss next.

v4l2 sub-system provides standard API's to interact with the medium sub-system this allow the linux to create user-space SDK making it easy to developer to interactive with media device and thus providing a nice abstraction from vendor specific implementations. And, the camera team has adopted this interface really well which gives us an interesting advantage of identifying the interface by API's and generating syzkaller definitions. 

The interface which I am talking about is /dev/media[0-4]. By calling ioctl interface of this node we can figure out what all sub-systems are enabled for the device/chipset. This API's will help you to enumerate all the sub-system and path on which these interfaces can be opened. I wrote a tiny C program to enumerate the devices and test if the interface can be open by user-space program. This program can also generate the syzkaller definition to open device. This make posting the Syzkaller definitions to never version of the chipset. 

### Shared Memory between kernel and User-space

While this attack surface was hiding in plane sight this interface go my attention when I was analyzing incident reports. Kernel driver uses an interesting way to communicate with the driver. It setups a shared memory page between the kernel and the user-space. This shared memory is populated with commands and once that is done and ioctl call is made referencing this memory region. This then consume this memory to execute the command in the kernel. But the major problem with this approach is that it suspectable to TOUTOC vulnerability. 

## How does this attack surface operates?

The way this works is the user-space program will make ioctl call to request manager sub-system to allocate a memory via `CAM_REQ_MGR_ALLOC_BUF`/`CAM_REQ_MGR_ALLOC_BUF_V2` ioctl commands, then map the memory in the user-space by `CAM_REQ_MGR_MAP_BUF`/`CAM_REQ_MGR_MAP_BUF_V2` ioctl command. By this call the driver will create a memory handler for each memory range and all the future communication will be done via this handler. When the respective sub-system is invoked via ioctl interface the use the buffer handle passed in the request to obtain the memory page which is in kernel address space. The kernel then decodes this buffer to figure-out the command and the parameter of the command to execute. I can this design-pattern as header-body pattern more on this later. The driver using buffer handle to obtain the memory region via call to `cam_mem_get_cpu_buf` and `cam_mem_get_io_buf` function. These function are usually buried 2-3 level down the function call and is invoked by switch command in the following order `VIDIOC_CAM_CONTROL` -> `CAM_CONFIG_DEV`, almost 90% of the time. I have given some details of the function prototype in the flowing code-block.

```C
// Below two function are used to retrieve pointers to the shared buffer.

/**
 * Input  - buf_handle - buffer handler create via request manager sub-system
 * Output - vaddr_ptr -> pointer to the shared memory. The address is in kernel address-space.
 *          len -> size of the buffer
 */
int cam_mem_get_cpu_buf(int32_t buf_handle, uintptr_t * vaddr_ptr, size_t * len)

/**
 * Input  - buf_handle - buffer handler create via request manager sub-system
 *          mmu_handle - handler obtain via CAM_QUERY_CAP ioctl call
 * Output - vaddr_ptr -> pointer to the shared memory. The address is in kernel address-space.
 *          len -> size of the buffer
 */
int cam_mem_get_io_buf(int32_t buf_handle, int32_t mmu_handle, dma_addr_t * iova_ptr, size_t * len_ptr)
```

By grepping these function one can start auditing the code. By following the taint of the output variable you can do very high quality auditing. As this buffer is then type-casted to different complex structures which has buffer offsets and fields used for looping. Interestingly the buffer can also host other buffer handler of similar nature and again obtain other buffer via call to same function i.e. `cam_mem_get_cpu_buf` and `cam_mem_get_io_buf`. Thus creating a hierarchy of buffers between kernel and users. This is why you may find calls to `cam_mem_get_cpu_buf` and `cam_mem_get_io_buf` deep within the code base which have very to find trace to upper-level IOCTL command which is exposed to user-space. But trust me wherever you find these two call you are 99% sure you are dealing with buffer space which can be influenced by the user-space and thus open-to-attack.

### Understanding shared-buffer

The shared memory has a structure which help to parse the command. The buffer starts with structure `struct cam_packet` which has header structure which describes the buffer size and the command id which describe what which the kernel driver has to execute. This is common pattern you will see through the code, I call it as *header-body* pattern. The header is the meta-data which describes the command and size of the buffer and body is the payload/parameter to the command/function which has to be executed.

The `cam_packet` buffer host three different type of buffer descriptor which has *buffer_handler* by the name of *mem_handle*. This is again used to get a shared buffer between user and kernel. Below are the list of buffers:
1. `struct cam_cmd_buf_desc` - this structure has offset which is validated by `cam_packet_util_validate_cmd_desc` function.
2. `struct cam_buf_io_cfg` - this structure doesn't have validation function but it does have a field (name *offset*)which needs to be validated.
3. `struct cam_patch_desc` - this structure has two handler *dst_buf_hdl*, *src_buf_hdl* and corresponding *dst_offset* and *src_offset* which are offset in the respective buffer. Both offsets needs to be validate unfortunately there is not function to do so.

> And I would say this a very big surface because almost all the sub-systems expose their functionality by this interface. And as you dive deep into its implementation it only get more wired and complex.

### Mitigation Strategy

1. The primary problem with the shared memory page is that the user-space application can change value while kernel is performing some operation on the data this TOCTOU. To mitigate this kernel has to copy the data from shared-memory to kernel memory and then do the operation. Since this is a common operation the code base has nice utility function `cam_packet_util_copy_pkt_to_kmd`. The structure which copies frequently is `struct cam_packet` which has couple of offset field which is validated by the `cam_packet_util_validate_packet` function. So the correct sequence call is `cam_mem_get_cpu_buf` then call `cam_packet_util_copy_pkt_to_kmd` to copy the data to the kernel followed by `cam_packet_util_validate_packet` to validate the offsets.
2. Another common checks which is missed is overlaying of buffer. The bigger buffer is divided into smaller chunks, the start of the buffer is located by offset from the base. When the validation is done it only validate the start of the buffer but it could so happen that the start is within the valid range of the parent buffer but the size of the buffer exceeds the buffer it is stretching itself to the neighboring heap thus leading to OOB read/write. 
3. It is interesting to note that the user-space program and the kernel driver are reference the same memory page but the user-space program has address in the user-address-space and the kernel driver which is again referring to the same kernel page is has a address which is in kernel-address-space. It is important to know this because when the kernel driver obtain the pointer to this memory region and it if does pointer arithmetic when running the commands and if it is not carful, it may breach the page boundary leading to Out-of-bound memory read/write problem. This OOB is more severe because the you can read/write stuff from adjacent  memory page which could be hold critical kernel memory data which can help create more reliable exploits.
