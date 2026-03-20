---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/vibe-specs"
created-date: 2026-03-16
related:
summary:
---

# Exploit Surface Cartography

## Core Concept

The fundamental insight is that IoT devices have a *finite, enumerable* attack surface, and that surface can be systematically discovered, catalogued, and probed without any firmware access or source code. The system treats the device as a black box and asks: "what does this device *listen to*, and for each thing it listens to, what inputs could make it misbehave?"

---

## Phase 1: Surface Discovery (Cartography)

Automatically enumerate every input channel the device exposes:

**Network layer:**
- Port scan (TCP/UDP, full range) from LAN, WAN, and guest network perspectives separately — because a port closed on WAN but open on LAN is itself a finding
- TLS/SSL certificate enumeration, cipher suite negotiation, protocol version support
- UPnP SSDP discovery → parse IGD description XML → enumerate every exposed action
- mDNS/Bonjour service enumeration
- DHCP: what options does the device *send* and *process*
- IPv6: link-local, SLAAC, DHCPv6, RA processing
- ICMP: which types/codes does it respond to, does it rate-limit
- Multicast group memberships

**Application layer:**
- HTTP/HTTPS admin UI: crawl every endpoint, form, parameter
- SNMP: walk the full MIB, identify writable OIDs
- TR-069/CWMP: does it initiate sessions, what ACS URL does it contact
- SSH/Telnet: if open, what's the banner, what commands are available pre-auth
- MQTT, CoAP, Zigbee bridge endpoints if present
- Custom proprietary protocols (detected by port/banner fingerprinting)

**Physical/adjacent:**
- Wi-Fi: beacon contents, supported rates, WPS state, PMF support
- Bluetooth if present
- WPS PIN vs PBC state

The output of Phase 1 is a **surface map** — a structured graph of every input channel with metadata.

---

## Phase 2: Attack Grammar Generation

For each discovered surface, select or generate a targeted attack grammar based on:

1. **Protocol identity** — HTTP gets web attack patterns; SNMP gets OID manipulation patterns; UPnP gets SOAP injection patterns
2. **CVE history mapping** — query a CVE database filtered by device class + protocol. A router's HTTP admin UI has a known CVE history; use that history to construct attack patterns that probe for the *class* of vulnerability, not just the specific CVE
3. **Input structure inference** — for unknown/proprietary protocols, use the observed response behavior to infer field boundaries and types, then fuzz at boundaries

The grammars are not generic fuzzers. They're structured: "for this SOAP endpoint, these are the parameter types, here are the boundary values, here are the injection payloads relevant to this parameter type."

---

## Phase 3: Probing & Anomaly Detection

Execute the attack grammars and monitor for anomalies across multiple dimensions simultaneously:

- **Availability**: does the device stop responding? Partial or full?
- **Response deviation**: does the response differ structurally from baseline?
- **State corruption**: does the device's behavior on *subsequent normal requests* change?
- **Information leakage**: does the response contain stack traces, internal IPs, filesystem paths, version strings not in normal responses?
- **Side-channel behavior**: response timing changes, traffic volume changes, unexpected outbound connections
- **Reboot/crash**: power cycle detection via connectivity loss + recovery timing fingerprint

Critically: anomaly detection is *differential*. Every probe is compared against a baseline response, so the system knows what "normal" looks like for that endpoint before it starts attacking it.

---

## Phase 4: The Surface Map Artifact

The output is not a flat list of findings. It's a **versioned, structured graph**:

```
Device: Acme Router XR-500
Firmware: 2.4.1
Surface nodes: 47
  ├── WAN:80/tcp (HTTP redirect) — 3 probes, 0 anomalies
  ├── LAN:80/tcp (HTTP admin) — 412 probes, 2 anomalies
  │     ├── ANOMALY: /cgi-bin/upload — response contains /tmp path on malformed boundary
  │     └── ANOMALY: /api/diag — 45s response time on 8192-byte input (DoS candidate)
  ├── LAN:1900/udp (UPnP SSDP) — 89 probes, 1 anomaly
  │     └── ANOMALY: AddPortMapping with external port 0 — device reboots
  ├── LAN:161/udp (SNMP v1/v2c) — 201 probes, 0 anomalies
  └── ...
```

When firmware 2.4.2 ships, the system re-runs and **diffs the surface maps**:
- New surfaces added (new attack area)
- Old surfaces removed (was that intentional? was it a backdoor?)
- Same surface, different anomaly profile (regression or fix?)
- Same surface, anomaly present in 2.4.1 now gone (confirmed fix)

This diff is the most valuable artifact for a security/QA team reviewing a firmware release.

---

## Phase 5: Behavioral Fingerprint Registry

Every completed surface map is distilled into a set of **behavioral fingerprints** — structured records of the form:

```
{
  protocol: HTTP,
  endpoint_pattern: /cgi-bin/upload,
  trigger: malformed_multipart_boundary,
  symptom: path_disclosure,
  device_class: router,
  hit_rate: 0.73
}
```

These fingerprints are stored in a shared (opt-in) registry and used to accelerate future scans:

- **Probe prioritization**: instead of running all probes uniformly, the system runs highest-yield probes first based on registry hit rates. Faster time-to-finding.
- **Cross-device amplification**: when a new device exposes a surface matching a known fingerprint pattern, that probe is automatically promoted and its anomaly detection weighted more aggressively.
- **Vendor/SDK attribution**: many IoT devices share OEM firmware or SDKs. The registry clusters devices by behavioral similarity and surfaces the insight: "these 14 devices from 6 vendors share the same HTTP stack and the same path disclosure bug."
- **Semi-known unknown discovery**: the gap between "known CVE" and "pure unknown" narrows automatically as the corpus grows — without requiring CVE database updates.

The data structure is already produced by Phase 4. This phase is a write-through index on top of it. The implementation cost is low; the compounding value is high.

---

## The Killer Differentiator

Most vulnerability scanners (Nessus, OpenVAS) work from a signature database — they check for *known* vulnerabilities. This system works from *behavioral anomaly detection against a protocol-aware baseline*. It finds **unknown** vulnerabilities in **known** protocols. That's the gap no existing tool fills for IoT devices.

The fingerprint registry extends this further: every scan makes the system smarter for all future scans. That compounding effect is the moat.

---

## Implementation Complexity Assessment

Pragmatically, this breaks into independently shippable layers:

| Layer | Complexity | Standalone Value |
|---|---|---|
| Surface enumeration only | Low | High — just knowing what's exposed is valuable |
| CVE-mapped attack grammars for top 5 protocols | Medium | Very high — covers 80% of real findings |
| Anomaly detection + differential reporting | Medium | High |
| Firmware-version surface diffing | Low | High |
| Proprietary protocol inference + fuzzing | High | Medium — save for v2 |
| Behavioral fingerprint registry + cross-device correlation | Medium | Compounding — value grows with corpus size |

You could ship layers 1-4 as a compelling v1 and leave proprietary protocol fuzzing for later.
