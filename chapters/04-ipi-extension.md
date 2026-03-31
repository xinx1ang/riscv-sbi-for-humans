# 04. IPI Extension

The IPI extension lets software request an inter-processor interrupt to other harts.

In plain language, one CPU core can ask firmware to help notify another CPU core.

## Why this matters

Modern kernels are multi-core. They often need to wake another hart for work such as:

- rescheduling
- TLB-related coordination
- stopping or synchronizing harts
- cross-core coordination during boot or runtime

## Why SBI is involved

Depending on privilege structure and platform design, sending an IPI may require machine-level mediation.
SBI provides a standard path so supervisor software does not need platform-specific machine-mode hooks.

## Mental model

An IPI is not payload delivery. It is a signal.

It usually means: "another hart should trap into its interrupt path and do follow-up work." The real work usually happens in software after the interrupt is observed.

## Common pitfall

Do not treat IPI as the same thing as the higher-level protocol that uses it.

For example:
- the IPI only nudges the remote hart
- the actual reschedule / shootdown / coordination logic lives above SBI

## What to connect this to

The IPI extension becomes much easier to understand once you read it together with RFENCE and SMP kernel design.
A lot of cross-hart coordination is really: signal first, then perform synchronized follow-up work.
