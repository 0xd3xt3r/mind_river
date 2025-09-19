---
up: "[[Tooling MoC]]"
tags:
  - "#type/tooling"
created-date: 2024-12-26
related: 
summary:
---
# Bochs Emulator

## Commands
1. `info [cmd]` - very useful for getting low level information
2. `sreg/reg/creg` - inspect different system and other register.

### Magic Breakpoint

When you're using [[Bochs]] with the [internal debugger](http://bochs.sourceforge.net/doc/docbook/user/internal-debugger.html), you can trigger the debugger via a facility called [magic breakpoints](http://bochs.sourceforge.net/doc/docbook/user/bochsrc.html#AEN2324). To trigger a magic breakpoint, you can insert `xchg bx, bx` (in GAS syntax, `xchgw %bx, %bx`) anywhere in the code and Bochs will trap into the debugger as soon as it executes it.

Put the following line in your Bochs configuration file to have it listen to magic breakpoints:
`magic_break: enabled=1`

### GDB support

Then	configure Bochs	with	gdb	stub	enabled.	Under	the	directory	of the	Bochs source code: `sudo ./configure --enable-gdb-stub` or, we can use some shortcut script by running: `sudo sh .conf.linux --enable-gdb-stub`

**Note**:	Enable	debugging	via	gdb	by	adding	`--enable-gdb-stub` instead	of `--enabledebugger` and `--enable-disasm` (they are mutually exclusive). And remember to use `sudo` all the time.

After	the	configuration,	run `sudo make` Add the following line to bochsrc file:
`gdbstub: enabled=1, port=1234, text_base=0, data_base=0, bss_base=0`

Open the bochs tool and it will wait for gdb connection on port 1234 `bochs –f bochsrc.txt`

Open another terminal window, run gdb and connect it to Bochs by:
```
gdb –q kernel.o
target	remote	:1234
```

Then the bochs will be connected to the gdb tool.