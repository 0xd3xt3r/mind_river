---
up: "[[Learning MoC]]"
tags:
  - "#learning/course"
created-date: 2025-01-02
status: in-progress
link: https://apps.p.ost2.fyi/learning/course/course-v1:OpenSecurityTraining2+TC1101_IntroTPM+2024_v2/home
title: Trusted Computing 1101 - Introductory Trusted Platform Module (TPM) usage
summary: Introductory Trusted Platform Module
---

### Glossary

1. RoT - Root of Trust
2. TPM - Trusted Platform Module
3. NV (storage) - Non-volatile memory
4. TSS/TCG - TPM Software Stack
5. HSM - Hardware Security Modules
6. Owner - The entity (user) that has administrative rights over the TPM.
7. PCR - Platform Configuration Register
8. Sealing - Sealing is the procedure of integrating a user secret (arbitrary data) as part of a newly generated TPM key. This key can then be kept outside the TPM, external hard drive, usb flash, etc. 
	1. The secret can be read only at the original TPM after proper authorization (password or policy). 
	2. TPMs support one more type of sealing that is called "sealing against PCR".
	3. Decrypting the key is called *unsealing*.
9. MITM - Machine-in-the-middle attack - Attacker listens to and tries to modify the communication between two parties.

## Reference Notes

### What, Why, and How?

