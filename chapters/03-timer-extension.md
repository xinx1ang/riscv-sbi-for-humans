# 03. Timer Extension

The Timer Extension is one of the most important SBI extensions in practice.
It allows supervisor software to request the next timer event through the SBI implementation.

## Why this exists

In many systems, the kernel running in S-mode cannot directly program the machine timer hardware.
That operation is delegated to M-mode firmware.
So instead of touching the timer directly, the kernel asks SBI to do it.

## What this usually means in practice

The kernel decides when it wants the next timer interrupt.
It then issues an SBI timer call.
The SBI implementation programs the machine timer so that a trap/interrupt will happen later.

## Why this is so central

Without timer interrupts, the kernel loses a lot of core OS functionality:

- scheduling
- timekeeping
- sleeping/wakeup
- preemption
- timeout handling

So even though the Timer Extension looks like a small API surface, it is operationally critical.

## Mental model

The Timer Extension is not "time services" in the broad sense.
It is specifically a privilege boundary helper for timer event programming.

The OS still owns scheduling policy and timeout logic.
SBI just provides the privileged operation needed to arm the hardware-facing timer path.

## Common pitfall

Do not confuse:

- deciding *when* the next event should happen
- programming the hardware so it actually happens

The first is kernel policy.
The second is what SBI helps with.

## Where to connect this mentally

When reading OpenSBI or a RISC-V kernel boot/runtime path, the timer extension is one of the first places where the SBI abstraction stops being theoretical and starts affecting normal OS behavior constantly.
