---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/vibe-specs"
created-date: 2026-03-16
related:
summary:
---

### The Deterministic SBOM Drift Architecture

This design relies on standard microservices and focuses purely on cryptographic comparison (diffing) rather than intelligent orchestration.

#### 1. The Edge Sensor (Device Level)

This is a lightweight, compiled binary (ideally written in C or Rust for memory safety and a tiny footprint) sitting on the IoT device.

- **Execution Monitor:** Instead of heavy polling, it hooks into the OS (e.g., using eBPF on embedded Linux) to listen for when a new binary executes or a shared library (`.so`) is loaded into memory.
- **Hasher Engine:** When a file executes, it calculates the SHA-256 hash of the binary.
- **Telemetry Packager:** It batches these hashes along with the Device ID and a timestamp into a lightweight JSON payload.

#### 2. Secure Transport Layer

Instead of custom messaging, stick to industry standards that enterprise firewalls already understand and allow.

- **Protocol:** mTLS (Mutual TLS) over MQTT or simple HTTPS REST.
- **Why:** MQTT is great for low-bandwidth, asynchronous telemetry, and mTLS ensures that only devices with a valid certificate burned in at the factory can talk to your backend.

#### 3. The Backend "Diffing" Engine (Event-Driven)

This is the core of the product. It doesn't need to be "smart"; it just needs to be fast and accurate.

- **Ingestion Gateway:** A simple API Gateway or MQTT Broker that receives the payloads and drops them into a message queue (like RabbitMQ or AWS SQS) to buffer the traffic.
- **The Verification Service (Microservice):** A fast backend worker (e.g., written in Go or Node.js). It picks up the payload from the queue.
    - _Step 1:_ It looks up the Device ID in the database to find its assigned "Golden SBOM."
    - _Step 2:_ It runs a rapid, deterministic comparison: _Does the hash from the device exist in the Golden SBOM?_
- **The Alerting Service:** If a hash is found that _isn't_ in the Golden SBOM (a drift), this service fires an alert to the dashboard and logs the incident.
    

#### 4. Data & Storage Layer

- **Relational Database (PostgreSQL):** Stores the mappings: Customer -> Device ID -> Current Firmware Version.
- **Blob Storage (S3/MinIO):** Stores the actual Golden SBOM files in standard formats like CycloneDX or SPDX.
- **Cache (Redis):** Caches the known-good hashes for the current firmware version so the Verification Service doesn't have to parse a massive SBOM file for every single telemetry ping.

#### 5. The Management Dashboard

- A standard single-page application (React) where security teams upload the original "Golden SBOM" when they ship a new firmware update, and view a fleet-wide dashboard showing which devices are "Compliant" and which have "Drifted."

---

### Why this works better for a prototype:

- **Deterministic:** A hash either matches or it doesn't. There are no false positives caused by AI hallucinations.
- **Standard Tech Stack:** You can build this using well-known, boring technology (Postgres, Go/Python, MQTT) which speeds up development and makes debugging vastly easier.
- **Clear Boundaries:** The Edge team and the Backend team can work completely independently once the JSON payload structure is agreed upon.
    

Would you like to drill down into how the Edge sensor should be implemented (e.g., user-space polling vs. kernel-level eBPF), or would you prefer to sketch out the database schema for handling the SBOMs?

## Crypto Standards

The two major industry standards for SBOMs—**CycloneDX** (backed by OWASP) and **SPDX** (backed by the Linux Foundation)—both explicitly require or heavily utilize component names, versions, and multiple cryptographic hashes (usually SHA-1, SHA-256, and SHA-512).

Here is how it actually looks under the hood.

### What an SBOM looks like (CycloneDX JSON Example)

When a company builds their firmware in their CI/CD pipeline, the SBOM generator scans the compiled binaries and shared libraries and outputs a JSON or XML file.

If your IoT device uses OpenSSL, the `components` array in the CycloneDX file will have an entry that looks exactly like this:

JSON

