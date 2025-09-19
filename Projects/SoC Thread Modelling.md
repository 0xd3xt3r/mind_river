
## Threats
1. Malware Persistence
2. Kernel can run user provided drivers
	1. Since its a laptop its more of developer environment so vendors might allow to you custom driver
	2. This will open UP all the other IP open for attack
	3. QDSS/ARM Core Sight sub-system can also be open for use
3. Why is Boot Process Security Important
	1. Many of the security feature which are provided by OS Kernel is dependent on the integrity of the Kernel image. Feature like loading signed driver and firmware update mechanism etc assume the Kernel image is not tempered.
		1. If the kernel image is tempered then attackers can patch the kernel routines which are doing signature checks and simply return true even if the check fails.
		2. To ensure this integrity this trust is placed on OS loader which had the digital signature data to check the integrity of Kernel it is loading.
		3. Now getting the OS operation system fully running is complicated process and there are whole lot of things are happening like hardware initialization, etc. OS Loader itself is loading by UEFI firmware which is loaded by BIOS which is loading by Boot ROM, you see there is a chain of dependencies almost like reverse domino effect. So if any of the piece is compromised it will be leveraged by the attack to any of the piece higher in the hierarchy.
		4. Also the complexity of this process means the will be opportunity for Implementation error which can be target of exploitation.
	2. The boot process can be influenced by the User/Malware by changing various configuration security boot process can be compromised by malware using exploit that targets SMM which has the ability to update BIOS and UEFI firmware images residing in SPI Flash.
		1. So the threat is not only at physical level where someone is flashing Malicious BIOS but remote attack which tack control of the system can do this as well.
4. What are the consequences of compromising boot security
5. How does the device deals with boot process failure (policy)
6. Safe Device disposal 
	1. End-of-Life program through our Asset Recovery Services (ARS). Through ARS, Lenovo can securely wipe hard drives and securely recycle parts utilizing industry recognized data sanitization standards such as NIST SP 800-88 R1 and Commission Regulation (EU) 2019/424, compliant with privacy laws such as HIPAA, GDPR, CCPA, Sarbanes Oxley, Gramm-Leach-Bliley, and others.


## Intel

### Components

1. Verified and Measure Boot
	1. Hardware root of trust.