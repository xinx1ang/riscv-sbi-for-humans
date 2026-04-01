# 08. PMU

The PMU extension is about performance monitoring support through SBI.

In plain language, it helps supervisor software work with performance counters and monitoring facilities using a standardized interface,
especially when direct low-level control is privileged or platform-specific.

## Why this matters

Modern kernels and performance tools care about questions like:

- how many cycles were spent?
- how many instructions retired?
- how often did cache or branch-related events happen?
- can profiling tools access counters safely and portably?

Performance monitoring is important for tuning, observability, and debugging.
But the exact hardware and privilege boundaries can vary by platform.

That is where SBI helps.

## Mental model

The PMU extension is not the performance analysis itself.
It is the privilege-boundary interface that helps software configure or access performance monitoring resources in a portable way.

Think of the layers like this:

- profiling / observability tools want data
- the kernel manages policy and exposure
- SBI provides standardized machine-level mediation where needed

## Why portability matters here

Performance monitoring features can differ significantly between implementations.
A portable OS cannot rely only on platform-specific control paths.

The PMU extension helps make at least part of this story more uniform,
so kernels and tooling can reason about counters with fewer hard-coded assumptions.

## What engineers should watch for

When reading or implementing PMU support, important questions include:

- which counters or events are available?
- who owns configuration authority?
- how are counters started, stopped, read, or filtered?
- what virtualization or privilege restrictions apply?
- what failures mean "unsupported" versus "temporarily unavailable"?

These details matter because PMU is often used by tooling,
and tooling tends to amplify any ABI ambiguity.

## Common pitfall

Do not think of PMU as only a benchmarking feature.
It is also a debugging and production-observability feature.
If PMU exposure is inconsistent,
performance analysis becomes much harder across different systems.

## Practical connection

For real systems, PMU support sits at the intersection of:

- hardware counter capabilities
- firmware mediation
- kernel perf integration
- virtualization and security policy

That makes it one of the more subtle SBI areas despite sounding straightforward at first.
