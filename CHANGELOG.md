# Changelog

All notable changes to this project are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.2.0] — 2026-08-18

### Added

- `SKILL.md` — a bootstrap step 0: the skill briefs the user before opening any file (what it reads, what it never reads, the file it creates in their repo, that it never sends anything) and waits for a yes. Catches the two common cases — the session started in the wrong directory, and the user not expecting a context file in their repository.

## [1.1.0] — 2026-08-18

Both changes come from the first end-to-end run of the pipeline on a live product.

### Added

- `references/project-audit.md` — the live surface now outranks the repository for anything customer-facing. A repo routinely lags production on price, plan names, and trial length; the audit fetches the public pricing and home page, records the date it checked, and reports any repo/production disagreement as a finding. In the first real run, the repository held two different price tables, both wrong against production — a cold email written from the repo would have quoted a false price.

### Changed

- `SKILL.md` and `references/three-pass-pipeline.md` — subagent prompts may pass the context file and reference files as absolute paths for the subagent to read itself, instead of pasting them. Pass outputs and the brief are still pasted verbatim.

## [1.0.0] — 2026-08-17

First public release.

### Added

- `SKILL.md` — entry point, two modes (bootstrap and writing), invariants that forbid invented facts, invented capabilities, multiple CTAs, and autonomous sending.
- Three-pass pipeline run as subagents: `writing-drafter` (Sonnet), `writing-reviewer` (Opus, never rewrites), `writing-rewriter` (Sonnet). Model pinned per agent, independent of the session model.
- `references/project-audit.md` — read-only repository audit with an explicit secret-file exclusion list, a shipped-versus-planned split, and `path:line` sourcing for every claim.
- `references/brainstorm.md` — post-audit brainstorm protocol: four questions per round, defaults proposed, three rounds maximum, style rules extracted from the user's own samples.
- `references/three-pass-pipeline.md` — pass mechanics, prompt contents, verdict loop capped at two rounds, fallback when the subagents are not loaded.
- `references/review-rubric.md` — P0/P1/P2 severity model, truth and risk checks first.
- `references/formats.md` — profiles for social post, landing copy, changelog, announcement, docs page, support reply, app store description.
- `references/cold-outreach.md` — deep profile: targeting signals, structure, single-ask ranking, personalisation boundaries, compliance requirements, follow-up limits.
- `assets/writing-context.template.md` — the per-project context file, with a quotable-proof table and an `ASSUMED` marker convention.
- Dual installation: plugin via marketplace, or clone into `~/.claude/skills/` where the bundled `.claude-plugin/plugin.json` loads the subagents.
- Documentation set: installation, usage, configuration, architecture, troubleshooting, limitations, privacy and security, contributing, security policy.

[1.2.0]: https://github.com/RAAAAAGEEEEE/three-pass-writing/releases/tag/v1.2.0
[1.1.0]: https://github.com/RAAAAAGEEEEE/three-pass-writing/releases/tag/v1.1.0
[1.0.0]: https://github.com/RAAAAAGEEEEE/three-pass-writing/releases/tag/v1.0.0
