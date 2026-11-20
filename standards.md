# Product-line standards

The short version of the product-line decisions in [docs/adr](docs/adr/README.md), for anyone (person or AI build session) working on a product in this line. Each product's `CLAUDE.md` points here. When a rule and an ADR seem to differ, the ADR wins.

## Rules a build can check

* **Native AOT.** Libraries are AOT compatible (`IsAotCompatible`), AOT and trimming warnings are errors, and apps and CLIs ship as Native AOT; CI enforces it. A product that cannot yet says so in its own ADR. ([ADR-222](docs/adr/0222-keep-libraries-native-aot-compatible-and-ship-apps-and-clis-as-native-aot-enforced-in-ci.md))
* **Warnings are errors.** Nullable reference types on, a strict analyzer set, suppressions local and explained. ([ADR-227](docs/adr/0227-treat-warnings-as-errors-with-strict-analyzers-and-nullable-reference-types.md))
* **No runtime reflection or assembly scanning.** Generate code at compile time with source generators. ([ADR-221](docs/adr/0221-generate-code-at-compile-time-instead-of-using-runtime-reflection-or-scanning.md))
* **No EF Core in applications.** Use ZeroAlloc.ORM with plain SQL migrations, PostgreSQL by default. Libraries may offer an EF Core adapter package. ([ADR-228](docs/adr/0228-applications-avoid-ef-core-and-use-zeroalloc-orm-with-plain-sql-migrations-on-postgresql.md))
* **Package ids follow the product name.** `Product.NET` and `Product.NET.*`. ([ADR-013](docs/adr/0013-package-ids-follow-the-product-name.md))
* **Async APIs return `ValueTask`** where calls often complete synchronously, take a `CancellationToken`, and use `ConfigureAwait(false)` in libraries. ([ADR-226](docs/adr/0226-use-valuetask-for-async-apis-and-keep-hot-paths-allocation-free.md))

## Rules that guide judgment

* **Prefer ZeroAlloc libraries where they fit**, and say in the pull request why when one exists but is not used. ([ADR-220](docs/adr/0220-prefer-zeroalloc-libraries-where-they-fit.md))
* **Expected failures are Result values** (ZeroAlloc.Results), exceptions are for the unexpected. ([ADR-223](docs/adr/0223-return-expected-failures-as-result-values-not-exceptions.md))
* **Keep the core small**; stores, DI wiring and integrations go in adapter packages. ([ADR-224](docs/adr/0224-keep-the-core-small-and-put-stores-di-and-integrations-in-adapter-packages.md))
* **Own small code rather than take a heavy dependency** for a small need. ([ADR-225](docs/adr/0225-prefer-owning-small-code-over-taking-heavy-third-party-dependencies.md))
* **Keep hot paths allocation-free.** ([ADR-226](docs/adr/0226-use-valuetask-for-async-apis-and-keep-hot-paths-allocation-free.md))
* **Integrate, but stay usable alone**: one identity (OIDC), configuration in git, optional co-hosting, shared telemetry, `services.Add...` wiring. No product may require another to work. ([ADR-008](docs/adr/0008-products-integrate-seamlessly-and-stay-usable-on-their-own.md))
* **A human stays in control of agent actions**: limited tools, a person for destructive steps, sandboxes for agent-written code. ([ADR-229](docs/adr/0229-keep-a-human-in-control-of-agent-actions.md))
* **No secrets in storage or as a runtime dependency.** ([ADR-230](docs/adr/0230-keep-secrets-out-of-storage-and-out-of-runtime-dependencies.md))
* **Security boundaries fail closed, helpers fail open.** ([ADR-231](docs/adr/0231-security-boundaries-fail-closed-helpers-fail-open.md))

## Unattended build sessions

* Read the product's `docs/specs/` and `docs/adr/`, and this file, before writing code.
* One branch and one pull request per issue, tests first. Merge a pull request yourself only when CI is green and your own review passes.
* Never create a release, publish a package or image, or force-push.
* Implementation choices inside a Spec (libraries, names, structure) are yours. A choice that would change behaviour, scope or architecture beyond the Specs and ADRs is not: write it as an open question in the issue and go on with the rest.
