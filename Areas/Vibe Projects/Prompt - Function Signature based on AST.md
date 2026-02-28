---
up: "[[Vibe Project MoC]]"
tags:
  - "#type/prompt-idea"
created-date: 2026-02-25
related:
summary: Generate signature of the function based on AST of the function
---

## Notes

The tool captures the functional content of a function via the Abstract Syntax Tree (AST). The AST reflects both source code expression-level functionality and control flow, and can be used to detect detailed information on the source code (i.e. data types, conditional statements, etc.) while ignoring trivial changes (such as file location, function name, comments, line number changes etc.). This is therefore different from version control tools that will highlight even an extra space as a relevant change.

Compare source code to the AST generated from it. Note that while line information is captured in the AST, it is ignored when generating fingerprint information. You could insert a comment between these two blocks, and the fingerprint would not change.

Information from the AST is captured in an MD5 checksum, wherein any meaningful change produces a unique hash. Future work on this tool may see the development of other fingerprint formats that carry more complex information, such as the severity of the impact of a change (was a variable value changed, or was a whole new block of code introduced?).