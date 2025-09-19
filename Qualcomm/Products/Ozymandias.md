# Ozymandias

The Static analysis tool that uses symbolic execution

## Task

## Paper Reading

- [All You Ever Wanted to Know about Dynamic Taint Analysis and Forward Symbolic Execution (but might have been afraid to ask)](https://oaklandsok.github.io/papers/schwartz2010.pdf)
- [(State of) The Art of War: Offensive Techniques in Binary Analysis](https://oaklandsok.github.io/papers/shoshitaishvili2016.pdf) ^84f10f
	- Two trade-off's 
		- Replayability
		- Semantic Insights
	- Challenges
		- CFG Recovery
			- challenges
				- Indirect Jump
					- Computed
					- context sensitive like callbacks
					- Object sensitive like C++ objects and virtual functions
		- Graph based vulnerability discovery
			- Control flow graph
			- data flow graph
			- control dependency graph
		- Value Set Analysis ([[VSA]]) - at instruction X1, a particular register holds values in range [0, 100]
			- VSA is based on abstract interpretation.
- [A Survey of Symbolic Execution Techniques](https://arxiv.org/pdf/1610.00502.pdf)
	- A fundamental idea to cope with these issues and to make symbolic execution feasible in practice is to mix concrete and symbolic execution: this is dubbed concolic execution, where the term concolic is a portmanteau of the words “concrete” and “symbolic”
	- **Dynamic Symbolic Execution**: One popular concolic execution approach, known as dynamic symbolic execution (DSE) is to have concrete execution drive symbolic execution.
	- **Selective Symbolic Execution. S2E**: takes a different approach to mix symbolic and concrete execution based on the observation that one might want to explore only some components of a software stack in full, not caring about others. Selective symbolic execution carefully interleaves concrete and symbolic execution, while keeping the overall exploration meaningful
	- **Path Selection Algorithms**
		- Several works, such as EXE, KLEE, Mayhem , and S2E, have discussed heuristics aimed at maximizing code coverage.
		- the **coverage optimize search** discussed in [[KLEE]] computes for each state a weight, which is later used to randomly select states. The weight is obtained by considering how far the nearest uncovered instruction is, whether new code was recently covered by the state, and the state’s call stack
		- Of a similar flavor is the heuristic proposed in [71], called **subpath-guided search**, which attempts to explore less traveled parts of a program by selecting the subpath of the control flow graph that has been explored fewer times.
		- **shortest-distance symbolic execution** does not target coverage, but aims at identifying program inputs that trigger the execution of a specific point in a program. The heuristic is based however, as in coverage-based strategies, on a metric for evaluating the shortest distance to the target point. This is computed as the length of the shortest path in the inter-procedural control flow graph, and paths with the shortest distance are prioritized by the engine.
	- **Symbolic backward execution (SBE)**: is a variant of symbolic execution in which the exploration proceeds from a target point to an entry point of a program. The analysis is thus performed in the reverse direction than in canonical (forward) symbolic execution
		- A crucial requirement for the reversed exploration in SBE, as well as in CCBSE, is the availability of the inter-procedural control flow graph which provides a whole-program control flow and makes it possible to determine the call sites for the functions that are involved in the exploration.

### Notes

## Video Recordings
- [Video 1](https://qualcomm-my.sharepoint.com/:v:/g/personal/nredini_qti_qualcomm_com1/ESEUSO1GPjBBk_6C6VFrzocBsPx52vwQTqf2kqDAzKUNLg)
	- This video is about the project overview and gives a brief code walk-through.
- [Video 2](https://qualcomm-my.sharepoint.com/:v:/p/mshelia_qti/EenHzMACYzpGnGZVqbNdk_8Bk2GIWK8B2qC6kgRT1fSW9g)
	- this video is about post processing and addr2line tool issues
- [NAS team internal sessions](https://qualcomm-my.sharepoint.com/personal/bkalyani_qti_qualcomm_com/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fbkalyani%5Fqti%5Fqualcomm%5Fcom%2FDocuments%2FLync%20Recordings)
- [LTE Architecture](https://www.youtube.com/watch?v=oIOOB-rc2oU&list=PLgQvzsPaZX_bimBc5Wu4m6-cVD4bZDav9&index=2&ab_channel=IrfanAli)
- [Wireshark PCAP Analaysis](https://www.youtube.com/watch?v=w4nME5Q3zQ4&ab_channel=TelcoBytes)
- [Navigating Ozy Cmd interface to review bugs](https://qualcomm-my.sharepoint.com/:v:/p/mshelia_qti/Ec3raGJVqRhGsJBQM6sQS0YB9yY18l4GqP03cVCe95QZIA)


## Suggestions & Ideas

- Make ghidra a UI for ozy

![[Pasted image 20211101223356.png]]

## Knowledge
- Keyword search in ms streams : **QCT/MST Air Interface**
- LTE Protocols
	- LTE Basics
	- RRC (Radio Resource Control) Protocol - [Part 1](https://web.microsoftstream.com/video/542710b4-76c5-49ea-a325-69d3cc2a8d02), [Part 2](https://web.microsoftstream.com/video/98d78f61-9004-4cc3-9d4d-44171151214b)
	- NAS (Non Access Stratum) Protocol

## Project
1. Improve addr2line to give accurate output of
2. Path estimation 
3. improve drawf symbol
4. tainting and untiatning stragery
5. checkers

## Ozy Commands
Docker password : zNyQfmzBGJOlRZSG3q4rago+3eXP8UgHR/u/E/BDYlpBXdbSJEaJyfBFGWzqTiV5

###  Docker Cmd
1. Build Docker image : `docker build -f docker/Dockerfile -t ozy_mshelia .`
2. Get the shell in docker `docker run -v /local/mnt/workspace/mshelia/ozymandis:/mob_bin -it ozy /bin/bash`
3. Get a shell in the running docker `docker exec <containter name> -it bash`

### Ozy Cmd
1. Nilo cmd
	`OZY_SKIP_MEMCPY=1 OZY_SINGLE_THREAD=1 python ozymandias/ozymandias.py -s -j 15 /local/mnt2/nredini/builds/apps.tz.2.0.29140184/qtee_tas/apps/bsp/trustzone/qsapps/evautil/build/evautil.elf -c config/ta.json -t 60 -xn -w /local/mnt2/nredini`
	j - no of thread
	t = time out
	w - work directory
2. Run analysis : `OZY_SKIP_MEMCPY=1 python ozymandias/ozymandias.py -s -j 15 /mob_bin/nas_unstrip  -c /mob_bin/test1.json -t 60 -xn -w //home/ozymandias/ozymandias/`
3. `OZY_SKIP_MEMCPY=1 python ozymandias/ozymandias.py -s -j 15 ../../ozymandias/nas_unstrip  -c config/test_stripped.json -t 60 -xn -w .`
4. `python ozymandias/ozymandias.py -s -j 15 ../../ozymandias/NAS_MSDEV_waipio.gen.testQ.exe  -c config/test_stripped.json -t 60 -xn -w .`

## NAS 

1. Entry pointer
	1. [msgri_client_register](https://dgps.qualcomm.com/code/s?refs=msgri_client_register)
	2. [msm_handle_mbms_cm_generic_cmd](https://dgps.qualcomm.com/code/s?refs=msm_handle_mbms_cm_generic_cmd) - This function handles MBMS_CM_GENERIC_CMD which comes from MBMS Apps (via CM)
	3. [msm_command_handler](https://dgps.qualcomm.com/code/s?refs=msm_command_handler) - This function handles all the commands from other layers
	4. [emm_process_cnm_command](https://dgps.qualcomm.com/code/s?refs=emm_process_cnm_command)
	5. [emm_connection_setup](https://dgps.qualcomm.com/code/s?refs=emm_connection_setup)
	6. [emm_process_ul_sms](https://dgps.qualcomm.com/code/s?refs=emm_process_ul_sms) - This function passes uplink SMS messages to NW'
	7. [decode_nas_msg](https://dgps.qualcomm.com/code/s?refs=decode_nas_msg) - This function decodes all the incoming NAS message and puts it into the 'C' structure
		1. [mm_emm_normal_msg_handler](https://dgps.qualcomm.com/code/s?refs=mm_emm_normal_msg_handler)
	8. [lte_nas_decode_emm_message](https://dgps.qualcomm.com/code/s?refs=lte_nas_decode_emm_message) - This function decodes the incoming NAS message
	9. [nas_qtf_lte_initialize](https://dgps.qualcomm.com/code/s?refs=nas_qtf_lte_initialize) - msg handler registration function for NAS
	10. [rrc_to_mm_sink_e_type](https://dgps.qualcomm.com/code/xref/BF-OG-MPSS.HI.4.1-CHITWAN_MNRMTEFS_TEST/crm/modem_proc/mmcp/test/qmi/inc/mmtask_v.h#242);
2. Some Interesting files
	1. modem_proc/mmcp/nas/mm/src/lte_nas_msg_parse.c
	2. Bhavik Kalyani
3. **Notes**
	1. msg queue handler 
	2. timer expiry handler - [esm_bpm_process_timer_expiry](https://dgps.qualcomm.com/code/s?refs=esm_bpm_process_timer_expiry)
	3. gephi - [[graph visualization]] tool


I am working in CCE environment and i am facing following problem
1. Docker
2. Downloading Ghidra Binary
3. Installing other source code review tools like Source Insight
4. Ubuntu registry  packages

## Silly Questions
1. Debugging technique
	1. inspect callback on address `inspect.b('instruction', instruction=0x11, when=AFTER, callback_func)`
	2. health_analysis/debug_path.py - path printing in defi
	3. list(state.history.bbl_addrs)
2. Symbolic variable who's length we don't know
	1. you can't do that, you have to specify the size.
	2. i case you don't know you go by default with pointer size of the architecture.
3. what is exploration technique? is it like a data pipeline
	1. exploration technique is important.

## Existing Bugs

- https://orbit/CR/2714612 - MN_get_cp_user_data
- https://orbit/CR/2878135
- https://orbit/CR/2119741
- https://orbit/CR/2387351
- https://orbit/CR/2358404
- https://orbit/cr/2557854
- 

## Entry points

1. **MN_get_MNCC_SETUP_IND** - 0x942b500
1. 0x91ee9a0
1. **MN_get_cp_user_data**
2. decode_dl_generic_nas_transport - msg_lib_decode_emm.c
## Communication Task

### Challenge 1

I need you help regarding a project I am working on, just to give you a little bit of context I am trying to run static analysis tool (Ozymandias) on Modem binary. Ozymandias (Ozy) its an internal tool created by VD Team. For the starter I am focusing on running the tool on NAS layer. In light of this effort I meet with some challenges for which I need your help the issues are as follows:
1. Since we are dealing with Modem code everything will be in CCE environment which put lots of restriction on downloading tools in the Virtual machine.
2. Different medium of downloads are block like:
	1. All HTTP/S connections are blocked which is not allowing me to download Ghidra. Is there a way download tools via HTTP/s?
	2. Installing python packages via pip are also block because of the above reason (HTTP). I wanted to install [[Frida DBI]] to trace fuzzing test cases execution. To understand important functions.
	3. Main Docker registry is also block do to which I am not able to setup Ozy's working environment.
	4. Ubuntu package registery
3. Using Source code review tools in CCE environment. Currently I am using Source Insight which a great tool is there something equivalent tool in CCE Environment which will achieve the same objective.
4. Can we Export the compiled Binary out of CCE environment. So that I can using some tools like DBI, disassembler,etc on NON-CCE environment.
5. To run Ozy we need very powerful machine(40 cores 50 GB RAM) do we have this machine in CCE environment? OR can I export the compiled binary to Non-CCE environment where we have such powerful machine to run such computation.

### Challenge 2

- Modem has lots of state machine for different sub-layers
- Different Sub-layer communicate with each other using Queue Mechanism (msgr), there is also legacy message API's but msgr is the latest. On top of that each layer expect other layer to be in certain state and sends and receives information from them.
- Symbolic Execution will fail due to following reason
	- Queue/De-Queue mechanism 
	- State initialization
	- Multi-threading
- The only way to make symbolic execution work is with pre-processing function with setup's the state. A much simpler way would be to execute program and then do symbolic stuff which is canonical execution.
- Target one protocol and try to create PoC like for eg SMS.
- Reading through test cases i have concluded that there are test cases on state transition which include queue/de-queue.
- Test cases are tested using the msgr framework.
- Testing code has lots of stubs functions which one layer wants to get it from other layer.

### Challenge 3

1. Modem communication software is based on [[Qurt]] OS. Full system emulation of the Modem with help us to do better integrated testing for fuzzing or doing dynamic analysis of the modem code.
2. Doing full system emulation with Qemu will help us to do [[Snapshot Fuzzing using QEMU]]. 
3. Currently are we able to run Qurt in Qemu full system emulation.


## TZ Apps Bugs Vetting

## App1

A: securemm.elf, D: 2021-12-31, T: 13, N: 10000, P: 841, V: 30, C: 30, r: 0, E: No, L: /local/mnt2/nredini//.ozymandias/securemm.elf_fd49efc99a93aae83db06cfb85392274/run_dump.log, H: fd49efc99a93aae83db06cfb85392274, m: TZ.APPS.2.0-00086-SAIPANAAAAANAZT-1.465947.1

**Hash ID** : fd49efc99a93aae83db06cfb85392274

### Bug 1

**Alert 56996 (0 similar alerts)**
* Binary: securemm.elf
* Binary hash: fd49efc99a93aae83db06cfb85392274
* Type: overflow (rmp_initialize+0x80)
* Bug score: 0.773781
* Hash: 0a46ef169460b6e6c89df52a3946427c
* Details: Possible out-of-bound write due to missing upper bound check.  Buffer size was determined at code location 0x400430. Buffer: <BV32 taint_value_size_of_x2__699_64[31:0]>
* Verified: No
* Full path: 0x4003ac (rmp_cmd_handler) -> 0x4003e4 -> 0x400400 -> 0x400410 -> 0x400428 -> 0x400430 -> 0x4004b8 -> 0x40bbd8 (memscpy) -> 0x4004d4 (rmp_cmd_handler) -> 0x4004dc -> 0x400000 (text_section_base) -> 0x4004f4 (rmp_cmd_handler) -> 0x400000 (text_section_base) -> 0x400508 (rmp_cmd_handler) -> 0x400538 (rmp_initialize) -> 0x400000 (text_section_base) -> 0x4005a4 (rmp_initialize) -> 0x400020 -> 0x4005b4 -> 0x4005b8

cmd and rsp functions are passed to **rmp_initialize** function but len parameter is not passed along with it.

### Bug 2
** Alert 56995 (0 similar alerts) **
* Binary: securemm.elf
* Binary hash: fd49efc99a93aae83db06cfb85392274
* Type: null pointer dereference (rmp_allocate_kl_index+0x44)
* Bug score: 0.735092
* Hash: c1046c77d9603604043b7e610e44f5ed
* Details: None
* Verified: No
* Full path: 0x4003ac (rmp_cmd_handler) -> 0x4003e4 -> 0x400400 -> 0x400410 -> 0x400428 -> 0x400430 -> 0x4004b8 -> 0x40bbd8 (memscpy) -> 0x4004d4 (rmp_cmd_handler) -> 0x4004dc -> 0x400000 (text_section_base) -> 0x4004f4 (rmp_cmd_handler) -> 0x400000 (text_section_base) -> 0x400508 (rmp_cmd_handler) -> 0x400de8 (rmp_allocate_kl_index) -> 0x400000 (text_section_base) -> 0x400e10 (rmp_allocate_kl_index) -> 0x400e24 -> 0x400e2c

### Bug 3

*** Alert 56997 (1 similar alerts) ***

* Binary: securemm.elf
* Binary hash: fd49efc99a93aae83db06cfb85392274

* Type: overflow (rmp_initialize+0x88)
* Bug score: 0.630249

* Hash: 3b988600ee50486bdfb684dd42def7f4
* Details: Possible out-of-bound write due to missing upper bound check.  Buffer size was determined a

### Bug 4

** Alert 56980 (1 similar alerts) **
* Binary: securemm.elf
* Binary hash: fd49efc99a93aae83db06cfb85392274
* Type: overflow (SDMX_get_dbg_counters+0x70)
* Bug score: 0.361
* Hash: fac1aa4fcc719e1491a5ec82c9d33067
* Details: Found a fully tainted condition concerning buffer length!
* Verified: No
* Full path: 0x4046c0 (sdmx_cmd_handler) -> 0x404700 -> 0x404734 -> 0x404780 -> 0x404ad8 (sdmx_log) -> 0x404798 (sdmx_cmd_handler) -> 0x400180 -> 0x40479c -> 0x40837c (SDMX_get_dbg_counters) -> 0x4083c0 -> 0x4083ec

`((sizeof(sdmx_get_counters_rsp_t) + pSdmxRsp->u32NumFilters*sizeof(sdmx_filter_dbg_counters_t)) > rsplen))` this can create integer overflow

## App2

A: sp_license.elf, D: 2022-01-07, T: 65, N: 7, P: 8004, V: 3, C: 3, r: 0, E: No, L: /local/mnt2/nredini//.ozymandias/sp_license.elf_22f92991bc6e9af866e776536a12e866/run_dump.log, H: 22f92991bc6e9af866e776536a12e866, m: TZ.APPS.2.0-00170-SPF.LAHAINA-1.466421.1

Allmost all of them are null pointer 

## App3
14) A: fipstestapp64.elf, D: 2022-01-08, T: 179, N: 10000, P: 4365, V: 41, C: 41, r: 1, E: No, L: /local/mnt2/nredini//.ozymandias/fipstestapp64.elf_17892ebd0a178a8f12c65599d76242ac/run_dump.log, H: 17892ebd0a178a8f12c65599d76242ac, m: TZ.APPS.2.0-00086-SAIPANAAAAANAZT-1.465947.1


