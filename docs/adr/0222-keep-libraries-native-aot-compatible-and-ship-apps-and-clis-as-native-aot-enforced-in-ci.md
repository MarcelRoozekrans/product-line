# ADR-222: Keep libraries Native AOT compatible and ship apps and CLIs as Native AOT, enforced in CI

## Status

accepted (2026-11-20). Records a pattern found across the products.

## Context

Libraries in the line set `IsAotCompatible`, treat AOT warnings as errors, and CI runs a native publish smoke test or holds a zero-warning baseline. Evidence in product ADRs: ADR-016 (MagicLink.NET), ADR-019 (AdoNet.Async), ADR-028 (AI.Sentinel), ADR-033 (BlazorNative), ADR-045 (Daedalus.NET), ADR-078 (Minos.NET), ADR-105 (Thalos.NET), ADR-206 (ZeroAlloc.Templates).

## Decision

* Libraries are Native AOT compatible (`IsAotCompatible`), and AOT and trimming warnings are errors.
* Applications and CLIs are published as Native AOT.
* CI enforces it, with a native publish smoke test or a zero-warning baseline.
* A product that cannot meet this yet says so in its own ADR and makes AOT a goal until it can (as MagicLink.NET does).

## Consequences

* Fast start-up and small memory, which suits small services and CLIs.
* Reflection-heavy dependencies are ruled out or wrapped (see [ADR-221](0221-generate-code-at-compile-time-instead-of-using-runtime-reflection-or-scanning.md) and [ADR-225](0225-prefer-owning-small-code-over-taking-heavy-third-party-dependencies.md)).
