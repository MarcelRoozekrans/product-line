# ADR-230: Keep secrets out of storage and out of runtime dependencies

## Status

accepted (2026-11-20). Records a pattern found across the products.

## Context

Evidence in product ADRs: ADR-027 (AI.Sentinel, replace secrets and personal data with placeholders), ADR-052 (Home Assistant, a secret store is never a runtime dependency), ADR-059 (LongtermMemory-MCP, reject secrets), ADR-082 (Mnemosyne.NET, store the shape of a value, not the value).

## Decision

* Products never store secrets or sensitive values they do not need; where content may contain them, they are redacted or rejected before storage.
* A secret store is used at deploy or start-up time, not as a dependency the running product needs to stay up.

## Consequences

* A leaked database or log holds no secrets.
* Some features need extra work to redact or to keep only the shape of data.
