
## [Tracepoints](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Tracepoints.html)

- Tracepoints is feature which does software tracing for code in gdb, you can record the data and dump it in the file and process it later.
- [Tutorials to demo how to use tracepoints](https://suchakra.wordpress.com/2016/06/29/fast-tracing-with-gdb/)
- https://www.linuxtopia.org/online_books/redhat_linux_debugging_with_gdb/analyze-collected-data.html
- [Tracepoints file format](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Trace-File-Format.html#Trace-File-Format)
- [GDB Documentation](https://sourceware.org/gdb/current/onlinedocs/gdb.html/index.html)

## Record and Replay
- [GDB Docs](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Process-Record-and-Replay.html#Process-Record-and-Replay)


## Some other interesting features to explore

- [Python Scripting Interface](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Python.html#Python)
- [GDB/MI Interface][https://sourceware.org/gdb/current/onlinedocs/gdb.html/GDB_002fMI.html#GDB_002fMI]
	- GDB/MI is a line based machine oriented text interface to GDB and is activated by specifying using the --interpreter command line option. It is specifically intended to support the development of systems which use the debugger as just one small component of a larger system.
- [In Process GDB Agent](https://sourceware.org/gdb/current/onlinedocs/gdb.html/In_002dProcess-Agent.html#In_002dProcess-Agent)
	- this is to debug process multi-threaded environment where you are not halting the process with breakpoint but have a agent code inside the process.
- [Remote Serial Protocol](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Remote-Protocol.html#Remote-Protocol)
	- GDB Remote debugging is implemented using this protocol.
- [Non-Stop Mode](https://sourceware.org/gdb/current/onlinedocs/gdb.html/Non_002dStop-Mode.html#Non_002dStop-Mode)
	- Its different technique to debug multi-threaded program.