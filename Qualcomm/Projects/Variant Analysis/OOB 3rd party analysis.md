---
tags:
  - qpsi/project
  - uefi
  - firmware-security
  - variant-analysis
  - "#type/sub-project"
status: in-progress
created-date: 2025-06-27
up: "[[Out of Band System Management]]"
summary: OOB Vulnerability in external vendors
---

## Project Research

### Fleeting Notes

- [Intel](https://www.intel.com/content/www/us/en/business/enterprise-computers/resources/out-of-band-management.html) and AMD supporting this tech since long.
	- Intel has Intel vPro Enterprise for Windows OS
		- Its based on Intel Active Management Technology (Intel AMT)
		- These RMM hardware-based capabilities operate independently of the OS and provide persistent connectivity
		- With remote keyboard, video, and mouse (KVM) control, IT administrators can access and work on remote PCs and devices as if they were *hands on*.
		- **Intel Mesh Commander** and **MeshCentral** for peer-to-peer remote management.
	- AMD : Out-Of-Band PC Management with AMD PRO Manageability
		- Standards DASH - Desktop & Mobile Architecture for System Hardware
		- [DASH](https://www.dmtf.org/standards/dash) is part of DMTF (formerly known as the Distributed Management Task Force).
		- Couple of Solution
			- EPYC System Management Software (E-SMS) [link](https://www.amd.com/en/developer/e-sms.html)
			- Advanced Platform Management Link ([APML](https://www.amd.com/en/developer/e-sms/apml-library.html)) Library
				- [sample open source library](https://github.com/amd/esmi_oob_library)
- Hypervisor breach may fall in high to medium impact but OOB breach will have sever impact
- We need to prioritize it internally at QPSI. That SS is called OOB SS and it should be our priority but some reason it is not.
- Please work with Nagaraju and Initiate this discussion. We can discuss and make case but any new topic has to go through QPSI vetting process. It means Murali, Nicolas and We should be aligned.


## Existing CVE

1. https://nvd.nist.gov/vuln/detail/CVE-2023-34329
2. https://nvd.nist.gov/vuln/detail/CVE-2024-54085

## Vulnerability research reports

1. [Redfish Vulnerability](https://eclypsium.com/blog/ami-megarac-vulnerabilities-bmc-part-3/)
	1. Http spoofing vulnerability.

## Reference

1. [BMC Vulnerability](https://arstechnica.com/security/2025/06/active-exploitation-of-ami-management-tool-imperils-thousands-of-servers/)
2. [UEFI Redfish Service](https://uefi.org/specs/UEFI/2.9_A/31_EFI_Redfish_Service_Support.html)