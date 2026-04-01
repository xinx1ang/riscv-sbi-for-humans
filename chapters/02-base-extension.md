# 02. Base Extension

The SBI Base Extension is the foundation that lets supervisor software discover what SBI implementation it is talking to.

In plain language, this extension answers questions like:

- what SBI spec version is supported?
- what implementation am I running on?
- which extensions are available?

## Why this matters

Before a kernel relies on a newer SBI feature, it needs a way to discover whether that feature exists.
The Base Extension provides that discovery path.

Without it, software would have to guess based on platform quirks or firmware assumptions.

## What kind of information lives here

Typical Base Extension functionality includes:

- SBI specification version
- implementation ID
- implementation version
- extension availability probing
- machine vendor / architecture / implementation identifiers (where supported by the spec version)

## The important mental model

The Base Extension is not about doing privileged work like setting timers or sending IPIs.
It is about capability discovery and identity.

Think of it as the "hello, who are you, and what do you support?" part of SBI.

## Why kernels care

A portable kernel cannot assume every SBI implementation supports the same optional extensions.
So a clean boot flow often looks like:

1. detect SBI version
2. probe required extensions
3. enable code paths based on availability
4. fall back when a feature is missing

## Common pitfall

A common mistake is to read the Base Extension as boring metadata.
It is actually what makes the rest of SBI usable in a portable way.

If extension probing is done badly, the kernel may call into functionality that firmware does not implement, or miss features that are available.
