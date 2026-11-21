# ADR-254: One portable skill format and pack manifest for every product that consumes skills

## Status

accepted (2026-11-21)

## Context

Several products read skills, each with its own rules:

* Thalos.NET.Skills, used by Daedalus.NET, reads `<root>/<name>/SKILL.md` with only `name`, `description` of at most 300 characters on one line, and `tags`; the body is at most 64 KiB, and files next to it are not loaded.
* Themis.NET will render skills per CLI from packs that are git repositories with tags, and needs to know what a pack contains.
* Copilot-Skill-Bridge converts Claude skills for Copilot and filters out skills that need Claude-only tools. Lucent Code loads skill sets from several sources.
* Claude Code, Codex and Copilot CLI read SKILL.md natively, following the open [Agent Skills specification](https://agentskills.io/specification): required `name` and `description`, optional `license`, `compatibility`, `metadata` and `allowed-tools`.

A format that departs from that specification would cut the skills off from the CLIs contributors use. A format that is only the specification leaves out what Thalos.NET and Themis.NET need: a bounded catalogue cost per turn, self-contained bodies, and a table of contents for a pack.

## Decision

The format is a strict profile of the Agent Skills specification plus a pack manifest. Every skill in the profile is also a valid Agent Skill.

**Skill profile**

* A skill is a folder `<name>/` with a `SKILL.md`. The name uses `[a-z0-9-]`, at most 64 characters, and equals the folder name.
* Frontmatter: `name` and `description` are required; `license` and `metadata` are optional. Nothing else.
* `metadata` is the specification's flat map of strings. Tags go there as one space-separated string, `tags: "release ci"`.
* `description` is one line of at most 300 characters and says what the skill does and when to use it.
* The body is self-contained and at most 64 KiB: it refers to no file next to it and names no tool of one particular CLI. It speaks of capabilities, such as "run the tests" or "ask the person", so every consumer maps them to its own tools.
* Text ported from another project keeps that project's copyright notice at the end of the body.

**Pack**

* A pack is a git repository whose releases are tags.
* A manifest `skillpack.json` at the root has this shape, checked against a JSON schema that ci-baseline ships with its validator:

  ```json
  {
    "name": "superpowers-extensions",
    "license": "MIT",
    "skills": ["plugins/disciplines/skills/test-first"],
    "extras": { "claude-code": ["plugins/unattended/hooks/hooks.json"] }
  }
  ```

  `name` is the pack name and `license` an SPDX identifier, both required strings. `skills` is a required array of skill folders, as paths relative to the root. `extras` is an optional object whose keys name a CLI and whose values are arrays of paths only that CLI can use; a consumer ignores the extras of a CLI it does not know. The version is the git tag, not a field.
* For direct installs in Claude Code, the pack may also carry `.claude-plugin/marketplace.json` at the root and a `.claude-plugin/plugin.json` in each plugin folder; the product line's consumers read `skillpack.json`.

**Consumers**

* Thalos.NET.Skills, Themis.NET, Copilot-Skill-Bridge and Lucent Code read the profile and the manifest. Thalos.NET accepts `license` and `metadata`, and reads tags from `metadata.tags`.
* The validator lives in ci-baseline, as a check in `conventions.yml` for a repository that holds a pack. Thalos.NET and Themis.NET validate on load with their own code, against the same rules.

## Consequences

* One skill file works in Claude Code, Codex, Copilot CLI, Daedalus.NET and Themis.NET without conversion.
* Most skills in superpowers-extensions need a shorter description, and the ones that lean on sibling files or Claude-only tools stay outside the profile unless they are rewritten. Their Claude Code plugins keep working.
* Thalos.NET accepts `license` and a one-level `metadata` map, reads tags from `metadata.tags`, and keeps reading its own `tags` key for its existing skills until they move.
* superpowers-extensions adds a `skillpack.json` for the skills that meet the profile, starting with the build-discipline skills of [ADR-253](0253-build-from-specs-without-superpowers-with-the-disciplines-as-rules-and-portable-skills.md).
* The same rules exist in three places, the ci-baseline check, Thalos.NET and Themis.NET; a shared set of example skills, valid and invalid, keeps them in step.
