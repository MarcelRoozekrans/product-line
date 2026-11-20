# ADR-226: Use ValueTask for async APIs and keep hot paths allocation-free

## Status

accepted (2026-11-20). Records a pattern found across the products.

## Context

Evidence in product ADRs: ADR-018 (AdoNet.Async), ADR-120 (AsyncEvents), ADR-128 (Cache), ADR-142 (Flux).

## Decision

* Async APIs return `ValueTask` or `ValueTask<T>` where calls often complete synchronously, take a `CancellationToken`, and use `ConfigureAwait(false)` in library code.
* Hot paths do not allocate per call; allocation budgets are checked in tests where it matters (for example with ZeroAlloc.TestHelpers).

## Consequences

* Lower allocation and latency, which is the point of the ZeroAlloc line.
* `ValueTask` has rules (await once, no concurrent awaits) that callers must respect.
