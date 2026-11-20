# ADR-224: Keep the core small and put stores, DI and integrations in adapter packages

## Status

accepted (2026-11-20). A concrete refinement of [ADR-008](0008-products-integrate-seamlessly-and-stay-usable-on-their-own.md).

## Context

Products in the line keep a core package with minimal dependencies and put each backend, the DI wiring and each bridge in its own package behind a shared contract. Evidence in product ADRs: ADR-071 (MailPeek), ADR-089 (Rag.NET), ADR-136 (EventSourcing), ADR-143 (Flux), ADR-155 (Mediator), ADR-165 (Outbox), ADR-176 (Rest), ADR-183 (Saga), ADR-186 (Scheduling).

## Decision

* A product's core package carries only what every user needs.
* Each store or backend, the dependency-injection wiring, and each bridge to another product ships as its own adapter package behind a contract in the core.

## Consequences

* Users take only the dependencies they use, which also helps Native AOT and trimming.
* More packages to version and publish; package ids follow [ADR-013](0013-package-ids-follow-the-product-name.md).
