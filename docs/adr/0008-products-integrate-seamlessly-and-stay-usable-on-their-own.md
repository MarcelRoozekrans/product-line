# ADR-008: Products integrate seamlessly and stay usable on their own

## Status

accepted (2026-11-15)

## Context

The products in this line more and more depend on each other: AI.Sentinel, Themis.NET, Ariadne.NET, Thalos.NET, Mnemosyne.NET, Minos.NET, Daedalus.NET, Rag.NET, MagicLink.NET and the ZeroAlloc libraries. Earlier product decisions already pointed this way: AI.Sentinel stays usable without Themis.NET, people sign in through the organization's identity provider, and AI.Sentinel and Ariadne.NET share Rego for policy. The requirement is that the products integrate seamlessly, including the ones that already exist, and that each stays usable on its own.

## Decision

The products integrate in five ways. Each product can still be used on its own, and each way of integrating is optional for it.

1. **One identity.** Products that need sign-in use the organization's identity provider (OIDC).
2. **One way of configuring.** Configuration lives in git, with the git host's review as approval, and the same structure of organization and teams.
3. **One place to host, if wanted.** A product's server part can be hosted inside another product's server (as Themis.NET hosts AI.Sentinel's), and can also run on its own.
4. **One way of sharing data.** Products record events and measurements in a shared way, for example through OpenTelemetry, so one product can read what another records.
5. **One way of wiring in .NET.** Each package plugs into an application the same way, in the `Microsoft.Extensions` style (`services.Add...`), so packages work side by side without glue code.

## Consequences

* A new product is designed against these five from the start.
* Existing products are checked against them; where one falls short, that becomes work for that product, not a reason to drop standalone use.
* Standalone use wins when it conflicts with integration: no product may require another to work.
* A first check found shared telemetry to be the real gap: products record under different names and nothing aligns them yet. Shared conventions are being worked out, starting from the OpenTelemetry GenAI semantic conventions.
* For organizations without an identity provider, magic-link sign-in is offered as a separate OIDC provider: MagicLink.NET.
* *Open: how a server part is hosted inside another product's server (the mechanism), beyond the AI.Sentinel case.*
