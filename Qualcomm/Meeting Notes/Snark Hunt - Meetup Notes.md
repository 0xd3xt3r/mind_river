---
created-date: 2021-09-23T10:00
up: "[[Meeting MoC]]"
participants: 
related: 
tags:
  - "#type/meeting"
  - "#qpsi/meeting"
---

> **summary**:: Internals learning initiatives

## Resources
- [Snark Hunt](https://confluence.qualcomm.com/confluence/display/QPSI/QPSI+Snark+Hunt+Process)


## 23-09-2021

**Date** : 23-Sept-2021
**Agenda** : Discuss CVE cve-2020-0423
**Reference** : https://www.longterm.io/cve-2020-0423.html

### Questions

### Notes
- The exploit is very simple [[UAF]] in binder driver but the exploitation is very interesting
- It exploits the [[UAF]] to do double free.
- it later does heap spraying with `sendmsg` and `signalfd`
	- slab doesn't exist on ARM64; the smallest slab there is kmalloc-128, which is used for all kmalloc() allocations up to 128 bytes.
- it then goes on to do KASLR Leak by leveraging `signalfd`.
- if you want to exploit any [[heap overflow]] you have to learn [[heap]] internal of [[linux]] kernel. 
- 

### Discussion

**Binder**

**Reference**

https://medium.com/swlh/binder-architecture-and-core-components-38089933bba
https://www.nds.ruhr-uni-bochum.de/media/attachments/files/2011/10/main.pdf
https://titanwolf.org/Network/Articles/Article?AID=a1aed079-dc87-452e-a148-33061c4f7833#gsc.tab=0
https://programmer.group/understanding-android-binder-mechanism-3-3-java-layer.html

### Action Items

## 2021

### URBan Explorers

#### Video Archives
1. [Meet 1: Kickoff and USB Intro & Security](https://qualcomm.sharepoint.com/teams/QPSI/Shared%20Documents/Forms/AllItems.aspx?id=%2Fteams%2FQPSI%2FShared%20Documents%2FURBan%20Explorers%20%2D%202022%20Snark%20Hunt%2FURBan%20Explorers%5F%20Kickoff%20and%20USB%20Intro%20%26%20Security%2D20220217%5F110423%2DMeeting%20Recording%2Emp4&parent=%2Fteams%2FQPSI%2FShared%20Documents%2FURBan%20Explorers%20%2D%202022%20Snark%20Hunt)