```
"components": [
  {
    "type": "library",
    "name": "libssl.so.1.1",
    "version": "1.1.1k",
    "description": "Secure Sockets Layer and cryptography libraries and tools",
    "hashes": [
      {
        "alg": "SHA-256",
        "content": "e4b6eb6315201c1074e6cb81b67cc156e75d0506ab68ea74fce4586411516ab5"
      }
    ],
    "purl": "pkg:generic/openssl@1.1.1k"
  }
]
```

### How Your Backend Uses This

Because the SBOM standardizes this data, your prototype's **Verification Service** logic becomes incredibly straightforward:

1. **The Edge Agent** sees `libssl.so.1.1` load into the device's memory.
    
2. It calculates the SHA-256 hash locally on the device: `e4b6eb...`
    
3. It sends `{"name": "libssl.so.1.1", "hash": "e4b6eb..."}` to your backend.
    
4. **The Backend** looks up the "Golden SBOM" for that specific device and does a simple key-value check. If the hash matches the `content` field in the JSON, the device is clean. If it doesn't, you have a drift event.
    

### The "Gotcha" to Watch Out For

As you build this, the main technical hurdle isn't the file format, it's the **generation** of the Golden SBOM.

Many basic SBOM tools only scan source code (like `package.json` or `requirements.txt`). For your product to work, your customers must generate **Runtime or Binary SBOMs**. The SBOM must be generated from the final compiled firmware image (using tools like Syft or Trivy) so that it captures the exact hashes of the compiled `.so` (shared object) files and executables, not just the source code dependencies.

Would you like to map out the database schema (e.g., in PostgreSQL) for how to ingest and store these massive CycloneDX JSON files so your backend can query those hashes in milliseconds?


# Innovate the solution

The single smartest, most radically accretive leap you could make right now is to shift the product from **Passive Drift Detection** to **Active Drift Prevention** using a **Local eBPF Enforcement Ring**.

Right now, the architecture is reactive: a binary executes, the agent hashes it, sends it to the cloud, the cloud compares it, and an alert is generated. By the time the dashboard lights up, the malicious payload (the "drift") is already running in memory.

Here is how you make it radically better.

### The Innovation: "Zero-Latency" Local Execution Blocking

Instead of sending every executed hash _up_ to the cloud for verification, you compile the "Golden SBOM" hashes into a highly compressed, memory-efficient format (like a Bloom filter or a flat hash table) and push it _down_ to the Edge Agent on the device.

When the device attempts to load a new shared library or execute a binary, your eBPF sensor intercepts the kernel's `execve` (execute) or `init_module` (load) system call.

**The workflow becomes:**

1. Process attempts to execute `lib_malicious.so`.
    
2. eBPF intercepts the call in kernel space and hashes the file.
    
3. It checks the local "Golden SBOM" hash table on the device.
    
4. If the hash is missing, **eBPF violently blocks the execution** before it even reaches user space.
    
5. The agent sends a tiny, asynchronous alert to the cloud: _"Blocked unauthorized execution of X."_
    

### Why this is a Massive Strategic Advantage

1. **It Solves the "Scale" Problem Instantly:** When you are a small, early-stage team focusing on a prototype, building a cloud backend that can ingest, process, and diff thousands of telemetry hashes per second from a fleet of devices is an expensive infrastructure nightmare. By pushing the diffing logic to the device, your cloud backend only needs to handle rare, low-volume "Alerts" rather than a constant firehose of "Telemetry."
    
2. **True Zero-Trust IoT:** You are no longer just selling an "auditing" tool for compliance; you are selling an active, zero-trust shield. If a hacker exploits a vulnerability to drop a remote access trojan (RAT), the OS simply refuses to run it because the RAT's hash isn't in the factory-approved SBOM.
    
3. **Air-Gapped Survival:** If an IoT device loses internet connection, a cloud-based detection system is blind. With local enforcement, the device remains 100% protected against physical tampering (like someone plugging in a USB drive) even when completely offline.
    
4. **Massive ROI for the Buyer:** Compliance officers will buy your tool for the SBOM auditing, but CISOs will renew the contract because it acts as an un-bypassable application allow-list that physically stops attacks.
    

By flipping the architecture so the edge does the heavy lifting, you build a much stronger security product while actually reducing the amount of cloud infrastructure your team needs to manage right now.