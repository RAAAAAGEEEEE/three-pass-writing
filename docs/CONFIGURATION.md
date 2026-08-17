# Configuration

There is one configuration file, and it lives in your project, not in this repo:

```
<your project>/.claude/writing-context.md
```

No environment variables, no settings file, no API keys.

## How it is created

The bootstrap writes it from [`assets/writing-context.template.md`](../assets/writing-context.template.md) after the audit and the brainstorm. You can also copy the template by hand and fill it yourself — the skill reads the file, it does not care who wrote it.

## Sections that matter most

| Section | Why it matters |
| --- | --- |
| **Shipped** / **Not shipped** | The single strongest guard against copy that promises a feature you never released. |
| **Quotable proof** | The whitelist of numbers. A number absent from this table cannot appear in any piece — it becomes `[[to confirm: ...]]`. |
| **Banned** | Words, constructions, and formatting habits you refuse. Pass 2 treats a banned word as P1, synonyms included. |
| **Voice** | Observable rules only — sentence length, person, punctuation, number placement. Adjectives like "punchy" produce nothing. |
| **Audience** + **Objections** | Decides whether a draft is aimed at the right reader. Wrong reader is a P1 no amount of style fixes. |
| **Open questions** | Anything listed here becomes a placeholder in the text instead of an invention. |

## Editing by hand

Encouraged. Your edits outrank anything the skill inferred, and correcting the file is cheaper than correcting every future draft. Two habits pay for themselves:

- Add to **Banned** every time a draft uses a word you dislike.
- Move a claim into **Quotable proof** the moment it has a source, so the pipeline stops asking.

## Marking assumptions

Anything the bootstrap could not verify is written as `ASSUMED`. The pipeline treats an `ASSUMED` line as unusable proof: it can inform tone and framing, never a factual claim. Remove the marker once you confirm the line.

## Committing it

It is plain markdown in your repo, so it gets committed if you commit it. That is usually what you want — the whole team writes from the same context.

Two consequences:

- **No secrets, ever.** No tokens, no customer names, no personal data. The skill refuses to put them there; do not add them by hand.
- **No unannounced plans.** If the repo is public, treat the file as public. Unreleased roadmap items belong somewhere else.

To keep it local, add it to `.gitignore`:

```gitignore
.claude/writing-context.md
```

## Multiple products in one repo

Monorepos: put a context file in each package directory that has its own product surface (`packages/api/.claude/writing-context.md`). Start Claude Code from that directory, or work on files inside it, so the right context loads.

## Refreshing after a product change

```
/three-pass-writing refresh the writing context
```

The audit runs again, the differences are shown against the current file, and nothing is overwritten before you agree. Worth doing after a release that changes what is shipped, a pricing change, or a repositioning.
