# 05. RFENCE Extension

The RFENCE extension is about remote fence operations.

In plain language, it gives one hart a standardized way to request that other harts perform certain fence or memory-management-related synchronization actions.

## Why this exists

On a multi-hart system, one hart updating address-translation or memory-related state is often not enough.
Other harts may still be running with stale views.

So software needs a way to coordinate global or targeted synchronization.

## Why SBI is involved

Some remote coordination operations cross privilege boundaries and may require machine-level support or standardized dispatch.
RFENCE provides that standard interface.

## Mental model

Think of RFENCE as a distributed synchronization helper.
It is not just "run fence somewhere else" in an abstract sense.
It is about making sure multiple harts converge on a consistent view after critical state changes.

## Why kernels care

Without reliable remote synchronization, you can get subtle bugs such as:

- stale TLB state
- inconsistent address translation behavior
- hart-to-hart memory management mismatches

These bugs are nasty because they can look random and timing-dependent.

## Common pitfall

The difficult part is not knowing what a fence is.
The difficult part is understanding *when remote harts must observe a coordinated state change*.

That is the real reason RFENCE matters.
