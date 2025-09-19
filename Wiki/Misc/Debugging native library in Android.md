---
tags:
 - Android
---

How to debug C/C++ code in android build with ndk toolchain

# Using LLDB

1.  Compile program with **debugging info** (i.e. keep the `-g` flag)	After compilation, copy them to your device's `/data/local/tmp` directory.
2.  Copy NDK provided `lldb-server` to your android phone(prefer the 64bit one), and start it by:
	
	```bash
	./lldb-server platform --listen "*:10086" --server
	```
	`10086` is port number, you may change it.
1.  Forward port by running:
	```bash
	adb forward tcp:10086 tcp:10086
	```
1.  Get device name
	```bash
	adb devices   #For me, it's 39688bd9
	```
1.  Install LLDB, adding its binary to `PATH`, typing these commands:
	```bash
	platform select remote-android
	platform connect connect://39688bd9:10086
	```
	
	Among with, `39688bd9` is my device id, `10086` is the port that I choose in previous steps.
1.  Now, you're connected with lldb-server, thus just use lldb like locally:

	```bash
	file some_executable_file_with_debug_info
	b main
	r
	```
1. [GDB commands to LLDB commands mapping](https://lldb.llvm.org/use/map.html)

# How to GDB android native libraries

1. Install NDK from android studio
1. Push appropriate gdb-server to phone
	```
	adb push ~/android-sdk-linux/ndk-bundle/prebuilt/android-<arch>/gdbserver/gdbserver /data/local/tmp
	adb shell "chmod 777 /data/local/tmp/gdbserver"
	adb shell "ls -l /data/local/tmp/gdbserver"
	```
4. Forward ports `adb forward tcp:1337 tcp:1337`
5. Copy libraries for symbols
		```
		mkdir -p ~/dbgtmp/
		adb pull /system/lib ~/dbgtmp/
		adb pull /data/data/<app package>/<lib folder>/* ~/dbgtmp
		```
6. Start application in debug mode
	```
	am set-debug-app -w --persistent <app name>
	am start -n <app name>/<main activity>
	```
7. Attach debugger
	`PID=``adb shell ps | grep instagram | grep -v : | awk '{print $2}'``; CMD="/data/local/tmp/gdbserver :1337 --attach ${PID}"; adb shell su -c "${CMD}"`

8. Start and connect gdb
	```
	~/android-sdk-linux/ndk-bundle/prebuilt/linux-x86_64/bin/gdb
	set solib-search-path ~/dbgtmp/lib
	target remote :1337
	```

## Notes

1. To get module base address
	- Either in GDB
	```
	info file <module name>
	set $base = <module start address gotten from last command>
	```
	 - Or in adb shell - `PID=``adb shell ps | grep instagram | grep -v : | awk '{print $2}'``; adb shell su -c cat /proc/${PID}/maps | grep <app name>`
1. GDB constantly stops on irrelevant signals - `handle <signal name> noprint nostop`
1. It is recommended you put your gdb configs in ~/gdbinit, example:
	```bash
	define instagram
		target remote :1337
		set solib-search-path ~/dbgtmp/lib
		handle SIGSEGV noprint nostop
		handle SIG33 noprint nostop
		handle SIGILL noprint nostop
		printf "Signal handlers on"
	end
	```
1.  Stop on load of shared libraries `set stop-on-solib-events 1`