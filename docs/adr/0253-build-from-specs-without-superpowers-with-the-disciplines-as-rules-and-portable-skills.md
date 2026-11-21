# ADR-253: Build from Specs without superpowers, with the disciplines as rules and portable skills

## Status

accepted (2026-11-21)

## Context

The products are built from Specs, published to each repository's `docs/specs/`, with ADRs in `docs/adr/` and one GitHub issue per Spec task. The [superpowers](https://github.com/obra/superpowers) plugin brings its own brainstorming, design and planning, and its bootstrap skill makes the agent look for a skill before every answer. In the first unattended MagicLink.NET run, brainstorming took over although the Specs and ADRs were already agreed.

What superpowers added is now covered elsewhere: the Spec decides what to build, the ADRs decide the important how, the issues are the plan, and one pull request per issue with CI and CodeRabbit is the review. Current models plan small steps on their own. What is not covered is the discipline while building: models still claim work is done without running the build and tests, and still fix a symptom instead of its cause.

Three kinds of consumer need the same skills: people (the maintainer and outside contributors, in Claude Code, Codex or Copilot CLI), Daedalus.NET through the Thalos.NET skill store, and later Themis.NET, which distributes skills to teams. A separate repository for a curated set was considered and rejected, because it would be a third source for the same thing.

## Decision

* Build sessions in repositories that work from Specs and issues do not use superpowers.
* [standards.md](../../standards.md) states the disciplines as rules that apply to everyone, in any CLI:
  * For a change in behaviour, write a failing test first and see it fail.
  * When something fails, find the cause before changing code again.
  * Before calling work done, run the build and the tests and show their output.
  * Check each review finding against the code; fix what is real and say why when a finding is wrong.
  * For work that touches several files, put a short plan in the pull request description.
  * One issue is one pull request; a Spec task too large for one pull request is split when its issue is written.
* The same disciplines are also written once as portable skills in [superpowers-extensions](https://github.com/MarcelRoozekrans/superpowers-extensions). That repository stays the one source of skills: superpowers becomes optional there instead of a dependency, and its plugins no longer require superpowers skills by name. The portable format is a separate, proposed decision (ADR-254).
* Daedalus.NET loads those skills as an extra Thalos.NET skill root and puts them on the implementer's allow-list, instead of keeping its own copy. Themis.NET distributes the same repository as a pack.

## Consequences

* Contributors get the rules through each product repository's `AGENTS.md`, which Codex, Copilot CLI and Claude Code all read (Claude Code through a `CLAUDE.md` that imports it). That file must point to `standards.md`; a product whose `AGENTS.md` does not yet do so adds the pointer. The skills can be installed without Themis.NET.
* superpowers-extensions changes: its marketplace drops the `obra/superpowers` dependency, references to `superpowers:` skills become conditional, and its README marks which plugins suit contributors.
* The next MagicLink.NET milestone runs without superpowers and is compared with the first run on questions asked, pull requests redone and CI failures.
