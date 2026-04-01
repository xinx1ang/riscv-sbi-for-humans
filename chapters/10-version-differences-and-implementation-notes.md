# 10. Version Differences and Implementation Notes

The SBI spec is not static.
Versions evolve, extensions are added, and implementation behavior matters.

That means learning SBI is not only about reading one idealized interface definition.
It is also about understanding compatibility and real-world deployment.

## Why this matters

Two systems may both claim to support SBI,
but differ in:

- supported specification version
- supported optional extensions
- implementation-specific quirks
- error handling behavior at the edges

Portable software needs to handle that reality.

## Mental model

Treat SBI as a negotiated capability surface, not a magical guarantee that every platform is identical.

The healthy engineering mindset is:

- detect version
- probe extensions
- branch carefully
- keep fallbacks where practical

## What changes across versions

Version changes may affect:

- which extensions exist
- how capabilities are discovered
- what metadata is exposed
- how software should probe for optional features

That is why the Base Extension is so important.
It gives software a principled way to adapt instead of guessing.

## Why implementation notes matter

The spec defines the contract,
but real systems are built from firmware such as OpenSBI,
bootloaders,
kernels,
and platform hardware behavior.

So implementation notes matter because engineers eventually hit questions like:

- what does this extension look like in OpenSBI?
- which path is optional versus widely expected?
- what does Linux or a hypervisor do when the feature is missing?
- what kinds of failures show up on real boards?

That is where practical understanding starts to exceed the value of a raw function table.

## Common pitfalls

### Assuming every extension is always present

Many SBI features are optional or platform-dependent.
If software assumes too much,
portability breaks quickly.

### Ignoring fallback behavior

A robust kernel does not just use the happy path.
It has to know what to do when an extension is unavailable.

### Treating version numbers as trivia

Version detection is not paperwork.
It influences which ABI assumptions are safe.

## Suggested engineering checklist

When working on SBI-related code, check:

- detected SBI version
- required versus optional extensions
- extension probe results
- expected firmware implementation
- fallback path when a service is absent
- platform-specific notes that affect behavior

## Final perspective

If you remember only one thing from this chapter, remember this:

SBI portability is achieved not by assuming sameness,
but by combining a stable abstraction with careful capability discovery.
