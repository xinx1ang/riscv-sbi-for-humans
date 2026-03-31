# 00. What SBI is

The RISC-V Supervisor Binary Interface (SBI) is the interface between supervisor-level software and machine-level runtime/firmware.

In plain language:

- your OS kernel usually runs in S-mode
- some hardware control operations require M-mode privileges
- SBI is the standardized way for S-mode software to ask M-mode firmware to do those operations

You can think of SBI as a small contract between the operating system and the machine-level runtime.

## Why SBI exists

Without SBI, every kernel would need board-specific or firmware-specific machine-mode hooks.
That would make kernels less portable.

SBI provides a stable abstraction so that:

- kernels can use timers, IPIs, hart control, reset, and other services
- firmware can hide platform-specific details
- operating systems can run across different RISC-V implementations with less custom glue

## Where SBI sits in the stack

A simplified software stack often looks like this:

```text
Applications
  ↓
User space
  ↓
Kernel (S-mode or HS-mode)
  ↓
SBI
  ↓
Machine firmware/runtime (often OpenSBI in M-mode)
  ↓
Hardware
```

## Typical example

A kernel wants to program the next timer interrupt.
In many systems, the kernel cannot directly touch the machine timer from S-mode.
So it makes an SBI call, and the SBI implementation in M-mode programs the timer on its behalf.

## Important mental model

SBI is not a device driver framework.
It is not a full firmware API for everything.
It is a narrow privilege boundary interface for operations that need machine-level help.

## Read this repository with two questions in mind

For each SBI function, ask:

1. Why can't the caller just do this directly?
2. Why is this operation standardized at the SBI layer?
