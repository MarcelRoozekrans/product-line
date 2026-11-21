# Product-line standards

The short version of the product-line decisions in [docs/adr](docs/adr/README.md), for anyone (person or AI build session) working on a product in this line. Each product's `AGENTS.md` (and the `CLAUDE.md` that imports it) points here. When a rule and an ADR seem to differ, the ADR wins.

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
* **Share telemetry conventions**: OpenTelemetry GenAI names from the pinned version, product attributes under the product's prefix, and log records that carry their trace. ([docs/telemetry.md](docs/telemetry.md))
* **The version follows the API**: package validation against the last release, and a suppression needs a breaking commit. ([ADR-252](docs/adr/0252-check-every-releases-public-api-against-the-last-release-so-the-version-follows-the-api.md))
* **Target the .NET versions Microsoft supports**: libraries all supported versions, applications and tools the newest LTS, generators netstandard2.0. An unsupported version is dropped in a yearly major. ([ADR-251](docs/adr/0251-target-the-net-versions-microsoft-supports-and-drop-old-ones-in-a-yearly-major.md))
* **Report vulnerabilities privately**: every repository has a `SECURITY.md` and private vulnerability reporting; receipt within 5 working days, assessment within 15, and a target of 90 days for fix and disclosure, sooner for critical findings; the previous major gets critical and high fixes for 6 months. ([ADR-248](docs/adr/0248-publish-a-vulnerability-disclosure-policy-for-every-product.md), [SECURITY.md](SECURITY.md))
* **Show people's activity per team by default**: names only after an approval reference such as a DPIA, and team views of at least 3 people. ([ADR-239](docs/adr/0239-show-peoples-activity-per-team-by-default-and-names-only-after-a-dpia.md))

## How to build

These hold for every build session, attended or not, in any CLI. Build sessions do not use superpowers: the Specs, ADRs and issues already decide what to build and how. ([ADR-253](docs/adr/0253-build-from-specs-without-superpowers-with-the-disciplines-as-rules-and-portable-skills.md))

* For a change in behaviour, write a failing test first and see it fail.
* When something fails, find the cause before changing code again.
* Before calling work done, run the build and the tests and show their output.
* Check each review finding against the code; fix what is real and say why when a finding is wrong.
* For work that touches several files, put a short plan in the pull request description.
* One issue is one pull request; a Spec task too large for one pull request is split when its issue is written.
* Fix the cause, not the symptom: no suppressed diagnostic, excluded analyzer or hidden failure to get a green build. When a workaround is the only step for now, for example while waiting on an upstream fix, say what it hides and track the real fix as an issue.

## Public text

Commits, pull requests, issues and comments in these repositories are public and permanent.

* No AI signatures: no AI co-author trailer, no "Generated with" line and no AI session link in commits, pull request bodies, issues or comments. A squash merge copies commit bodies into history, so they must be absent from the commits too. The `commit-hygiene` check of ci-baseline's `conventions.yml` enforces it for commits in a repository that turns it on.
* No nested parentheses in a commit body: release-please cannot parse a body line that starts with a word followed by a nested parenthesis, such as `Foo(Bar(1))`, and drops the commit from the changelog while its workflow succeeds. A fenced code block does not protect it; rephrase so no parenthesis nests. The `commit-hygiene` check runs release-please's own parser on each commit.
* No private details: no hostnames or internal addresses, user names, email addresses, names of household members, home or environment names, tokens, entry ids or paths under a home directory. Describe the problem generically and keep what a maintainer needs: versions, error messages and the relevant code.

## Unattended build sessions

* Read the product's `docs/specs/` and `docs/adr/`, and this file, before writing code.
* One branch and one pull request per issue, tests first. Merge a pull request yourself only when CI is green and your own review passes.
* Never create a release, publish a package or image, or force-push.
* Implementation choices inside a Spec (libraries, names, structure) are yours. A choice that would change behaviour, scope or architecture beyond the Specs and ADRs is not: write it as an open question in the issue and go on with the rest.
