---
up: "[[Writing MoC]]"
tags:
  - "#type/writing/social-media"
status: published
created-date: 2024-12-21
---

Convenience plays an important role in the success of any security feature. 

Intel BIOS Guard is a very good example of how a lack of usability can undermine even a well-intentioned innovation.

Intel BIOS Guard was designed to address the risk of flash-based attacks on the BIOS by hardening the mechanisms responsible for updating the system firmware.

Before BIOS Guard, the responsibility for updating the BIOS on Intel-based hardware rested with the System Management Mode (SMM). SMM had read/write access to the SPI flash, where firmware was stored. This access made it a target for attacks, allowing malicious actors to exploit SMM and gain persistence on the system.

To counter this, Intel introduced BIOS Guard, which shifted the responsibility for updating the SPI flash from SMM to a new subsystem called the Intel Embedded Controller. This change aimed to close the security gap and reduce the attack surface.

But this feature was a failure, reason? It was not convenient to use!

This feature required an immediate system restart after the update. This limitation posed a challenge for organizations managing fleets of machines, where downtime during updates was impractical and disruptive. Also for normal user it seems that was a big ask!

This issue led many vendors to avoid enabling the feature, with even major players like Apple choosing not to implement BIOS Guard. Perhaps most surprising, Intel itself - the very creator of the feature - also chose not to enable it.

The problem was still alive and Intel had another trick tucked up its sleeves: Intel Boot Guard. But that a discussion for another day.

⚡ ⚡ ⚡

I came across this anecdote while I was reading "Rootkits and Bootkits", a fascinating book that packs decades of research on low-level systems. If you’re interested in system security, I highly recommend giving it a read.

As someone passionate about System security, I frequently explore related topics on my blog, https://taintedbits.com. Feel free to check it out for more insights!