---
tags:
  - "#qpsi/project"
  - variant-analysis
  - "#type/sub-project"
  - "#qpsi/ar25"
status: done
created-date: 2024-07-17
completed-date: 2024-07-17
up: "[[Android Kernel Driver Fuzzing]]"
summary: Incident Response ticket analysis
---



## Objective

## Notes

## IRVR Part 2

| **Driver** | **Jira** **Ticket**                                                                                                                                                                                  | **Related CRs**                                                                                                               | **Security Engineer** | Sub-system | Comments                                                                                              |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- | --------------------- | ---------- | ----------------------------------------------------------------------------------------------------- |
| camera     | [QPSIVR-2284](https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2284?filter=allopenissues "https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2284?filter=allopenissues") | [4178811](https://orbit/CR/4178811 "https://orbit/CR/4178811")                                                                | Munawwar              | JPEG       | Windows Target                                                                                        |
| camera     | [QPSIVR-2193](https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2193?filter=allopenissues "https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2193?filter=allopenissues") | [3992706](https://orbit/CR/3992706 "https://orbit/CR/3992706") [3992803](https://orbit/CR/3992803 "https://orbit/CR/3992803") | Munawwar              |            | Windows Target                                                                                        |
| Camera     | [QPSIVR-2310](https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2310?filter=allopenissue "https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2310?filter=allopenissue")   | [4216838](https://orbit/CR/4216838 "https://orbit/CR/4216838")                                                                | Munawwar              |            | TOUTOC in ICP                                                                                          |
| camera     | [QPSIVR-2285](https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2285?filter=allopenissues "https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2285?filter=allopenissues") | [4187743](https://orbit/CR/4187743 "https://orbit/CR/4187743")                                                                | Munawwar              |            | Windows Target                                                                                        |
| camera     | [QPSIVR-2337](https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2337?filter=allopenissues "https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2337?filter=allopenissues") | -                                                                                                                             | Munawwar              |            | TOUTOC in Flash                                                                                       |
| Camera     | [QPSIVR-2223](https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2223?filter=allopenissues "https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2223?filter=allopenissues") | [4068644](https://orbit/CR/4068644 "https://orbit/CR/4068644")                                                                | Munawwar              |            | DMA Fence UAF                                                                                         |
| Camera     | [QPSIVR-2288](https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2288?filter=allopenissues "https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2288?filter=allopenissues") | [4207213](https://orbit/CR/4207213 "https://orbit/CR/4207213")                                                                | Munawwar              |            | Windows Target                                                                                        |
| camera     | [QPSIVR-2286](https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2286?filter=allopenissues "https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2286?filter=allopenissues") | [4191010](https://orbit/CR/4191010 "https://orbit/CR/4191010")                                                                | Munawwar              |            | Windows Target                                                                                        |
| camera     | [QPSIVR-2341](https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2341?filter=allopenissues "https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2341?filter=allopenissues") | -                                                                                                                             | Munawwar              | OPE        | This is a medium tier chipset we havn't target this sub-system as part of our campaign. Out of scope. | 
| camera     | [QPSIVR-2342](https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2342?filter=allopenissues "https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2342?filter=allopenissues") | -                                                                                                                             | Munawwar              | OPE        | We are not fuzzing OPE                                                                                |
| camera     | [QPSIVR-2147](https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2147?filter=allopenissues "https://jira-dc.qualcomm.com/jira/projects/QPSIVR/issues/QPSIVR-2147?filter=allopenissues") | [3927984](https://orbit/CR/3927984 "https://orbit/CR/3927984") [3932332](https://orbit/CR/3932332 "https://orbit/CR/3932332") | Munawwar              |            | Already analyzed this in the past                                                                     |

## IRVR Tickets

- [ ] QPSIVR-2093
- [ ] QPSIVR-2082
- [ ] QPSIVR-2080
- [ ] QPSIVR-2077
- [ ] QPSIVR-2075
- [ ] QPSIVR-2074
- [ ] QPSIVR-2073
- [x] QPSIVR-2072
	- de-sync between buffer length very similar to other de-sync
- [x] QPSIVR-2071
	- difficult to analyze need more understand of the code base
- [x] QPSIVR-2070
	- buffer offset are stored in list and the offset is only verified for one item in the list.
- [x] QPSIVR-2069
	- same as QPSIVR-2061 and dublicate to QPSIVR-2012
- [x] QPSIVR-2068
	- buffer size de-sycn
- [x] QPSIVR-2067
	- buffer size de-sycn
- [x] QPSIVR-2066
	- de-sync betweeen *settings_array_size* and *settings_array_offset*.
		- Buffer pointer validation issue
- [x] QPSIVR-2065
	- here are two length field which are used for validation one is "cmd_header->size" and other is DMA Buffer. This vulnerability exploitions this desyncronization between two field.
- [x] QPSIVR-2064
	- race condition on *krefcount* field in struct which maintains global shared buffered list. The field should be protected by lock.
		- recommendation is part of kernel docs https://www.kernel.org/doc/Documentation/kref.txt
- [x] QPSIVR-2063
	- Shared buffer can be freed while we are using it.
		- very similar to QPSIVR-2062
- [x] QPSIVR-2062
	- TOCTOU vulnerability, This is an old issue and I also discussed with Scott before and he said they working on this to fix it.
- [x] QPSIVR-2061
	- no check on buffer offset
		- same as QPSIVR-2069 and duplicate to QPSIVR-2012
- [x] QPSIVR-2058
	- invalid if condition
		- found very similar bug in before in DSP with critical rating