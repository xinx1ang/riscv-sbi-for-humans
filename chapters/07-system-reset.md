# 07. System Reset

The System Reset extension gives supervisor software a standardized way to request system reset or shutdown style actions through SBI.

In plain language, this is how a kernel says:

- reboot the machine
- power down the machine
- perform a specific reset flow if the platform supports it

## Why this exists

A kernel often knows *when* the system should reboot or shut down,
but the low-level mechanism for actually doing that is platform-specific and privileged.

Without SBI, every kernel would need custom board- or firmware-specific logic to trigger reset behavior.
That would hurt portability.

The System Reset extension standardizes the privilege boundary:

- the kernel makes the request
- the SBI implementation performs the machine-level action

## Mental model

Think of this extension as the final control handoff for system lifecycle events.

The OS decides policy:

- panic and reboot
- graceful shutdown
- restart after update

The SBI implementation performs the actual machine-level reset or shutdown path.

## Why engineers care

Reset logic is easy to underestimate because the API surface looks small.
But if reset behavior is wrong, the system may:

- hang instead of rebooting
- loop unexpectedly
- fail to power off
- behave differently across boards

That makes this extension important for portability and operational reliability.

## What to pay attention to in the spec

When reading the System Reset extension, focus on:

- what reset types are defined
- what reset reasons or categories may be reported
- what happens if the requested reset type is not supported
- whether the call is expected to return on success or only on failure

That last point matters a lot in low-level code.
A successful reset request often means normal control flow never returns.

## Common pitfall

Do not treat reboot and shutdown as just another ordinary function call.
They are lifecycle transitions.
That means error handling and return assumptions need extra care.

If the call returns unexpectedly,
you may already be in a failure case.

## OpenSBI / kernel connection

In practice, this is one of the clearer examples of SBI's role:

- the kernel decides the policy outcome
- firmware hides platform-specific reset machinery
- the same kernel code can work across different machines with less custom glue
