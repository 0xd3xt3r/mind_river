---
up: "[[Learning MoC]]"
tags:
  - "#type/video-stream"
  - linux
  - debugger
status: todo
created-date: 06-12-2024
media_link: https://www.youtube.com/watch?v=ISFLjtUYKx4&ab_channel=gamozolabs
---

## Annotations

- [1:09:00]() - explains Linux ptrace API's
- [1:20:00]() - issue of waiting for the tracee process
- [1:32:00]() - discusses the  solution we he want to implement
- [1:40:00]() - discusses the issue of waiting for the tracee process

## Notes

- [Project Link](https://github.com/gamozolabs/lemonade/)

- ptrace API's are not stable and you need to do work around to get things working.
- writing this debugger for phones only
- SIGSTOP : When SIGSTOP is sent to a process, the usual behaviour is to pause that process in its current state. The process will only resume execution if it is sent the SIGCONT signal. SIGSTOP and SIGCONT are used for job control in the Unix shell, among other purposes. SIGSTOP cannot be caught or ignored.
- pidfd_open sub-system is not entirely useful but can be used.
- SIGCONT : When SIGSTOP or SIGTSTP is sent to a process, the usual behaviour is to pause that process in its current state. The process will only resume execution if it is sent the SIGCONT signal. SIGSTOP and SIGCONT are used for job control in the Unix shell, among other purposes.
- waitpid behavior flag. WCONTINUED - also return if a stopped child has been resumed by delivery of SIGCONT.
- WNOWAIT  Leave  the  child in a waitable state; a later wait call can be used to again retrieve the child status information. WNOWAIT Leave  the  child in a waitable state; a later wait call can be used to again retrieve the child status information.
	- this allows us to do blocking peak

## Task

- [ ] Complete watching this stream #learning ➕ 06-12-2024
