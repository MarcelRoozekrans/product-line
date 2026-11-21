# Product-line decisions

Architecture decisions that apply to every product in the line. Each product keeps its own decisions in its own repository; ADR numbers run across the whole line, so the numbers here have gaps.

Several decisions record a pattern found across the products. Their Context names the product ADRs that show it; those live with the products and are not all published yet.

* [ADR-008: Products integrate seamlessly and stay usable on their own](0008-products-integrate-seamlessly-and-stay-usable-on-their-own.md)
* [ADR-013: Package ids follow the product name](0013-package-ids-follow-the-product-name.md)
* [ADR-220: Prefer ZeroAlloc libraries where they fit](0220-prefer-zeroalloc-libraries-where-they-fit.md)
* [ADR-221: Generate code at compile time instead of using runtime reflection or scanning](0221-generate-code-at-compile-time-instead-of-using-runtime-reflection-or-scanning.md)
* [ADR-222: Keep libraries Native AOT compatible and ship apps and CLIs as Native AOT, enforced in CI](0222-keep-libraries-native-aot-compatible-and-ship-apps-and-clis-as-native-aot-enforced-in-ci.md)
* [ADR-223: Return expected failures as Result values, not exceptions](0223-return-expected-failures-as-result-values-not-exceptions.md)
* [ADR-224: Keep the core small and put stores, DI and integrations in adapter packages](0224-keep-the-core-small-and-put-stores-di-and-integrations-in-adapter-packages.md)
* [ADR-225: Prefer owning small code over taking heavy third-party dependencies](0225-prefer-owning-small-code-over-taking-heavy-third-party-dependencies.md)
* [ADR-226: Use ValueTask for async APIs and keep hot paths allocation-free](0226-use-valuetask-for-async-apis-and-keep-hot-paths-allocation-free.md)
* [ADR-227: Treat warnings as errors, with strict analyzers and nullable reference types](0227-treat-warnings-as-errors-with-strict-analyzers-and-nullable-reference-types.md)
* [ADR-228: Applications avoid EF Core and use ZeroAlloc.ORM with plain SQL migrations on PostgreSQL](0228-applications-avoid-ef-core-and-use-zeroalloc-orm-with-plain-sql-migrations-on-postgresql.md)
* [ADR-229: Keep a human in control of agent actions](0229-keep-a-human-in-control-of-agent-actions.md)
* [ADR-230: Keep secrets out of storage and out of runtime dependencies](0230-keep-secrets-out-of-storage-and-out-of-runtime-dependencies.md)
* [ADR-231: Security boundaries fail closed, helpers fail open](0231-security-boundaries-fail-closed-helpers-fail-open.md)
* [ADR-239: Show people's activity per team by default, and names only after a DPIA](0239-show-peoples-activity-per-team-by-default-and-names-only-after-a-dpia.md)