1. Why master Trusted Computing? [link](https://www.youtube.com/watch?v=PW7WSXFzeJg)
	1. How to used hardware security to improve security of products. As software based security in not enough.
	2. Website : TPM.dev
	3. TPM 2.0 is the focus of the course
	4. Setting up a TPM 2.0 dev env.
	5. [03:33]() - What is TPM?
	6. [04:26]() - Lab Environment
	7. [06:01]() - Lab Highlights
	8. [06:26]() - Software Stack
		1. tpm2-tss - Microsoft, Linux 
		2. ibmtss
		3. [wolfTPM](https://github.com/wolfSSL/WolfTPM) - embedded systems
	9. [08:34]() - What is Root-of-Trust?
	10. [11:55]() - Which hardware support RoT?
	11. [13:15]() - RoT for Storage
		1. RoT of Reporting
			1. used for proof of bandwidth counter on network router
			2. proof of milage counter on car
		2. Internal flag of compromise or an attempt of attack, dictionary attack
2. When to use a TPM? [link](https://www.youtube.com/watch?v=J9iWyPIY6Lw)
	1. [00:10]() - When not to use a TPM.
		1. [00:47]() - **Its not a Cryptographic accelerator.**
			1. wolfTPM Performance consideration
			2. infineon [SLB 9672](https://www.infineon.com/cms/en/product/security-smart-card-solutions/optiga-embedded-security-solutions/optiga-tpm/optiga-tpm-slb-9672-fw15/), 
				1. SLB 9670.
				2. OPTIGA TPM SLB 9672 RaspberryPi *Evaluation Board* - SPI TPM HAT
				3. [Product video](https://www.youtube.com/watch?v=rMHhkAv9Go8) 
			4. STMicro ST33TP
			5. Microchip ATTPM20
			6. Nuvoton NPCT650, NPCT750
		2. [05:28]() - **Its not common storage.**
			1. People start storing everything in TPM memory and the memory is very limited
			2. More memory it will be costly
			3. It should be only be used for critical data.
			4. STMicro ST33 TPM2.0 - 3 4 year old and very mature product. NVRAM storage of 112 kB
		3. [08:17]() - **TPM is not a standalone solution**
			1. We need a host device which uses the TPM Module to send commands. TPM is more of a service provider.
			2. TPMs are a passive devices.
			3. Active device means its making some decision over a HASH or signature.
	2. [10:30]() - Summary of how not to use TPM.
	3. [12:44]() - Why to use a TPM?
		1. Better Physical tampering protection.
		2. Protection against MITM(Machine in the Middle)
	4. [14:23]() - Why to choose a TPM?
		1. Multiple TPM vendors and up-to-date library.
		2. You can simply switch TPM chip if you want more memory and software library will still work.
	5. [16:33]() - Software libraries for using a TPM
	6. [18:23]() - Use cases for TPM's Secure Storage
		1. Signing Operations
		2. Sealing - Arbitrary data, files or State of system
		3. Keys - Establish connection of TLS
		4. Storing sensitive data - trick various important every occurred with the device.
			1. Taxi car/Truck is supplied with device which stores mileage counter.
			2. Opening a door when it should be closed
			3. Medical device temperature while shipping
	7. [21:10]() - Secure Storage - *This part seems to be repeating with previous section*
		1. NVRAM as a secure counter
	8. [22:56]() - RoT for Reporting
		1. Remote attestation
		2. Key attestation
		3. Proof of the system uptime
		4. Third largest automated Car washing company integrate TPM into their systems
	9. [25:07]() - Recap
		1. Compared to other HSM Solution TPM is very affordable.
3. TPM types, and comparison to Secure Elements & Trust Zone - [link](https://www.youtube.com/watch?v=51I9VpkOrNU)
	1. [00:21]() - Different type of TPM
	2. [00:54]() - Discrete TPM - Physical TPM chip
		1. Platform Configuration Registers (PCR)
		2. Cryptographic Processor
			1. RNG
			2. Key generator
			3. Hashing
			4. Digital Signature generation
	3. [03:25]() - Components of TPM1.2 (deprecated)
		1. Cryptographic Processor / Execution Enginer
		2. Security Enablement - Key Gen, Hash, RNG, Platform Key
		3. Storage
			1. Transient Memory / Versatile Memory - Temp data
			2. Persistent Memory
		4. [06:57]() - TPM2.0
	4. [07:19]() - [[Integrated TPM]] / [[Firmware TPM]]
		1. Custom Silicon chip which is part of SoC .
		2. It run on the same Same CPU
		3. A machine might have two TPC, one core is general CPU and other core is [[TPM core]].
		4. [10:10]() - [[Intel]] example
		5. [10:53]() - [[AMD]] Example
		6. [11:35]() - Typical Example [[Cortex-A5]] SoC
		7. [12:03]() - [[Intel vPro]] Chipsets TPM, example of ASIC
		8. [12:34]() - [[AMD PSP]] technology
	5. [14:16]() - Firmware TPM - TPM in TEE, cost effective as there is not special hardware require.
	6. [15:42]() - Software TPM - good for learning
	7. [17:15]() - Virtual TPM/ vTPM - used to protect virtual machine.
		1. ASW Nitro
		2. VMWare vSphere
		3. Google vTPM with Shielded VM
		4. Security Guarantee is a modrate since its based on software.
	8. [19:58]() - What is a Secure Element?
		1. Another type of HSM
		2. Example - SIM, Banking cards like debit card, credit card.
		3. Embedded secure element like NXP EdgeLock SE050 and Infineon OPTIGA Trust M.
	9. [21:51]() - Secure Element(SE) vs TPM 2.0
		1. TPM is build for security, crypto acceleration is not done to protect again attacks.
		2. Both offer secure storage.
		3. SE doesn't provide crypto-exec and offload it to main processor.
		4. SE some level of tamper-protection and can be less expensive then TPM with less feature and less security.
	10. [24:38]() - What is ARM's TrustZone.
		1. Its a capability and not a complete solution
		2. Gives isolation and separation.
		3. Can be used as TPM and can be said as Firmware TPM
4. How to setup Lab Environment
	1. https://github.com/tpm2-software/tpm2-tss
	2. https://github.com/tpm2-software/tpm2-tools

### TPM Core Concepts

1. TPM Keys : [link](https://www.youtube.com/watch?v=C62yKrpalEA)
	1. [00:18]() - Key Types
		1. Private key material never leaves the TPM in plain form
		2. **Primary Key** - private material never level TPM, we can only regenerate them with seed + user provided entropy.
			1. It is used to encrypt and protect Child Key, it is called wrapping. Primary key is wrapping the child key.
		3. **Child Key** - are actually used to perform cryptographic operation like signing, encryption, etc.
			1. To load/use the child key we need to have primary key authorisation to decrypt/unwrap the child key in side the TPM.
	2. [02:15]() - TPM Hierarchies
		1. Different seed/hierarchies have special purpose
		2. Platform - OEM put different objects during manufacturing
			1. OEM -> system builder, integrator
			2. platform authorization is randomized at system boot done by first OEM software like UEFI, Boot, etc.
		3. Endorsement - this hierarchy is used by TPM Vendor
			1. primary key is created using general seed of the TPM and has properties defined by the TCG.
			2. This doesn't have the capability to generate signature keys that by specification
		4. Owner - This is generally used by end user.
			1. Its can have as many hierarchies as we want.
			2. Each primary key has its own authorization.
		5. NULL - Random seed at every power-on cycle(not sleep)
			1. usually used by one time/temporary keys.
	3. [06:07]() - Primary and child keys
		1. primary are only used to wrap the child key although they can do more but not recommended.
		2. key is made up of => Type + Properties/Attributes + Parameter(size) + unique template(user provided entropy).
			1. TPM2B_PUBLIC structures
	4. [07:50]() - Key Properties
		1. TPMA_OBJECT Bits
		2. Sign Property - to generate signature
		3. \_DECRYPT - key used for decryption
		4. Fix TPM - cannot transfer between TPM. Only one TPM can used it.
	5. [10:48]() - Create a Primary key
		1. `tpm2_createprimary -C o` -> create primary key in TPM owner hierarchy.
		2. [11:42]() - `-p |--key-auth` authorization in owner is usually missing. but is recommended to have password.
			1. `-G | --key-algorithm` key algorithm rsa2048, ecc
			2. `-c | --key-context` context file to save
		3. [12:56]() - `-u|--unique-data` provide additional user controlled entropy to the KDF.
		4. Example commands
			1. `tpm2_createprimary -c primary.ctx -G ecc256`
			2. `tpm2_createprimary -C o -c primary.ctx -G rsa2048`
		5. saving the public portion of any tpm key `-o | --output <key>` and the format `-f <tss/psm/der/tpmt>` recommended *pem/der*. 
			1. this also make it compatible with different cryptographic library.
	6. [14:50]() - Create a child key
		1. take primary key as input `-C primary.ctx`
		2. `-c key.ctx` - child context which will be result of the operation
			1. context file will have both private and public part. Private part is encrypted and protected before leaving the TPM and embedded in the context file.
			2. [15:36]() - you can store the private and public part separately. `-u key.pub` and `-r key.priv` private
			3. [15:57]() - Example
				1. `tpm2_create -C primary.ctx -G ecc256:ecdsa-sha12 -c key.ctx`
			4. Load the existing key in the TPM with `tpm2_load -C primary.ctx -u key.pub -r key.priv -c key.ctx`
				1. there is `tpm2_createandload` command to do create and load at the same time.
	7. [17:59]() - Lab workflow.
		1. Flush TPM object to remote TPM key from the key slot because it only has limited slots.
		2. also it has limited persistence key which are also limited.
		3. `tpm2_flushcontext` to remove specific handle or all related objects.
			1. `-t` loaded session or `-l` saved session.
		4. `tpm2_flushcontext -tls` flush everything
	8.  [18:53]() - Sealing secrets
		1. `-i | --sealing-input` data to be sealed.
		2. `tpm2_create -C primary.ctx -i seal.data -c seal.ctx`
		3. `tpm2_unseal -c seal.ctx`
		4. we can also seal data with PCR register
	9. [20:32]() - Digital Signature
		1. Generate signature - `-o | --signature` - signature file (output)
			1. `tpm2_sign -c rsa.ctx -g sha256 -o sig.rssa message.data`
		2. Verify the signature - `-s | --signature` input, `-m | --message` the message file.
			1. `tmp2_verifysignature -c rsa.ctx -g sha256 -s sig.rssa -m message.data`
2. Using the TPM as Random generator, HMAC and hashing tool : [link](https://www.youtube.com/watch?v=fDCiP0Gs0go)
	1. [00:10]() - Randomness
		1. [01:38]() - `tpm2_getrandom -o <output file>`
			1. max random number in one exec you can get largest supported hash digest.
			2. now a days TPM support SHA-512 and maximum you can get is 64 bytes.
	2. [03:13]() - What is hashing
		1. [04:05]() - Command `tpm2_hash`
			1. `tpm2_hash -g <algo> --hex -o <output file`
			2. `tpm2_hash -g sha256 -o hash.bin data.txt`
	3. [05:15]() - What is HMAC, message is not encrypted
		1. Command `tpm2_hmac`
		2. You need the primary/child "key" of type `-G hmac`
		3. Example `tpm2_hmac -c hmac.key --hex data.file`
	4. [06:21]() - Practical OTP Generation
		1. `tpm2_import`
3. Secure storage on the  TPM : [link](https://www.youtube.com/watch?v=4hyqzPfgX-g)
	1. Also called Non-volatile Random Access Memory (NVRAM)
	2. [00:16]() - Access type
		1. bulk read/write
		2. Counter - TPM command to only increase counter one at a time
		3. Set bit -
		4. Use as PCR - covered in advance course
	3. [02:03]() - Workflow
		1. Define NV index - size of data, and auth type
		2. read/write operation defined prev
		3. undefine as NV Index - as good as deleting the data. (Delete Operation)
	4. [04:06]() - NV index type and use cases.
		1. Counter type has can have max of 8 byte value
		2. Bits - max 8 byte
		3. Extend type - nvram as PCR
	5. [05:46]() - How to define NV Index
		1. `tpm2_nvdefine [option] [NV index]`
		2. `-C` - hierarchy
		3. `-s` - size of the data area in bytes
		4. `-p` - password
		5. `[NV index]` - last arg, index at which the data will be written if not specified it will take the next available one. Best take auto assigned one
		6. Design you NVRAM memory layout, and see at what index what is stored.
		7. Example
			1. `tpm2_nvdefine -C o -s 20 -p <region password> <index/offset>` output : `nv-index: 0x1000002`
	6. [07:29]() - Writing data
		1. `-i [file path | - (stdin)]` - data to write
		2. `-P` - authorization
		3. `-C` - hierarchy
		4. `-offset [num]` - offset from start of NV Index 
		5. `tpm2_nvwrite -i write.data -offset 8 -P mypassword <NV index>`
	7. [08:38]() - How to read?
		1. `-o` - output file to write the data to.
		2. `tpm2_nvread -P mypassword -s <read size> <NV index>`
	8. [09:30]() - NV Index attributes
		1. owner hierarchy
		2. type of nv index
		3. [11:25]() - other interesting config
	9. [12:28]() - NV Counter Type
		1. `tpm2_nvdefine -s 8 -a "authread|authwrite|nt=1" <nv index>`
		2. `tpm2_nvincrement <nv index>`
		3. `tpm2_nvread -s 8 --print-yaml 7`
4. Understanding the TPM Parameter Encryption: [link](https://www.youtube.com/watch?v=W4jigPWMI6M)
	1. Parameter encryption
	2. Password, Policy, Trial & HMAC
	3. Start TPM session immediately after you create primary key. Since Primary key doesn't leave TPM its a good to generate session key.
	4. `tpm2_startauthsession --hmac-session -c primary.ctx -S session.ctx`
	5. `tpm2_nvdefine -S session.ctx -C o -s 20 -p mypassword 6`
	6. `tpm2_nvwrite -i document.data -P session:session.ctx+mypassword 6`
5. Protecting external Sensitive Data with TPM (encryption/decryption)
	1. `tpm2_createprimary -c primary.ctx `
	2. `tpm2_create -C primary.ctx -G aes128 -c key.ctx`
	3. `tpm2_encryptdecrypt -c key.ctx -o secret.enc secret.data`
	4. `tpm2_encryptdecrypt -d -c key.ctx -o secret.dec secret.enc`
	5. create primary key of ECC(ecc256) and generate the child key of type symmetric encryption this will make encryption faster
	6. `tpm2_rsaencrypt -s,--scheme [padding scheme]`
	7. `tpm2_rsadecrypt -s [padding scheme]`
6. TPM capabilities and key persistence
	1. `tpm2_evictcontrol` - allow objects to be persisted or remove the persisted objects.
	2. `tpm2_changeauth -c o -p newpass newerpass`
	3. `tpm2_getcap -l|--list`
	4. design the slot

## Commands

```bash

docker run --name tpmlab -w ~/lab -it -d --platform=linux/amd64 -e TPM2TOOLS_TCTI=mssim:host=localhost,port=2321 tpmdev/ost2-tpm-course:latest
docker start tpmlab

docker attach tpmlab

# start the server
tpm_server > tpm.log &
tpm2_startup -c

tpm2_getrandom 8 > randombytes

# For hardware TPM set the DA Lockout value to 0 for deactivation.

# TPM it will enter a protection mode called “Dictionary Attack Lockout Mode” or for short DA lockout mode.
tpm2_dictionarylockout –setup-parameters –max-tries=4294967295 –clear-lockout
```

## Question?

1. Slots and TPM memory constrains, when to flush the slots and when and how to load the key.

## External Documents

1. [TCG Overview Document](https://trustedcomputinggroup.org/wp-content/uploads/2019_TCG_TPM2_BriefOverview_DR02web.pdf)

## Related Material

1. [Trusting your RaspberryPi](https://www.youtube.com/watch?v=S6HWK8PF5MU)
2. [wolfTPM Roadmap and Best Differentiators](https://www.youtube.com/watch?v=fuC61ZyWRLU)
	1. Benchmark of TPM modules and cryptographic operations
3. https://designfirst.ee/integrations.html

## Permanent Notes

 1. You can have a separate TPM Chip which you can buy and integrate on you board and all the cryptographic services are available and also its protected against chip-to-chip(Machine in the middle attack) communication you just have to enable it.
 2. 