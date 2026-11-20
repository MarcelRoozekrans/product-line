# ADR-227: Treat warnings as errors, with strict analyzers and nullable reference types

## Status

accepted (2026-11-20). Records a pattern found across the products.

## Context

Evidence in product ADRs: ADR-019 (AdoNet.Async, AOT warnings as errors), ADR-023 (AgenticAI.DotNet), ADR-099 (SSH.NET.Fork.Abstractions), ADR-147 (Inject, fail the build on unsafe wiring). ADR-099 asked whether quality gates belong in an ADR at all; a line-wide ADR answers that once.

## Decision

* Every project builds with warnings as errors, nullable reference types enabled, and a strict analyzer set (including ZeroAlloc.Analyzers where it applies).
* A suppression is local, with a reason next to it.

## Consequences

* Problems surface at build time, also in unattended build sessions.
* Upgrading an analyzer or the SDK can break the build; that is intended.
