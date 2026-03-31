# 01. How SBI calls work

At a high level, an SBI call is an `ecall` executed by supervisor software.
The caller places arguments in registers, specifies an extension ID and function ID, and traps into the SBI implementation.

## Mental model

This is similar in spirit to a system call boundary, except here the boundary is usually:

- caller: S-mode kernel / hypervisor-side software
- callee: M-mode SBI implementation

## Common pieces

An SBI call usually includes:

- extension ID (which service family you want)
- function ID (which function in that extension)
- argument registers
- return value / error code

## Why this matters

Once you understand the calling pattern, the rest of the SBI spec becomes much easier to read.
A lot of the spec is really: "for this extension, here are the IDs, arguments, and return semantics."

## What to pay attention to later

When reading any individual extension, always check:

- privilege assumptions
- whether the operation is synchronous or may affect another hart
- error reporting semantics
- whether the call is mandatory or optional for a platform
