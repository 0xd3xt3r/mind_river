---
tags:
  - qpsi/project
status: done
---
# QDI Driver Fuzzing

QDI is *Qualcomm Driver Interface* for its SoC peripherals. 

[Project Tracking Page](https://confluence.qualcomm.com/confluence/display/QPSIRoadMapTeam/QDI+Driver+Fuzzing+Tracking)
[CR Tracking](https://confluence.qualcomm.com/confluence/display/QPSIRoadMapTeam/QDI+Driver+Fuzzing+CR+Tracking)

## Team

- Jyotsna Krishnaswamy - DSP Lead
- Marcel Selhorst 
- Swanand Sapre - Developer fixing the Bugs
- Gaurang Parikh - Mproc Sec Lead 
- Amey Mahajan - Kernel Team

## Command Ref
1. Project Directory - */local/mnt2/workspace2/yshrivas/projects/kw/py_tools/CodeGenerator/*
2. Create Random Test case ` dd if=/dev/urandom of=x.bin bs=1 count=1024`
3. Run a test fuzz case - `/pkg/modem_sw/afl/4.0.0/afl-2.52b/afl-fuzz -m 2000 -i input -o output/ -- ./fuzz @@`
4. Test the new code `make; ./fuzz input/x.bin`
5. */local/mnt2/nredini/slicer/py_tools/CodeGenerator*
6. */local/mnt2/workspace2/yshrivas/projects/kw/py_tools/CodeGenerator/slice_slpi_waipio_dump*
7. search symbol `grep -nr "sym" --include="\*.c"`
8. pull files `scp -r mshelia@infiltrate-linux:/local/mnt2/workspace2/yshrivas/projects/kw/py_tools/CodeGenerator/slice_slpi_waipio_uipc_router/missing slice_slpi_waipio_uipc_router/`
9. get code coverage :
	1. Script path : **/local/mnt2/nredini/slicer/py_tools/utils**
	2. command `python get_coverage.py --html \<project directory\>`

## QDI Email Template

Good day,

My name is Nilo and I work for QPSI-VD. My team and I recently completed a survey of 81 QDI drivers to be included in Waipio releases to spot possible cross-boundary violations between user space and root PD. In particular, we analyzed each driver to find sensitive code locations where user-provided data gets dereferenced and access it without being copied first (e.g., through methods such as copy_from_user and copy_to_user). Currently, we are reaching out each QDI driver’s point of contact (PoC), and share with them our findings so that the teach-teams can inspect our results, and appropriately address the issues we found. Among the other drivers, we also analyzed i2c, for which I found you as the PoC 😊.

Just to give you a little context of our work: Each QDI driver was sliced (i.e., only the code reachable for the driver entry point is considered) and compiled for an out-of-build fuzzing campaign. Then we dynamically analyzed the driver (using AFL and ASAN) until at least the 70% of functions were covered. Within the slice you might see a file called function_stubs.c, which contains functions that were not defined in the slice, but referenced somewhere in the code. We implemented these functions ourselves so that our analysis would proceed.

Attached to this email you can find an archive, which contains the results of our analysis. Let me quickly explain how to it:

-   The archive contains the slice of the driver under src/. You can view the harness we created to fuzz the driver in main.c. We fuzzed the driver by ‘poisoning’ user data, and finding program locations where poisoned data was dereferenced without being copied it first (i.e., through copy_from_user routine).
-   We marked sensitive code locations (i.e., when user-provided data is dereferenced) with the macro ‘BUG_FIX’. BUG_FIX is a temporary patch I applied to the slice so that the fuzzer would continue its analysis and explore deeper paths. You can think this macro as a ‘copy_from_user’.
-   To inspect sensitive code locations, I just grep for ‘BUG_FIX’ and inspect those lines. You might also find lines marked with ‘x-boundary access’ . These lines highlight examples where user-data gets dereferenced and used after the BUG_FIX macro.

Of course, if you need any help understanding our results, or if you have any questions/feedback/suggestions do not hesitate to contact me 😊

## Creating a Slice instruction

#### Steps:

#### Setup Phase

clone the repo, and install dependencies (use dsp branch)

[Clicer Repo](https://github.qualcomm.com/qpsi/py_tools.git)

copy and modify KW to use include files to target arch (by default KW picks up x86 header files). KW location  is /prj/qct/asw/SABin/Linux/Klocwork/Server/

cp -r /prj/qct/asw/SABin/Linux/Klocwork/Server <whatever_path>

cd <whatever_path>/config/; vim clang_options.py

For each version of ClangOptions class add:
'fuse-baremetal-sysroot': [0],
'maggressive-size-opts': [0],

Copy TYPES.xml (from py_tools repo) in user KW plugin folder

`cp <path_py_tools>/CodeGenerator/TYPES.xml ~/.klocwork/plugins/`

Getting the build.log

Slice inputs.: Builds can be found on polaris or sareports

**Build:** SLPI.HY.5.0-00089-WAIPIO-1
**File:** ChipInfoQDI.c
**Function:** ChipInfoQDI_Invoke

**Find the build location and build command**

`findBuild -lo SLPI.HY.5.0-00089-WAIPIO-1`

output

```bash
MainMake: python ./slpi_proc/build/ms/build_client.py WAIPIO_SLPI_PACK --ddp <- build command

LinuxPath:       /prj/qct/asw/crmbuilds/snowcone/builds917/PROD/SLPI.HY.5.0-00089-WAIPIO-1 <- build location
```


**Copy build we want locally.**

`cp -r /prj/qct/asw/crmbuilds/snowcone/builds917/PROD/SLPI.HY.5.0-00089-WAIPIO-1 ./`

Navigate where the build_client.py resides (i.e., the main make file).

`cd ./slpi_proc/build/ms/`

**Clean the build**

`python build_client.py --clean`

**# Using the modified copy of KW, build the project  with trace (-T) and get the output on file**

```
<whatever_path>/bin/kwinject -w -T tf.txt python ./build_client.py WAIPIO_CDSP_PROD  
<whatever_path>/bin/kwinject -t tf.txt -o bs.out
```

Once we know the .c file containing the function we want to fuzz, we find the path of the object file in the build using grok/source insight or grep

`grep -r --include="*.lib" 'ChipInfoQDI.o'`

`cd <path>/build/*_root_libs/*.prod/`

we consider the *lib present in the directory, run size on the them, and pick the one that contains the .o of the .c file we want

The files in the lib will be the files that we consider in our initial slice, which are the files in the next command
`egrep -e '^version|^config|file1.c;|file2.c;' bs.out > bs_sub.out`

Recompile using the subset of files we got (BTFW is just the name of a dummy project. We need it merely for its license.). This command will generate the file tables/build.log

`<whatever_path>/bin/kwbuildproject --url https://oceanic:8087/BTFW --tables-directory tables bs_sub.out`

### How to create a SLICE

Prepare an harness skeleton and copy it in CodeGenerator/generating_harness.py

copy the generated tables/build.log into the slicer folder

`cp tables/build.log <py_tools_dir>/CodeGenerator/`

`cd <py_tools_dir>/CodeGenerator/`

Run cslicer. This will create the conf.yaml

`./clicer new-slice build.log conf.yaml --custom-stubs`

update conf.yaml with function to fuzz and stubs

Example of conf.yaml
```yaml
build_log: build.log
entry_functions:
	- QDI_EVA_Invocation
custom_stubs_info: stub_info.json
kw_path: <whatever_path>/bin
kw_project_url: https://oceanic:8087/BTFW
translation_units:
	- Local_path_tu1
	- Local_path_tu2
	- Local_path_tu3
```

**Run the slice!**

`./clicer slice conf.yaml`

Browse the output directory and check the harness, and update it if necessary

```bash
make the slice
make fuzz
```

## Discussion Questions

- [x] Why are using clang's older version
- [x] Committing/Saving and saving our harness code and other changes.
- [x] Filing Security CR for this project
	- will do continue the discussion on Monday
- [x] Next Driver to work on? almost all of them are done
- [x] dump driver
	- its, ok. mention it in the comment that function call is reference in struct and the struct field is never getting invoked. and function cannot be reached.
- [x] *cdsp_os* coverage by file or whole project?
- [x] *qurt_qdi_buffer_lock* implementation bug.
- [x] *memscpy* implementation
- [x] ipc_router driver buffer_lock overflow. The code on code grok is different from the one in the slice.
	-  use buffer lock2
		- you should also have unlock
	- len sanitization
- [x] handle poisoning is not valid.
	- what Gaurang said was issue is invalid
- [ ] local client check
	![[Pasted image 20211019204213.png]]
- [x] eAI Driver fuzzing results.
- [ ] security rating for CR's
- ADDRESSSPACE_SEPARATION

## Project Meetings Update

- we should raise CRs only for bugs. If we find a boundary violation that we are not sure it's a bug (or we conclude that is not), we have to send an email to the PoC instead of raising a CR, and tell them that we found an unsafe use of variable X, but we are not sure that it can be weaponized.
- Qurt_OS drivers handle both user's input as well as other driver's input. Around these drivers that should be checks similar to 'client == QDI_HANDLE_LOCAL_CLIENT'. If an unsafe access is found, but the client is LOCAL_CLIENT, we don't have to report the issue. Also, when we report bugs, [[Jyotsna]] asked to report those Qurt_OS drivers that do not implement the above check.
- Please document in go/qdifuzzing:
	1. QDI_HANDLE_LOCAL_CLIENT - please document that it trusts user input,c all need to validate.
	2. If a driver calls LOCAL driver, please call it out as detailed as possible - it will need manual review of user input validation.

	Please schedule meeting with your driver tech teams (30 mins) and include me to discuss your crash log. Include this in the invite as a gude for the tech teams

	Problem/background - 1. QDI driver crashes due to potential bugs discovered in fuzzing 2.. Address Space Separation - FR54244 in Nov, will break your driver will break if you invalid user memory access; 3. The driver should have been fixed by now (waipio CS) - FR54233

	ASK: 1. Look for BUG_FIX, 2. Analyze (CR) and fix Fix by Oct, 3. If its shared memory crash, have you been in touch with Qurt - Amey/Jeremy


## Project Update

**Count Bugs** : `ag -c --cc -w FIXME 2>/dev/null | grep -Po '\d+' | awk '{s+=$1} END {print s}'`

**Driver code path :** /local/mnt2/workspace2/yshrivas/projects/kw/py_tools/CodeGenerator/

| Driver ID | Driver Name                      | Coverage | Path                          | Sec Bug | DoS |
| --------- | -------------------------------- | -------- | ----------------------------- | ------- | --- |
| 9         | dump                             | 44       | slice_slpi_waipio_dump        | -       | -   |
| 16        | uipc_router                      | 71       | slice_slpi_waipio_uipc_router | 3       | 1   |
| 48        | memory                           | 71       | slice_cdsp_os                 | -       | -   |
| 49        | qurtos file                      | 71       | slice_cdsp_os                 | 1       | -   |
| 50        | qurtos_local_client              | 75       | slice_cdsp_os                 | -       | -   |
| 77        | gpr_mpd_kernel                   | 62       | slice_adsp_gpr_mpd_kernel     | 1       | -   |
| 73        | cdsp_ceng (unsigned driver)      | 73.5     | slice_cdsp_ceng               | 1       | -   |
| 76        | eai_lib - [[eAI Driver Fuzzing]] | 45       | slice_adsp_eai                |  -      | -   |

> actually, there are a few: gpr (77), eai_lib (76) and fastrpc_loader (68)

## Bug Reports

Bugs that were reported while working on this project

**CR's that were created**
1. 3042725
2. 3041161
3. 3041109
4. 3040927
5. 3040223
6. 3053714
7. 3053742

### Bug 1
```c
case QDI_OS_MEM_POOL_REMOVE_PAGES:
	// local client check is done, does this remove the security vulnerability
	if (client_handle != QDI_HANDLE_LOCAL_CLIENT)
		QURTOS_RETURN_ERROR(-1);
	return mem_pool_remove_pages(&me->pool, a1.num, a2.num, a3.num, (void(*)(void *))a4.num, a5.ptr);
break;
```

which expands to 

```c
static int mem_pool_remove_pages(mem_pool_t *pPool, unsigned first_page,
                                 unsigned last_page,
                                 unsigned flags,
                                 void (*pfnCallback)(void *),
                                 void *argCallback)
{
   int ret;

   qurt_rmutex_lock(&mem_lock);
   ret = mem_forget_pages(pPool, first_page, last_page, flags);
   qurt_rmutex_unlock(&mem_lock);

   if (ret == QURT_EOK) {
      if (pfnCallback) {
	  // you call any address with any argument
         (*pfnCallback)(argCallback);
      }
   }

   QURTOS_RETURN_ERROR(ret);
}
```


### Bug 2

PoC : Amay Mahajan, Amit kulkarni

**Title** : Out-of-bound memory read/write "open_ramfs_mmap_impl" function in "qurtos"

**Description**

Starting from the entry point "open_ramfs_invocation" in the "qurtos", assuming that method is equal to "QDI_MMAP", the function "open_ramfs_invocation" is called, and the "a6.num" parameter is user-controlled. If the value of "a6.num" is set to arbitrary value it can be use to do out of bounds indexing in "dir->vaddr" array and can possibly lead to out-of-bound read/write.

```c
case QDI_MMAP: {

 	void *ret;
	ret = open_ramfs_mmap_impl(client_handle, tmp_dir, a2.num, a3.num, a4.num, a5.num, a6.num);
	return qurt_qdi_copy_to_user(client_handle, a1.ptr, &ret, sizeof(ret));
}
```

The variable "a6.num" is pass as "offset" variable in the  function "open_ramfs_mmap_impl".

```c
static void *open_ramfs_mmap_impl(int client_handle, const struct fs_dirent *dir, unsigned req_addr, unsigned byte_count, unsigned prot, unsigned flags, unsigned offset) {
	[...]

	// offset variable is user controller and not sanitization is done on the cde
	qurt_qdi_handle_invoke(QDI_HANDLE_GENERIC, QDI_OS_MEM_LOOKUP_PHYSADDR, (unsigned)(&dir->vaddr[offset]) >> 12, &ppn);
 	left_pad = (unsigned)(&dir->vaddr[offset]) & 0xFFF;

	// integer overflow 
	page_count = ((byte_count + left_pad) + 0xFFF) >> 12;

	[...]
	// TODO : Need more understanding
	// integer overflow on byte_count can create large memory copy
	byte_count = (offset + byte_count > dir->sz) ? dir->sz - offset : byte_count;

	// target memory which can be corrupted by putting arbitrary value of "offset"
	// this primitive can be used to copy arbitrary data from one place to other
	 memcpy ((void *)(M_PAGE(mem_virt_req) << 12), (const void *)(&dir->vaddr[offset]), byte_count);

}
```

**Steps to reproduce**

1. method = QDI_MMAP
2. a6.num = any large integer value

### Bug 3

**Title** : GRP MPD Kernel Driver using deprecated buffer lock API's

**Description**

In entry point "gpr_mpd_qdi_invocation" in the "grp mpd kernel" there are various call to deprecated "qurt_qdi_buffer_lock" API for buffer locking. Below are location at which the call is used.

```c
case GPR_MPD_CMDID_GET_RX_SIGNAL:
{
qurt_signal_t **ret_signal;
  
AR_MSG(DBG_HIGH_PRIO, "GPR_MPD_CMDID_GET_RX_SIGNAL   refcnt=0x%X", pugpr->qdiobj.refcnt);
ret = qurt_qdi_buffer_lock(client_handle, a1.ptr, sizeof(qurt_signal_t **), QDI_PERM_W, (void **)&ret_signal);
```

k
```c
case GPR_MPD_CMDID_ASYNC_SEND:
{
	ret = qurt_qdi_buffer_lock(client_handle, a1.ptr, size, QDI_PERM_R, (void **)&packet);
}
```


```c
case GPR_MPD_CMDID_PEEK_RX_PACKET: {
	ret = qurt_qdi_buffer_lock(client_handle, a1.ptr, sizeof(gpr_packet_t), QDI_PERM_W, (void **)&ret_packet_header);
}
```

```c
case GPR_MPD_CMDID_READ_RX_PACKET: {
	
	ret  = qurt_qdi_buffer_lock(client_handle, a1.ptr, size, QDI_PERM_W, (void **)&ret_packet);
	
	[...]
	
	ret = qurt_qdi_buffer_lock(client_handle, a3.ptr, sizeof(uint32_t), QDI_PERM_W, (void **)&ret_bytes_copied);
}
```
## Reference

1. [QDI Driver Fuzzing - Part 1](https://web.microsoftstream.com/video/58ab2e1e-b14f-4207-84e9-e82a0c28ab8e "https://web.microsoftstream.com/video/58ab2e1e-b14f-4207-84e9-e82a0c28ab8e")
2. [QDI Driver Fuzzing - Part 2](https://web.microsoftstream.com/video/4a6a83a3-3847-46c4-b27e-f67d89656612 "https://web.microsoftstream.com/video/4a6a83a3-3847-46c4-b27e-f67d89656612")
3. [go/SecureQDI](https://confluence.qualcomm.com/confluence/pages/viewpage.action?pageId=255605162)
4. [DSPSec Campaign Semmle Findings](https://confluence.qualcomm.com/confluence/display/QPSISSR/DSPSec+Campaign+Semmle+Findings "https://confluence.qualcomm.com/confluence/display/qpsissr/dspsec+campaign+semmle+findings")
5. [Project Source Code](https://github.qualcomm.com/qpsi/py_tools.git "https://github.qualcomm.com/qpsi/py_tools.git")
6. [CoreGrok](corebsp-grok-server.qualcomm.com)
7. [Ashish Update](https://qualcomm-my.sharepoint.com/personal/aundirwa_qti_qualcomm_com/_layouts/15/Doc.aspx?sourcedoc={e6499d53-e88e-42a2-b8f3-794a578a0e0c}&action=edit&wd=target%28New%20Section%201.one%7Cc9bf590b-74d7-419c-8f9e-d1572a316a0d%2FQDI%20MDSP%20Fuzzing.%7Ce15266b4-c10f-4b96-a379-370e6e899cb0%2F%29)
8. https://github.qualcomm.com/qpsi/py_tools/

