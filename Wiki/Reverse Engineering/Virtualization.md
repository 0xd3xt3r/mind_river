
## Intel VT-x

### Virtualization Internals Blog Series

The purpose of this series of articles is to explain how x86 virtualization internally works. I find most of the information dispatched in academical work and research papers, which is pretty hard to understand for beginners, I will try to start from scratch and build knowledge as needed. This could be useful for understanding how virtualization works, or writing your own hypervisor or in other scenarios such as attacking hypervisors security.
https://docs.saferwall.com/blog/virtualization-internals-part-1-intro-to-virtualization/

### Talos Barbervisor

The simple goal of the project was to replay an x86 memory snapshot in a virtual machine (VM) using Intel's virtualization technology Intel VT-x. A snapshot in this case is the collection of system registers and memory at an arbitrary time in execution. Because the main purpose of a VM would be for fuzzing, precise locations of the snapshots are paramount. For example, if there is a file parser in a large GUI driven application, a snapshot could be taken after the GUI has been loaded and after an input buffer has been loaded. With the snapshot in hand researchers can quickly reset to this location, ignoring the precise computing time setting up everything before the parser was called.
[link](https://blog.talosintelligence.com/barbervisor/)

## KVM

1. [Linux Kernel Documentations](https://git.kernel.org/pub/scm/linux/kernel/git/will/kvmtool.git/tree/README)
2. [KVM tutorial](https://lwn.net/Articles/658511/)
3. https://zserge.com/posts/kvm/
4. [Intel Trust Domain Extensions (TDX) Security Review](https://services.google.com/fh/files/misc/intel_tdx_-_full_report_041423.pdf)