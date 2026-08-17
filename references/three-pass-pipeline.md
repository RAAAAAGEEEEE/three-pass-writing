# The three-pass pipeline

One model drafts, a stronger model attacks the draft, the first model repairs it. The point is not "more passes" — it is that **the critic never writes and the writer never judges its own work**. A model reviewing its own output defends it; a model reviewing someone else's cuts it.

## Why the models are split this way

| Pass | Model | Why |
| --- | --- | --- |
| 1 Draft | Sonnet | Produces natural, rhythmic prose. Left alone it over-claims and over-explains. |
| 2 Review | Opus | Better at catching an unprovable claim, a wrong reader, a structural flaw. Left alone it writes correct, flat copy. |
| 3 Rewrite | Sonnet | Applies the fixes while keeping the voice. |

Each pass runs as a subagent: separate context, no memory of the others except what you paste in. That isolation is the mechanism — pass 2 must meet the draft as a stranger would.

## Launching the passes

Use your subagent tool (`Task` in Claude Code, `Agent` in some clients), one call per pass, sequentially. Never run them in parallel: each pass consumes the previous one's output.

```
subagent_type: "writing-drafter"   → then "writing-reviewer" → then "writing-rewriter"
```

If those subagent types do not resolve, the plugin is not loaded. Fall back to a general-purpose subagent, set `model` explicitly per pass (`sonnet`, `opus`, `sonnet`), and paste the corresponding file from `agents/` at the top of the prompt. Tell the user which path you took, in one line.

## What each prompt must contain

**Pass 1 — drafter**
- The full contents of `.claude/writing-context.md`
- The format profile from `references/formats.md`, or `references/cold-outreach.md`
- The brief: goal, reader, channel, length, constraints, deadline
- Any raw material the user supplied (notes, an old version, a transcript)

**Pass 2 — reviewer**
- Everything pass 1 received
- The draft, the three alternative openings, and the facts list, verbatim
- `references/review-rubric.md`

**Pass 3 — rewriter**
- The draft, verbatim
- The full findings list and the chosen opening
- The writing context and the format profile (for the banned list and the length target)

Never summarise a pass output before handing it to the next pass. Summarising is where fidelity dies.

## Loop control

```
draft → review → SHIP?     → done
                 RESTART?  → stop, report to the user, fix the brief
                 REWRITE?  → rewrite → review again (once)
                             SHIP?    → done
                             REWRITE? → stop, hand over text + open objections
```

Two full loops is the ceiling. Past that, the blocker is the brief or the proof, not the prose, and only the human can settle it.

## When to skip a pass

Skipping is allowed, and saying so is mandatory.

- **Trivial edit** (a subject line, a two-sentence reply, a tweak the user dictated): one pass, no pipeline. Announce it.
- **User supplied the text and wants a critique**: run pass 2 alone, return the findings, and ask before rewriting.
- **User explicitly asks for a fast draft**: pass 1 alone, and state plainly that it has not been reviewed.

Never skip pass 2 on anything that leaves the building — outreach, a published post, landing copy, a customer-facing announcement.

## Cost

Three subagent calls, one of them on a deep model, per piece. For a cold email that is fine. For fifty variants of a subject line it is not: draft the variants in a single pass 1, review them in a single pass 2, and rewrite only the survivors.
