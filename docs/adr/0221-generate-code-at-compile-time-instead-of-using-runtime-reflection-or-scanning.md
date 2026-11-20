# ADR-221: Generate code at compile time instead of using runtime reflection or scanning

## Status

accepted (2026-11-20). Records a pattern found across the products.

## Context

Across the product line, libraries generate dispatch, registration, proxies and serializers from attributes or partial types at compile time, and have retired runtime reflection and assembly scanning. Evidence in product ADRs: ADR-021 (AdoNet.Async), ADR-078 (Minos.NET), ADR-121 (AsyncEvents), ADR-124 (Authorization), ADR-129 (Cache), ADR-135 (Collections), ADR-137 (EventSourcing), ADR-140 (Flux), ADR-145 and ADR-146 (Inject), ADR-148 (Mapping), ADR-152 and ADR-154 (Mediator), ADR-159 (ORM), ADR-164 (Outbox), ADR-167 (Pipeline), ADR-170 (Resilience), ADR-174 and ADR-177 (Rest), ADR-180 (Results), ADR-182 (Saga), ADR-185 and ADR-187 (Scheduling), ADR-196 (StateMachine), ADR-212 (Validation), ADR-216 (ValueObjects).

## Decision

* Code that would otherwise rely on runtime reflection or assembly scanning is generated at compile time, with source generators.
* A product may deviate where the problem is inherently runtime, and says so in its own ADR.

## Consequences

* Behaviour is visible and checkable at build time, and the products stay friendly to trimming and Native AOT ([ADR-222](0222-keep-libraries-native-aot-compatible-and-ship-apps-and-clis-as-native-aot-enforced-in-ci.md)).
* Generators are more work to write and test than reflection.
* Known exception: ZeroAlloc.Specification uses runtime expression trees on purpose (ADR-194).
