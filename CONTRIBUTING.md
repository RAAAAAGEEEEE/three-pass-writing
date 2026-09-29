# Contributing

Issues and pull requests welcome.

## What is most useful

1. **A format profile that is missing** — you write something regularly that `references/formats.md` does not cover well.
2. **A review check that caught a real mistake** — something pass 2 should have flagged and did not. These are the highest-value contributions: the rubric is the core of the skill.
3. **An audit source worth reading** — a file or convention that reveals what a product actually ships, in an ecosystem the audit currently misses.
4. **A false positive** — a case where pass 2 blocks copy that was fine. Over-flagging trains users to ignore findings.

## What will be declined

- Anything that makes the skill send, post, publish, or commit on its own.
- Deliverability tricks: sender disguise, filter evasion, faked prior relationships, tracking the recipient did not expect.
- Instructions that would put customer data, personal data, or secrets into the context file.
- Persuasion patterns built on false urgency or manufactured social proof.
- Bloating `SKILL.md`. Its body stays in context for a whole session; new procedure belongs in `references/`.

## House rules for the prose

This is a writing skill, so its own text is held to its own rubric.

- Instructions state what to do, not why it is important.
- Observable rules over adjectives. "Sentences under 12 words" is usable; "punchy" is not.
- No claim the repository cannot back. No performance or improvement numbers without a cited source.
- No promises about results.

## Before opening a pull request

Validate the manifests:

```bash
claude plugin validate . --strict
```

If you change behaviour the evals describe, update the matching case in `evals/<case>/` (a `prompt.md` and a `graders/` folder, see the [plugin evals reference](https://code.claude.com/docs/en/plugin-evals)). Run them with `claude plugin eval .` when your Claude Code build offers it.

Install the branch locally and run one real piece through it end to end:

```bash
claude plugin marketplace add /absolute/path/to/your/clone
```

Say in the pull request what you actually ran and what came out. A change to `references/review-rubric.md` that was never run against a real draft is not reviewable.

If behaviour changes, update the documentation in the same commit — the file that describes it, and `CHANGELOG.md`. Documentation drift is treated as a defect here, not a follow-up task.

## Commits

Present tense, one concern per commit, and a subject that says what changed for the user rather than which file moved.
