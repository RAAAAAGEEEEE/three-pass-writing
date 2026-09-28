# Three-Pass Writing

A Claude Code skill that writes your product's copy the way a good team does it: someone drafts, someone else tears it apart, then it gets rewritten.

Three passes, three subagents, two models:

| Pass | Model | Job |
| --- | --- | --- |
| 1 | Sonnet | Draft |
| 2 | **Opus** | Critique, never rewrite |
| 3 | Sonnet | Apply the fixes, change nothing else |

The point is not "more passes". It is that a model reviewing its own output defends it, and a model reviewing someone else's cuts it.

## The problem it solves

Ask any model to write a cold email about your product and you get fluent copy that promises a feature you never shipped, quotes a number nobody measured, and opens with a line that would work for any product on earth.

This skill starts by reading **your repository** — what actually ships, what is still a TODO, which numbers have a source — then asks you the handful of questions the code cannot answer, and writes the result into a context file. Every piece of copy afterwards is grounded in that file. A claim with no source becomes `[[to confirm: ...]]` instead of a confident invention.

## Who it is for

Solo founders and small teams who write their own outreach, posts, landing copy and release notes, and who already work in Claude Code.

## Status

**v1.5.0, beta.** Manifests validated with `claude plugin validate`, installation reproduced from a clean state (see [docs/INSTALLATION.md](docs/INSTALLATION.md)), and the full pipeline run end to end on a live product — bootstrap, three passes, and the review loop stopping where it should. The prose quality depends on the context file the bootstrap builds with you: a thin bootstrap produces thin copy. Read [docs/LIMITATIONS.md](docs/LIMITATIONS.md) before relying on it.

## Requirements

- Claude Code v2.1.220 or later (checked with `claude --version`)
- Access to both Sonnet and Opus on your plan — the pipeline is built on the split
- A repository for the product you are writing about (or you get the brainstorm alone, without the audit)

## Install

As a plugin, from this repo used as its own marketplace:

```bash
claude plugin marketplace add RAAAAAGEEEEE/three-pass-writing
```

```bash
claude plugin install three-pass-writing@three-pass-writing
```

Or as a personal skill — clone into your skills folder, where the bundled `.claude-plugin/plugin.json` makes Claude Code load its three subagents too:

```bash
git clone https://github.com/RAAAAAGEEEEE/three-pass-writing ~/.claude/skills/three-pass-writing
```

Both paths are detailed, with the Windows variant and the verification step, in [docs/INSTALLATION.md](docs/INSTALLATION.md).

## Quickstart

From the repository of the product you want to write about:

```bash
claude
```

Then, in the session:

```
/three-pass-writing write a cold email to agency owners who just posted a hiring ad
```

First run in a project triggers the bootstrap: a read-only audit of the repo, a short brainstorm, and a `.claude/writing-context.md` file. Every later run reuses it.

## What a run looks like

```
Audit — 11 files read
PRODUCT     Turns a repo into grounded marketing copy — README.md:3
SHIPPED     three-pass pipeline, repo audit, 7 format profiles — SKILL.md:78
NOT SHIPPED variant A/B tracking — docs/LIMITATIONS.md:24
PROOF       0 external calls, 3 subagents per piece — SKILL.md:96
GAPS        who exactly is the reader? what may be quoted publicly?

[brainstorm: 4 questions, defaults proposed]

Pass 1 (sonnet) → draft, 3 alternative openings, 6 facts sourced
Pass 2 (opus)   → VERDICT: REWRITE — 1 P0, 2 P1
                  P0 "cuts writing time by 60%" → no source anywhere
Pass 3 (sonnet) → final, 118 words

FINAL
<copy-ready text>

STILL OPEN
[[to confirm: the 60% figure — measured, or dropped?]]
```

Illustrative example, not a recorded transcript.

## How it works

`SKILL.md` is the entry point and stays small. It routes to reference files that load only when needed: the audit procedure, the brainstorm protocol, the pipeline mechanics, the review rubric, the format profiles. The three passes run as subagents defined in `agents/`, each pinned to its model.

Details in [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Configuration

Everything project-specific lives in one file you own and can edit by hand: `.claude/writing-context.md`. The skill generates it, you correct it, your corrections win.

See [docs/CONFIGURATION.md](docs/CONFIGURATION.md).

## Security and privacy

The skill reads your repository and never opens `.env`, credential files, keys, or customer data. It makes no network call, sends no email, publishes no post, and commits nothing on its own. The context file it writes lands in your repo, so it is built to hold no secrets.

Full posture in [docs/PRIVACY_AND_SECURITY.md](docs/PRIVACY_AND_SECURITY.md).

## Limits

- It does not measure anything. No open rates, no A/B results, no analytics.
- It cannot verify a claim that exists nowhere in your repo — it flags it and asks you.
- Three subagent calls per piece, one on Opus. Not the tool for fifty subject-line variants.
- Cold outreach compliance guidance is not legal advice.

The honest list is in [docs/LIMITATIONS.md](docs/LIMITATIONS.md).

## Roadmap

Non-contractual, in rough order: a `/writing-context refresh` path that diffs the repo since the last audit; per-format sample libraries derived from the user's own accepted pieces; an eval set for the review rubric.

## Contributing

Issues and pull requests welcome — see [CONTRIBUTING.md](CONTRIBUTING.md).

## Licence

MIT — see [LICENSE](LICENSE).

## Documentation

- [Installation](docs/INSTALLATION.md)
- [Usage](docs/USAGE.md)
- [Configuration](docs/CONFIGURATION.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Troubleshooting](docs/TROUBLESHOOTING.md)
- [Limitations](docs/LIMITATIONS.md)
- [Privacy and security](docs/PRIVACY_AND_SECURITY.md)
- [Changelog](CHANGELOG.md)
