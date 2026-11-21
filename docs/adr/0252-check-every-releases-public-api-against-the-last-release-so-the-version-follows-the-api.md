# ADR-252: Check every release's public API against the last release, so the version follows the API

## Status

accepted (2026-11-21). Settles a conflict between products.

## Context

Every product releases with release-please, which takes the next version from Conventional Commits: `fix:` is a patch, `feat:` a minor, `!` or `BREAKING CHANGE:` a major. SemVer defines versions by the public API, but a commit type is only the author's claim about it. A `feat:` that removes a method becomes a minor unless someone remembered `!`. commitlint checks the form of a message, not whether it is true. In unattended runs this slips through easily.

The .NET SDK's package validation compares a package's public API with a baseline version and fails on a breaking change. Checked on 2026-11-21:

* Rag.NET validates each package against its last published version, with suppression files per major.
* Minos.NET and Mnemosyne.NET validate against a fixed baseline (1.0.1 and 1.0.0), not the last release, so an API added later and removed again is not caught.
* AI.Sentinel, Thalos.NET, Ariadne.NET, ZeroAlloc.Telemetry and MagicLink.NET do not validate.
* In Rag.NET a break is accepted by adding a suppression. Nothing checks that a breaking commit comes with it, so a break can ship without a major.

## Decision

* Conventional Commits propose the version; an API check confirms it for breaking changes. Package validation does not report added public API, so for additions the commit type alone decides: a public addition needs `feat:`.
* Every library runs package validation against its last released version, per package. Rag.NET's setup is the template.
* Libraries turn on `ApiCompatEnableRuleCannotChangeParameterName`. The SDK leaves it off by default, so a renamed public parameter, which breaks callers that use named arguments, would otherwise pass.
* A suppression lives only until the release that contains its break becomes the baseline. The SDK reports a suppression that no longer matches as unnecessary, and that stays an error (`ApiCompatPermitUnnecessarySuppressions` is not turned on), so an old entry cannot hide a later break.
* A pull request that adds or extends an API compatibility suppression must contain a commit marked as breaking (`!` or `BREAKING CHANGE:`). Otherwise CI fails.
* The marker must reach the default branch. When a pull request is squash-merged, its title becomes the commit, so the check also requires the marker in the pull request title.
* Before 1.0, SemVer allows breaking changes in a minor; there the check reports but does not fail.
* A deliberate break, such as dropping a target framework under [ADR-251](0251-target-the-net-versions-microsoft-supports-and-drop-old-ones-in-a-yearly-major.md), carries a `!` and adds the suppression package validation asks for; together they pass.

## Consequences

* For breaking changes the version follows the API, not only the commit message. Additions still depend on the right commit type, and so do breaks the SDK's rules do not see, such as a change in behaviour.
* Each repository needs the baseline wiring and the suppression check; Minos.NET and Mnemosyne.NET only move their baseline to the last release.
