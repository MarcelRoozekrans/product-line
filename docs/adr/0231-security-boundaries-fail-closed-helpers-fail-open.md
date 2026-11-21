# ADR-231: Security boundaries fail closed, helpers fail open

## Status

accepted (2026-11-20). Settles a conflict between products.

## Context

Products decided differently what happens when a check itself fails: AI.Sentinel fails closed (ADR-025), Ariadne.NET fails open on purpose (ADR-031), and Rag.NET's security features fail open by default (ADR-091). ADR-031 and ADR-091 both asked for one line-wide statement.

## Decision

* A component that is a security boundary (it decides whether something may happen, such as AI.Sentinel's hooks or a sign-in) fails closed: when it cannot decide, the answer is no.
* A component that helps or advises (guidance, linting, an architecture check during development) fails open: when it cannot decide, work goes on and the failure is reported.
* Each product states in its own ADR which of its components are boundaries.

## Consequences

* Rag.NET's retrieval limits by role or trust level are a boundary, so failing open by default contradicts this. Settled on 2026-11-21 by Rag.NET's ADR-250: deny-by-default for untagged chunks comes as an option in 1.x and becomes the default in 2.0, with a startup check that warns about untagged chunks.
* Ariadne.NET's fail-open stays, as long as it acts as a helper and not as a gate.
