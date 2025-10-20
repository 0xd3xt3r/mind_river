---
up: "[[Writing MoC]]"
tags: "#type/writing/social-media"
related: "[[NASS - Fuzzing Android Native System Service]]"
status: todo
created-date : 2025-10-19
---

Interesting read

Spent this weekend reading a interesting paper on Fuzzing Android Native services.

Our goal is to find memory corruption bugs in proprietary native Android system services triggerable via RPCs.

Android has huge user-space attack surface from compromised or malicious application, to mitigate that android has very good sand-boxing and privilege separation by means or permission and SELinux policies. To over-come this privilege boundaries, attackers try to compromise Native services which are more privileged to communicate with Kernel.

In a survey they found 528(30%) of all service are native. Out of those 528 service 316 are proprietary and 44 are only partially open-source.

Attack Path looks like : User Apps -> Natives Services -> Kernel

- The tool tries to recover the Service RPC input specification automatically, they call it Deserialization-Guided Interface Extraction (DGIE). This help to analyze system service even without access to the source code. These services are Build Binder RPC, entry-point of the service can be found easily. Since this interface is very well-documented.
- Isolating code coverage Collecting code coverage

![[android-nass-paper-title.png]]
![[android-nass-attack-path.png]]