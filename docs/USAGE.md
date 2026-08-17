# Usage

## First run in a project

Start Claude Code from the repository of the product you are writing about, then:

```
/three-pass-writing
```

With no `.claude/writing-context.md` present, the skill runs the bootstrap:

1. **Audit** — read-only pass over your repo. Produces a 15-line summary where every claim carries a `path:line`, plus a list of gaps.
2. **Brainstorm** — at most three rounds, four questions per round, each with a proposed default so `ok` is a complete answer. The one question worth answering carefully: *send me one or two things you've written that sound right, and one that doesn't.*
3. **Context file** — written to `.claude/writing-context.md`. Read it. Correct it. Your edits outrank anything the skill inferred.

Budget ten minutes. It happens once per project.

## Writing

```
/three-pass-writing write a cold email to agency owners who just posted a hiring ad
```

```
/three-pass-writing a changelog entry for the batch export we shipped this week
```

```
/three-pass-writing rewrite the hero section, the current one is too abstract
```

The skill can also trigger on its own when you ask for copy in a project that has a context file.

Give it the goal and the reader. It fills the rest from the context file and asks at most two questions for what is genuinely missing.

## Reviewing text you already wrote

```
/three-pass-writing review this before I send it: <paste>
```

Runs pass 2 alone and returns the findings. It will ask before rewriting — a critique you can act on yourself is often what you wanted.

## Reading the output

Three blocks, always in this order:

1. **The final text**, copy-ready, nothing interleaved.
2. **What pass 2 caught** — P0 and P1 only, five lines maximum.
3. **Open decisions** — every `[[to confirm: ...]]` left in the text, and any claim you have to verify before sending.

`[[to confirm: ...]]` means the pipeline needed a fact you never gave it. It refused to invent one. Supply it or cut the sentence — never ship the brackets.

## Severity, in one line each

- **P0** — false, unverifiable, or unshipped claim; a legal or medical promise. Blocking.
- **P1** — promise without proof, generic opening, doubled CTA, wrong reader, over length. Fix before sending.
- **P2** — rhythm, repetition, weak verbs. Your call.

## Verdicts

- `SHIP` — pass 3 is skipped. Nothing was found worth fixing, and no work is manufactured to look busy.
- `REWRITE` — the normal path.
- `RESTART` — the brief or the angle is wrong. The skill stops and tells you what to change rather than polishing something aimed at the wrong person.

Two review loops maximum. Past that, the blocker is the brief or the proof, and only you can settle it.

## Refreshing the context

After a product change that matters:

```
/three-pass-writing refresh the writing context, we shipped the API and dropped the free plan
```

Re-runs the audit, shows what changed against the current file, and asks before overwriting.

## Habits that make it work

- **Feed it proof.** Every number you paste into the brainstorm is a number the copy can use. Every number you leave out becomes a placeholder.
- **Correct the context file by hand.** It is the highest-leverage file in the loop; two minutes there beats twenty in rewrites.
- **Keep the banned list growing.** Every time a draft uses a word you hate, add it.
- **Do not batch.** Fifty subject-line variants through three passes is the wrong tool — ask for them in a single draft pass instead.
