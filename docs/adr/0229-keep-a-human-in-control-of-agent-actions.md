# ADR-229: Keep a human in control of agent actions

## Status

accepted (2026-11-20). Records a pattern found across the products.

## Context

Evidence in product ADRs: ADR-024 (AgenticAI.DotNet, role-bound tools), ADR-046 and ADR-047 (Daedalus.NET, a human gate and run control), ADR-064 (Lucent Code, approve or bypass tool actions), ADR-084 (Mnemosyne.NET, opt-in), ADR-103 (Thalos.NET, agent-written code only in per-run sandboxes). The same idea runs through Themis.NET and AI.Sentinel (approval and pause).

## Decision

* Agents get only the tools their role needs.
* Destructive or irreversible actions, and control over a run, need a person.
* Code an agent writes runs in a sandbox until a person or a defined gate lets it through.

## Consequences

* Agents are slower where a person is in the loop; that is the trade.
* An unattended build session works within limits a person set beforehand: it may merge a pull request once CI is green, but never releases, publishes or force-pushes (see [standards.md](../../standards.md)).
