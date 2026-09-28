# Changelog

All notable changes to this project are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.5.0] - 2026-09-28

### Added

- `SKILL.md`: two invariants, no em dash in the copy and no emoji unless the writing context allows them; a "Writing in French" section that hands the three passes the French rules of the `redaction` skill when it is installed.
- `references/review-rubric.md`: em dash and unallowed emoji are P1; French typography and French AI-writing tells are checked for text in French.
- `agents/writing-drafter.md`: the no-em-dash and no-emoji rules.

### Changed

- Every file a pass reads (`SKILL.md`, `agents/`, `references/`, `assets/`) is rewritten without em dashes, so the instructions no longer model the punctuation the copy must avoid.

## [1.4.0] — 2026-08-18

### Added

- `references/three-pass-pipeline.md` and `SKILL.md` — a review council for high-stakes pieces: four reviewers with one lens each (facts & risk, reader, structure & ask, voice) and an arbiter that merges duplicates, rejects findings that do not quote the text, resolves conflicts between lenses, and returns one ranked list and one verdict. Documented with its cost (6 calls, 5 on the deep model) and an explicit warning against adding lenses that duplicate an existing search.

## [1.3.0] — 2026-08-18

### Added

- `references/three-pass-pipeline.md` and `SKILL.md` — a panel mode for high-stakes pieces: three drafters in parallel on divergent angles, a judge that picks one and grafts the best lines from the losers, then one rewrite. Measured on a real cold email, the panel caught two P0 claims that two sequential review loops had missed, for roughly 6% more tokens. The guidance is explicit that more loops is the wrong answer to "make it better" — past two, the blocker is a missing fact or a human decision.

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

[1.5.0]: https://github.com/RAAAAAGEEEEE/three-pass-writing/releases/tag/v1.5.0
[1.4.0]: https://github.com/RAAAAAGEEEEE/three-pass-writing/releases/tag/v1.4.0
[1.3.0]: https://github.com/RAAAAAGEEEEE/three-pass-writing/releases/tag/v1.3.0
[1.2.0]: https://github.com/RAAAAAGEEEEE/three-pass-writing/releases/tag/v1.2.0
[1.1.0]: https://github.com/RAAAAAGEEEEE/three-pass-writing/releases/tag/v1.1.0
[1.0.0]: https://github.com/RAAAAAGEEEEE/three-pass-writing/releases/tag/v1.0.0
