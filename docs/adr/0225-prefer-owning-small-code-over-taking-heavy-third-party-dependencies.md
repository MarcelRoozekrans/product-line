# ADR-225: Prefer owning small code over taking heavy third-party dependencies

## Status

accepted (2026-11-20). Records a pattern found across the products.

## Context

Products in the line drop vendor SDKs, frontend toolchains and SDK dependencies when they cost AOT compatibility or raise version floors. Evidence in product ADRs: ADR-061 (LongtermMemory-MCP, a dependency-free hook), ADR-070 (MailPeek, no frontend toolchain), ADR-105 (Thalos.NET, own Anthropic and Ollama clients), ADR-112 (Unearth, SHA-256, JSON and MCP in-crate), ADR-176 (Rest), ADR-200 (Telemetry, no OpenTelemetry SDK).

## Decision

* When a third-party dependency would cost Native AOT compatibility, raise version floors or pull in a large tree for a small need, the product owns a small implementation instead.
* For jobs a ZeroAlloc library covers, [ADR-220](0220-prefer-zeroalloc-libraries-where-they-fit.md) applies first.

## Consequences

* Fewer surprises from upstream changes, and AOT stays reachable.
* Owned code must be maintained and tested. This is a preference for small needs, not a rule against all dependencies: OpenIddict in MagicLink.NET is a deliberate dependency of the right size.
