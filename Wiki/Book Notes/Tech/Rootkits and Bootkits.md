---
up: "[[Reading MoC]]"
tags:
  - "#type/reading/book"
  - "#knowledge/firmware-security"
  - "#knowledge/uefi-security"
aliases:
  - UEFI Security
  - Boot Security
  - Firmware Security
status: reading
created-date: 2024-12-11
title: Rootkits and Bootkits - Reversing Modern Malware and Next Generation Threats
---

**Author**:: Alex Matrosov, Eugene Rodionov, and Sergey Bratus

## Reference Notes

1. Intel Software Guard Extension (SGX)
2. **Intel BIOS Guard** - BIOS Guard moved the task of updating the contents of the SPI flash from SMM to a separate chip (the Intel Embedded Controller, or EC) and removed the permissions that allowed the SMM to write to the SPI flash.
	1. SMM was originally allowed both read and write access to SPI flash storage as a means of implementing routine BIOS updates. This meant the integrity of the BIOS was dependent on the code quality of any code running in the SMM,
	2. This made the integrity of the BIOS dependent on the quality of any code running in the SMM with calls to outside memory regions
	3. BIOS Control Bit Protection (BIOS_CNTL), which is effective only against an attacker attempting to modify the BIOS from the operating system;
	4. The the SPI flash Write Protection (PRx) control is more effective because its policies can’t be changed from the SMM. Many vendors don’t use PRx protections—including Apple and, surprisingly, Intel, the inventor of this protection technology
		1. The reason vendors like Apple and Intel tend to disable PRx protections is that these protections require an immediate reboot, making updating the BIOS less convenient.
		2. PRx doesn’t guarantee any type of integrity measurements on the actual contents of SPI, as it only implements bit-based locking of direct read/write access in the very early PEI stage of the boot process
	5. platform developers took steps to separate BIOS updates from the rest of the SMM functionality, introducing a series of additional security controls, such as Intel BIOS Guard. 
	6. Known vulnerability
		1. CVE-2017–11313 and CVE-2017–11314
		2. Speed Racer vulnerability (VU#766164, originally discovered by Corey Kallenberg in 2014) to bypass SPI flash protection bits via a race condition.
	7. BIOS Lock Bit (BLE) and the SMM BIOS Write Protection Bit (SMM_BWP)
	8. No matter which route the BIOS updater takes, it’s critical that the helper code authenticate the update image before installing it
3. Intel Boot Guard - moved the root of trust from SPI into hardware
	1. ![[intel-boot-guard-tech.png]]
	2. Boot Guard divides Secure Boot into two phases: in the first phase, Boot Guard authenticates everything located in the BIOS section of the SPI storage, and in the second stage, Secure Boot handles the rest of the boot process, including authentication of the OS bootloader.
	3. The Intel Boot Guard technology spans several levels of the CPU architecture and the related abstractions. One benefit is that it doesn't need to trust the SPI storage.
	4. Boot Guard separates integrity checking of the BIOS stored in the SPI flash from the BIOS itself by using the Authenticated Code Module (ACM), which is signed by Intel, to verify the integrity of the BIOS image before allowing it to execute.
	5. Root of trust moves inside the Intel microarchitecture, wherein the CPU’s microcode parses the ACM contents and checks the digital signature verification routines implemented in the ACM, which in turn will check the BIOS signature.
	6.  Verified Boot - a recent variant of Boot Guard that Intel introduced in 2013, which we’ll discuss in more detail in the next chapter—locks the hash of an OEM public key within the field programmable fuse (FPF) store
	7. Even the Measured Boot scheme, which relies on the TPM as its root of trust, can be compromised, because the measuring code itself runs in SMM and can in many cases be modified from the SMM, even though the key stored in the TPM hardware cannot be changed by SMM. Although some attacks on the TPM chip are possible, the SMM privilege–wielding attackers do not need them, as they would simply attack the firmware’s interfaces to the TPM. In 2013 Intel introduced Verified Boot, which we just mentioned, to address this Measured Boot weakness.
4. All the researched Intel-based hardware had CPU debugging enabled, so all the doors were open to attackers with physical access to the CPU. Some of the platforms included support for the Intel BIOS Guard technology, but it was disabled in the manufacturing process to simplify BIOS updates.
5. What Makes Firmware Vulnerable?
	1. Vendors will typically describe UEFI firmware updates broadly as BIOS updates, because the BIOS is the main firmware included, but a typical update also delivers many other kinds of embedded firmware to the various hardware units inside the motherboard, or even the CPU.
6. System Management Mode (SMM)
	1. SMM is a highly privileged execution mode of x86 processors. It was designed to implement platform-specific management functions independently of the OS. These functions include advanced power management, secure firmware updates, and configuration of UEFI Secure Boot variables.
	2. The key design feature of SMM is that it provides a separate execution environment, invisible to the OS. The code and data used in SMM are stored in a hardware-protected memory region, called SMRAM, that is accessible only to code running within SMM. To enter SMM, the CPU generates a System Management Interrupt (SMI), a special interrupt intended to be raised by the OS software.
	3. SMI handlers are the platform firmware’s privileged services and functions. The SMI serves as a bridge between the OS and these SMI handlers. Once all the necessary code and data have been loaded in SMRAM, the firmware locks the memory region so that it can be accessed only by code running in SMM, preventing the OS from accessing it.
	4. Attack
		1. the best way to attack the privileged code is to target any data that can be consumed from outside the isolated privileged memory region.
		2. Potential targets include function pointers consumed by the SMM code that can point execution to areas outside SMRAM or any buffers with data that SMM code reads/parses.
	5. Defense
		1. UEFI firmware developers try to reduce this attack surface by minimizing the number of SMI handlers communicating directly with the outside world (Ring 0—the kernel mode of the operating system), as well as by finding new ways to structure and check these interactions.
		2. the SMM code must never act on the outside data unless it’s been copied and checked inside the SMRAM. the attacker could potentially race to change it between the point of check and the point of use.
			1. Classic cross-privilege level attack
	6. In SMI callout issues, SMM code unwittingly uses a function pointer, controlled by the attacker, that points at an implant payload outside the SMM. In arbitrary code execution, SMM code consumes some data from outside SMRAM that is capable of affecting the control flow and can be leveraged for more control.
	7. The SmmIsBufferOutsideSmmValid() function accurately detects pointers to memory buffers outside the SMRAM range, with one exception: it’s possible for the Buffer argument to be a structure and for one of the fields of this structure to be a pointer to another buffer outside SMRAM. If the security check happens only for the address of the structure itself, SMM code may still be vulnerable, despite a check with SmmIsBufferOutsideSmmValid(). Thus, SMI handlers have to validate each address or pointer—including offsets!—that they receive from the OS prior to reading from or writing to such memory locations. Importantly, this includes returning status and error codes. Any type of arithmetic calculation that happens inside SMM should validate any parameters coming from outside of SMM or less privileged modes.

### Intel Management Engine (ME)

1. ME uses a separate x86-based CPU (in the past, it used the boutique ARC CPU) and serves as the foundation for the Intel hardware root of trust and multiple security technologies such as Intel Boot Guard, Intel BIOS Guard, and, partially, Intel Software Guard Extension (SGX)
2. How is works?
	1. Intel created a special interface, called the *Host-Embedded Controller Interface* (HECI), so ME applications could communicate with the operating system kernel.
	2. UEFI firmware initializes HECI via a proxy SMM driver, *HeciInitDxe*, located inside the BIOS. This SMM driver passes messages between ME and the host OS vendor-specific driver over the PCH bridge, which connects the CPU and the ME chip.
	3. Applications running inside the ME can register HECI handlers to accept communication from the host operating system.
3. Security Implication
	1. If the OS kernel is taken over by an attacker, these interfaces become a part of the ME's attack surface; for example, an overly trusting parser inside an ME application that does not fully validate messages coming from the OS side could be compromised by a crafted message, just as weak network servers are. 
	2. It's important to reduce the attack surface for ME applications by minimizing the number of HECI handlers. Indeed, Apple platforms permanently disable the HECI interfaces.

### UEFI firmware vulnerabilities (chapter 16)

![[uefi-vulnerability-classification.png]]

2. according to the potential impact of an attack on that vulnerability. They presented their classifications at 
3. Black Hat USA 2017 in Las Vegas in their talk [Firmware Is the New Black—Analyzing Past Three Years of BIOS/UEFI Security Vulnerabilities](https://www.youtube.com/watch?v=SeZO5AYsBCw)
4. OOB Management with Intel Active Management Engine
5. *How to Hack a Turned-Off Computer, or Running Unsigned Code in Intel Management Engine* at Black Hat Europe 2017
	1. CVE-2017-5705, CVE-2017-5706, and CVE-2017-5707
6. CVE-2017-5689 (INTEL-SA-00075) - which allowed for remote access and authentication bypass.
7. Scope of vulnerability
	1. communicate with a platform over a network even when the main CPU is not active or is completely powered down. 
	2. It also uses the ME to read and write DRAM at runtime, independently of the main CPU.
	3. the system could also be attacked when turned off.
8. Vulnerability Classification
	1. **Misconfigured protections** By attacking the hardware or firmware during the development process or post-production stage, an attacker can misconfigure technology protections to allow them to be bypassed easily later.
		1.  NIST 800-147 (“BIOS Protection Guidelines”) and NIST 800-147B (“BIOS Protection Guidelines for Servers”) and becoming outdated since their initial release in 2011 and update for servers in 2014.
	2. **Nonsecure root of trust** This vulnerability involves compromising the root of trust from the operating system via its communication interfaces with firmware
	3. **Unauthenticated BIOS update process** Vendors may break the authentication process for BIOS updates, whether intentionally or not, allowing attackers to apply any modifications they want to the update images.
	4. **Outdated BIOS with known security issues** BIOS developers might continue to use older, vulnerable code versions of BIOS firmware, even after the underlying codebase has been patched, which makes the firmware vulnerable to attack. An outdated version of the BIOS originally delivered by the hardware vendor is likely to persist, without updates, on the users’ PCs or data center servers. This is one of the most common security failures involving BIOS firmware.

### S3 Boot script

1. Attack Plan
	1. Attacker must get the S3 boot script pointer (address) from the UEFI variable AcpiGlobalVariable, which points to the boot script location in unprotected DRAM memory.
	2. the attacker inserts a malicious dispatch opcode record at the top of the copied boot script to place as the first boot script opcode command. They then overwrite the boot script address location by setting the AcpiGlobalVariable to a pointer to the modified malicious version of the boot script.
	3. S3 boot script dispatch code (EFI_BOOT_SCRIPT_DISPATCH_OPCODE) should now point to the malicious shellcode.
	4. The malicious boot script is executed right after the attacked machine returns from sleep mode.
2. This vulnerability and its exploit are also useful for disabling some of the BIOS protection bits, such as BIOS Lock Enabled, BIOS Write Protection, and some others configured.
3. Mitigating the S3 boot script issue required integrity protection from Ring 0 modifications. One way to achieve this was to move the S3 boot script to the SMRAM (SMM memory range).
4. Intel architects designed a LockBox mechanism to protect the S3 boot script from any modifications outside of SMM.

### Intel chip Firmware

**Power Management Unit (PMU)** A microcontroller that controls the power functions and transitions of a PC between different power states, such as sleep and hibernate. It contains its own firmware and a low-power processor.

**Intel Embedded Controller (EC)** A microcontroller that is always on. It supports multiple features, such as turning the computer on and off, processing signals from the keyboard, calculating thermal measurements, and controlling the fan. It communicates with the main CPU over ACPI, SMBus, or shared memory. The EC, along with the Intel Management Engine described shortly, can function as a security root of trust when the System Management Mode is compromised. The Intel BIOS Guard technology (vendor-specific implementations), for example, uses the EC to control the read/write access to SPI flash.

**Intel Integrated Sensor Hub (ISH)** A microcontroller responsible for sensors, such as device rotation detectors and automatic backlight adjustors. It can also be responsible for some low-power sleep states for those sensors.

**Graphics Processing Unit (GPU)** An integrated graphics processor (iGPU) that is part of the Platform Controller Hub (PCH) design in most modern Intel x86-based computers. GPUs have their own advanced firmware and computing units focused on generating graphics, such as shaders.

**Intel Gigabit Network** Intel-integrated ethernet network cards for x86-based computers are represented as PCIe devices connected to PCH and contain their own firmware, delivered via BIOS update images.

**Intel CPU Microcode** The CPU’s internal firmware, which is the interpretive layer that interprets the ISA. The programmer-visible _instruction set architecture (ISA)_ is a part of microcode, but some instructions can be more deeply integrated on the hardware level. Intel microcode is a layer of hardware-level instructions that implement higher-level machine code instructions and the internal state machine sequencing in many digital processing elements.

**Authenticated Code Module (ACM)** A signed binary blob executed in cache memory. Intel microcode loads and executes within protected internal CPU memory, which is called _Authenticated Code RAM (ACRAM)_, or _Cache-as-RAM (CAR)_. This fast memory is initialized early in the boot process. It functions as regular RAM before the main RAM is activated and before the reset-vector code for early boot ACM code (Intel Boot Guard) runs; it can also be loaded later in the boot process. Later, it is repurposed for general-purpose caching. The ACM is signed by an RSA binary blob with a header that defines its entry point. Modern Intel computers can have multiple ACMs for different purposes, but they are mostly used to support additional platform security features.

**Intel Management Engine (ME)** A microcontroller that provides the root-of-trust functionality for multiple security features developed by Intel, including the software interface to the _firmware Trusted Platform Module_, or _fTPM_ (usually the TPM is a specialized chip on an endpoint device for hardware-based authentication that also contains separate firmware of its own). Since the sixth generation of the Intel CPU, the Intel ME is an x86-based microcontroller.

**Intel Active Management Technology (AMT)** The hardware and firmware platform used for managing personal computers and servers remotely. It provides remote access to monitors, keyboards, and other devices. It comprises Intel’s chipset-based Baseboard Management Controller technology for client-oriented platforms (discussed next), integrated into Intel’s ME.

**Baseboard Management Controller (BMC)** A set of computer interface specifications for an autonomous computer subsystem that provides management and monitoring capabilities independently of the host system’s CPU, UEFI firmware, and real-time operating system. The BMC is usually implemented on a separate chip with its own ethernet network interface and firmware.

**System Management Controller (SMC)** A microcontroller on the logic board that controls the power functions and sensors. It’s most commonly found in computers produced by Apple.

## Logs

```dataview
TASK
WHERE icontains(text, this.file.name) AND 
	  !(icontains(text, "#task") OR icontains(text, "#question"))
	AND !completed
GROUP BY file.name as Filename
SORT fows.file.ctime DESC
LIMIT 20
```