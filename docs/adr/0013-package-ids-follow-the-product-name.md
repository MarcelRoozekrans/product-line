# ADR-013: Package ids follow the product name

## Status

accepted (2026-11-20)

## Context

Themis.NET had been given the package prefix `ThemisNet.`, to avoid another owner's packages that start with `Themis.`. When MagicLink.NET needed package ids, the decision was that this naming is not right: a product should use its own name, as the other products in the line already do.

## Decision

* Across the product line, the main package id is the product name, and further packages start with the product name and a dot. For example: `Themis.NET`, `Themis.NET.Server`; `MagicLink.NET`, `MagicLink.NET.Abstractions`.
* For Themis.NET this replaces the prefix `ThemisNet.`.

## Consequences

* Package names match the product names, so nobody has to translate between them.
* Nothing had been published under `ThemisNet.`, so switching cost nothing.
* Themis.NET carries a risk: another owner publishes packages starting with `Themis.`. If that prefix is reserved, nuget.org rejects any id starting with `Themis.`. A reservation for the product line's prefixes has been requested. *Open: the answer of nuget.org; if `Themis.NET.*` is refused, this decision is revisited for Themis.NET.*
