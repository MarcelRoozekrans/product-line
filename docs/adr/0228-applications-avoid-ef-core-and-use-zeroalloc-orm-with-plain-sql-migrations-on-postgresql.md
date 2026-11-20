# ADR-228: Applications avoid EF Core and use ZeroAlloc.ORM with plain SQL migrations on PostgreSQL

## Status

accepted (2026-11-20). Records a pattern found across the products.

## Context

Evidence in product ADRs: ADR-016 (MagicLink.NET), ADR-045 (Daedalus.NET replaced EF Core with ZeroAlloc.ORM), ADR-207 (ZeroAlloc.Templates). ADR-081 (Mnemosyne.NET) also uses PostgreSQL as its only store. EF Core has no Native AOT path for its stores. Libraries still offer EF Core adapters for their users (ADR-151 Mapping, ADR-186 Scheduling).

## Decision

* Applications in the line do not use EF Core. They use ZeroAlloc.ORM with plain SQL migrations, on PostgreSQL by default.
* Libraries may still offer an EF Core adapter package for their users ([ADR-224](0224-keep-the-core-small-and-put-stores-di-and-integrations-in-adapter-packages.md)).

## Consequences

* Applications stay Native AOT compatible ([ADR-222](0222-keep-libraries-native-aot-compatible-and-ship-apps-and-clis-as-native-aot-enforced-in-ci.md)).
* Migrations are hand-written SQL. Where a product must support another database, that is decided in the product's own ADR.
