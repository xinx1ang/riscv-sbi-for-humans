# RISC-V SBI for Humans

A human-friendly, engineer-oriented guide to the RISC-V Supervisor Binary Interface (SBI).

Goal: explain the SBI specification chapter by chapter and section by section in clearer language than the raw spec, with context, motivation, call flow, implementation notes, and common pitfalls.

## Why this exists

The SBI spec is precise, but it is not always the easiest document to learn from on a first pass. This repository aims to make SBI easier to understand for:

- people learning the RISC-V software stack
- people reading OpenSBI or OS boot code
- kernel / hypervisor / firmware engineers
- anyone who wants a "why" and "how" explanation instead of only a normative definition

## Planned structure

- 00. What SBI is and where it sits in the software stack
- 01. How SBI calls work (`ecall`, calling convention, error model)
- 02. Base extension
- 03. Timer extension
- 04. IPI extension
- 05. RFENCE extension
- 06. Hart State Management (HSM)
- 07. System reset
- 08. PMU
- 09. Debug console and newer extensions
- 10. Version differences and implementation notes

## Style

Each chapter tries to cover:

- what the spec says
- what it means in plain language
- who calls it
- who implements it
- what can go wrong
- how it connects to OpenSBI / kernels / hypervisors

## Status

Work in progress.
