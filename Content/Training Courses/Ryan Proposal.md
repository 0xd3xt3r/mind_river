
# High Level Skill Set

## Fuzzing

## Reverse Engineering

## Exploitations

# Trainings

## Fuzzing

### Hardik Shah

Introduction  
• Different types of vulnerabilities  
• Buffer overflow  
• heap overflow  
• integer overflow  
• use after free  
• out of bound read/Write  
• Hands on: Manually identifying the vulnerabilities in sample C code.  
• What is fuzzing?  
• Fuzzing Process  
• Different types of fuzzer  
• dumb fuzzer  
• mutation fuzzer  
• coverage guided fuzzer.  
• Basic blocks and code coverage  
• Binary instrumentation  
• Corpus collection  
• Corpus minimization  
• What is AFL and AFL++?  
• How does it works?  
• Fork server Vs persistent mode  
• How to write harness for persistent mode  
• Fuzzing Strategies  
• Different Sanitizers  
• ASAN  
• UBSAN  
• MSAN  
• Using AFL  
• How to compile and install AFL++  
• How to compile Simple C program with AFL++  
• Various compilation options for AFL++  
• Fuzzing Simple C program using AFL++  
· Fuzzing real world programs  
• Fuzzing TCPDump  
• Fuzzing libtiff
• Advanced Topics with AFL++  
• Using HongFuzz
• Using LibFuzzer
• Hands on Fuzzing exercises  
• Fuzzing ImageMagick  
• Fuzzing libEMF  
• Fuzzing libGD  
• Fuzzing OpenSSL
• Root cause analysis and debugging using GDB  
• Crash triaging using Crashwalk  
• OSS-Fuzz introduction  
• Firmware Fuzzing  
• Q & A  
• Conclusion

https://typhooncon.com/mastering-fuzzing-a-comprehensive-training-on-identifying-vulnerabilities-in-software/

### ringzer0 training

#### Part 1: Introduction to vulnerability research and fuzzing

1. Introduction to vulnerability research    
2. Code review
3. Reverse engineering
4. Fuzzing
5. Introduction to fuzzing
6. Methodology (target analysis, attack surface)
7. Setup harness writing
8. Corpus management
9. Monitoring the fuzzing campaign
10. Crash triaging
11. Common tools
12. Limits: hard to reach / detect bugs, inappropriate targets
13. Memory sanitizers

#### Part 2: Instrumentation of binary-only software's

1. Static instrumentation
2. Handling and modifying binary formats
3. Dynamic instrumentation
4. Hooking, Hot-patching, frida
5. Dynamic Binary Instrumentation (DBI)
6. QBDI, understanding, preload, bindings
7. harnessing at binary-level

#### Part 3: Binary-level fuzzing with Honggfuzz/QBDI

- fuzzing closed-source binaries with Honggfuzz and QBDI
- being faster:
    - optimizing instrumentatation
    - DBI-based fork-server
    - DBI caching mechanism
    - DBI persistency
- getting deeper:
    - comparison breaking
    - function interposition etc.

#### Part 4: Exploration and vulnerability research with symbolic execution

- introduction to symbolic execution
- SMT solving: concepts, theories (BV, Array)
- usage of SMT for reverse-engineering
- introduction Triton
- TritonDSE: path exploration with Triton
- bridging the gap with fuzzers: building a basic in-memory concolic fuzzer

All softwares and tools provided during this training can be freely used but not shared for those that are not open-sourced (if any).

## Vulnerability Class

## Reverse Engineering

## Exploitation
