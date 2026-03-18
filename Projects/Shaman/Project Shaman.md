---
up: "[[Personal Project MoC]]"
aliases:
  - Shaman DBI
created-date: 2024-12-16
status: in-progress
tags:
  - "#type/project/personal"
---

> **Summary**:: Shaman is a Dynamic Binary Analysis framework

## Feature Roadmap

```base
filters:
  and:
    - file.hasTag("type/product-feature")
    - file.hasLink(this.file)
properties:
  file.name:
    displayName: File name
views:
  - type: table
    name: Map of Knowledge
    order:
      - file.name
      - Summary
      - file.mtime
    sort:
      - property: file.mtime
        direction: DESC
    columnSize:
      file.name: 319
      note.summary: 834
      file.mtime: 333

```

## Tasks and Questions

### ToDo

- [ ] #task File system monitoring ➕ 2026-03-18
	- [ ] monitor change in file content with inotify kernel API
		- [ ] this is usually done to monitor data/config files
		- [ ] These file change as an when you interact with the device.
	- [ ] monitor the binary for any change in hash - 
		- [ ] this will usually be done to monitor executable file which don't change dynamically
		- [ ] these file usually changes across version
		- [ ] we will usually monitor for hashes
- [ ] #task Attack Surface Mapping ➕ 2026-03-18
- [ ] #task [[Feature - Dynamic SBOM monitor]] ➕ 2026-03-18
- [ ] #task OpenWRT Test Targets ➕ 2026-03-18
	- [ ] Test router target for various architectures like MIPS, ARM, etc.
	- [ ] https://openwrt.org/docs/guide-user/virtualization/qemu


### Completed Task

