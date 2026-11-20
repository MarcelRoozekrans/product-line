# ADR-220: Prefer ZeroAlloc libraries where they fit

## Status

accepted (2026-11-20)

## Context

The ZeroAlloc libraries (https://github.com/ZeroAlloc-Net) are Native AOT-friendly, allocation-free .NET libraries, and the product line builds on them. When a build session works unattended, nothing in a product's Specs or ADRs tells it to use them, so it picks whatever it knows.

Options considered: always use a ZeroAlloc library when one exists; prefer one where it fits and say why when not; no rule.

## Decision

* In every product of the line, a build prefers a ZeroAlloc library where one fits the job.
* When a ZeroAlloc library exists for the job but is not used, the pull request says why (for example: a framework the product builds on expects something else).

## Consequences

* The products exercise the ZeroAlloc libraries, which finds their gaps early.
* "Where it fits" leaves room: a framework such as OpenIddict may expect the standard ASP.NET Core dependency injection, and fighting it costs more than it gains.
* Because build sessions read the repository and not the design notes, this rule is also summarised in [standards.md](../../standards.md), which each product's `CLAUDE.md` points to.
