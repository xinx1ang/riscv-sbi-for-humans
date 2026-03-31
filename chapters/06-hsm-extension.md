# 06. Hart State Management (HSM)

Hart State Management is the SBI extension for controlling hart lifecycle state.

In plain language, it is how software can reason about and request hart start/stop/suspend style transitions through SBI.

## Why this matters

On a multi-core RISC-V system, not every hart is always running in the same state.
During boot, hotplug, low-power flows, or recovery paths, software may need to:

- start another hart
- stop a hart
- suspend and resume harts
- query hart state

## Why SBI owns this boundary

Hart lifecycle control often touches privileged machine-level state.
The kernel may decide policy, but firmware/runtime is commonly responsible for the low-level state transition mechanics.

That makes HSM a natural SBI extension.

## Mental model

HSM is the control plane for hart lifecycle transitions.

The OS says what should happen.
The SBI implementation helps make the transition happen safely on the platform.

## Why it is more subtle than it looks

Starting a hart is not just "turn CPU on".
It usually also involves:

- entry address / boot target
- expected privilege handoff
- initial execution environment
- synchronization with the rest of the system

Similarly, stopping or suspending a hart is not just "turn CPU off". It has ordering and coordination implications.

## Common pitfall

It is easy to read HSM as just a list of hart lifecycle calls.
The real engineering challenge is state-machine correctness:

- what transitions are legal?
- what state is observable?
- what races can happen if multiple actors coordinate badly?
