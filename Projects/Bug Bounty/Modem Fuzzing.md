---
tags:
  - vulnerability-research
status: in-progress
created-date: 26-03-2021
---
# Data Modem Fuzzing
/modem_proc/datamodem_r/tools/tests/fuzzing


# VoIP

## Protocol Used
- [SDP Protocol - Session Description Protocol](https://vocal.com/voip/sdp/)
- [RTP - the Realtime Transport Protcol](https://vocal.com/voip/rtp/)
- [SDP - the Session Description Protocol](https://vocal.com/voip/sdp/)
- [TLS - Transport Layer Security](https://vocal.com/networking/sslv3tlsv1/)

## Open Incidents
1. [Samsung Incident found by Project Zero Team](https://googleprojectzero.blogspot.com/2023/03/multiple-internet-to-baseband-remote-rce.html)
These are all the ticket opened by [Ivan Fatric](https://bugs.chromium.org/p/project-zero/issues/list?q=label%3AVendor-Samsung%20finder%3Difratric&can=1)

Natalie Silvanovich

[CVE-2023-26072](https://bugs.chromium.org/p/project-zero/issues/detail?id=2395&sort=-id&q=CVE-2023-26072&can=1)
[CVE-2023-26073](https://bugs.chromium.org/p/project-zero/issues/detail?id=2396&q=label%3AVendor-Samsung%20finder%3Difratric&can=1)

**Critical Vulnerabilities as be the blog**
1. Baseband modem chipsets do not properly check format types specified by the Session Description Protocol (SDP) module, which can lead to a denial of service.
1. Affected Chipset : 
	1. Mobile : Exynos Modem 5123, Exynos Modem 5300, Exynos 980, Exynos 1080
	2. Auto : Exynos Auto T512, Exynos Auto T5124, Exynos Auto T5125, Exynos Auto T5126
2. CVEs
	1. [CVE-2023-24033](https://bugs.chromium.org/p/project-zero/issues/detail?id=2382)
	2. [CVE-2023-26496](https://bugs.chromium.org/p/project-zero/issues/detail?id=2402)
	3. [CVE-2023-26497](https://bugs.chromium.org/p/project-zero/issues/detail?id=2401)
	4. [CVE-2023-26498](https://bugs.chromium.org/p/project-zero/issues/detail?id=2400)

Target Protocols

The Session Description Protocol was designed for the purpose of describing media sessions. Typically SDP is not a standalone protocol, but rather is used by other signaling protocols such as [SIP](https://vocal.com/sip-2/), [RTSP](https://vocal.com/v2oip/rtsp/), or [MGCP](https://vocal.com/voip/mgcp/ "MGCP")

This is a VoIP protocol and especially for [video conference application](https://googleprojectzero.blogspot.com/search?q=whatsapp).


## Targets

1. [Facebook thrift library](https://github.com/facebook/fbthrift)
2. [SDP RFC](https://datatracker.ietf.org/doc/html/rfc4566)