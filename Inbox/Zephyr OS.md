---
up: "[[Knowledge Base MoC]]"
tags:
  - "#type/knowledge"
status: todo
created-date: 2025-12-30
related: "[[Out of Band System Management|OOB Security]]"
summary: 
---


## Building

1. [Install basic dependency](https://docs.zephyrproject.org/latest/develop/getting_started/index.html)
2. [ARM toolchain](https://docs.zephyrproject.org/latest/develop/toolchains/arm_toolchain_for_embedded.html)
3. [Fuzzing with LLVM](https://docs.zephyrproject.org/latest/samples/subsys/debug/fuzz/README.html)
4. [Native simulation board](https://docs.zephyrproject.org/latest/boards/native/native_sim/doc/index.html)


### Commands

```bash
# set toolchain variant
echo $ZEPHYR_TOOLCHAIN_VARIANT
# output -> llvm

# specify toolchain path
echo $LLVM_TOOLCHAIN_PATH
# output -> /home/you/Downloads/ATE

# bul
python3 -m west build -t run -b qemu_x86 samples/subsys/debug/fuzz --force -- -DCMAKE_C_COMPILER=clang-18
 
# compile fuzzing build
west build --force -p -t run -b native_sim/native/64 samples/subsys/debug/fuzz
```