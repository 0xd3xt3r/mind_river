
## Hacking a Printer 

Tags : #bare-metal-firmware, #reverse-engineering, #firmware-reversing, gamom

Github: https://github.com/gamozolabs/canon_pixma_mx492/

### Objective 
1. Dump firmware from flash chip
2. Finding oracle where basically you connect to the device's network service and find signature which identifies the service name version. 
3. Then try to find co-relation between the device functionality and code of the firmware which is finding the firmware code which is handling the command.
4. Find exploit for the firmware
5. Figure out compression and decompression code of the firmware
6. Then patch the firmware and implant it in the device such that it purposefully has [[arbitrary code execution]] vulnerability . This is so that we can image the running device, such that we can fuzz it. This technique can be used for research to look deeper into the system.


### Day 1 Notes

[Source link](https://www.youtube.com/watch?v=2LVtEoQA8Qo&ab_channel=gamozolabs)

### Notes
1. The binary uses [[custom compression]] algorithm, such that it is easy to implement in assembly, you can tell that from entropy graph. The decompression is done in the boot process.
2. Looking for loading address. 8000, 0000 is a clean address and the decompression algorithm will need location where to put the decompressed binary data.
3. Search for traces for ARM instruction set. There was ARM string in the binary. 
4. Load the binary in Ghidra and witness the magic, with the assumption of ARM v7 little-endian was showed some promising functions but new exploration continued.
5. Look for code that actually look very real, like function with prologue and epilogue.
6. Brute force different architecture, architecture version and endianess to look at what valid code looks like. In this case the actual architecture came out to be ARM thumb v5t big endian architecture the results were not that promising.
7. Different ARM version should be a huge deal.



