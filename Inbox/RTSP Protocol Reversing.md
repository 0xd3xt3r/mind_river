---
up: "[[Knowledge Base MoC]]"
tags:
  - "#type/knowledge"
status: todo
created-date: 2025-02-02
related: "[[IP Camera Vulnerability Research]]"
summary: How to understand and reverse engineering RTSP Protocol
---

## Notes

1. What is the functional requirement of the protocol?
	1. It helps in setting up and managing connections between devices for streaming audio or video.
	2. This protocol was designed to control video and audio streams without downloading media files.
2. The main disadvantage of RTSP is that it isn't widely used for broadcasting multimedia over the Internet.
3. RTSP uses the Real-time Transport Protocol (RTP) with Real-time Control Protocol (RTCP) to deliver media streams.

## Reference

1. https://getstream.io/glossary/rtsp-protocol/
2. [Protocol Specification](https://www.ietf.org/rfc/rfc2326.txt)