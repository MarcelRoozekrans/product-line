# ADR-223: Return expected failures as Result values, not exceptions

## Status

accepted (2026-11-20). Records a pattern found across the products and settles which Result type to use.

## Context

Products in the line return expected failures as Result values. Evidence in product ADRs: ADR-022 (AgenticAI.DotNet), ADR-079 (Minos.NET), ADR-126 (Authorization), ADR-149 (Mapping, TryMap), ADR-175 (Rest), ADR-178 and ADR-179 (ZeroAlloc.Results). The products disagreed on which Result type: AgenticAI.DotNet and Daedalus.NET use CSharpFunctionalExtensions, the others ZeroAlloc.Results, which mirrors that API.

## Decision

* An expected failure (validation, not found, a refused request) is returned as a Result value. Exceptions are for the unexpected.
* The line default is ZeroAlloc.Results, in line with [ADR-220](0220-prefer-zeroalloc-libraries-where-they-fit.md).

## Consequences

* Failure paths are visible in method signatures and cheap at run time.
* AgenticAI.DotNet and Daedalus.NET still use CSharpFunctionalExtensions; moving them is their own work, guided by ZeroAlloc.Results' migration guide. *Open: when, and whether, they move.*
