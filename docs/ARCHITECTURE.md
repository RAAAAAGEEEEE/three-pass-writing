# Architecture

## Layout

```
three-pass-writing/
├── SKILL.md                          entry point, kept small — it stays in context
├── agents/
│   ├── writing-drafter.md            pass 1 — model: sonnet
│   ├── writing-reviewer.md           pass 2 — model: opus
│   └── writing-rewriter.md           pass 3 — model: sonnet
├── references/                       loaded on demand, not at startup
│   ├── project-audit.md
│   ├── brainstorm.md
│   ├── three-pass-pipeline.md
│   ├── review-rubric.md
│   ├── formats.md
│   └── cold-outreach.md
├── assets/
│   └── writing-context.template.md
├── .claude-plugin/
│   ├── plugin.json                   makes the folder load as a plugin
│   └── marketplace.json              makes the repo installable as a marketplace
└── docs/
```

## Two states, one file

Everything hinges on `.claude/writing-context.md` in the **user's** project, not in this repo.

```
/three-pass-writing
        │
        ├── context file missing ──► BOOTSTRAP
        │                             audit (read-only) → brainstorm → write context
        │                                                                    │
        └── context file present ◄──────────────────────────────────────────┘
                    │
                    ▼
                 WRITING
```

That file is the boundary between the generic skill and one specific product. The skill ships no assumptions about any product; the file holds all of them.

## The pipeline

```
brief + context + format profile
        │
        ▼
   pass 1  writing-drafter   (sonnet)  → draft + 3 openings + facts + placeholders
        │
        ▼
   pass 2  writing-reviewer  (opus)    → verdict + severity-ranked findings
        │
        ├── SHIP    → done
        ├── RESTART → stop, report, fix the brief
        └── REWRITE
                │
                ▼
   pass 3  writing-rewriter  (sonnet)  → final + change log + unapplied
                │
                ▼
        pass 2 again, once at most
```

Each pass is a **subagent**: its own context window, no memory of the others beyond what the orchestrator pastes in. That isolation is the whole design. Pass 2 has to meet the draft the way a stranger would, or it inherits the drafter's blind spots along with its reasoning.

The orchestrator (your main session) carries the state between passes and never edits the text itself.

## Why two models

Sonnet writes prose with rhythm; unchecked, it over-claims and over-explains. Opus is better at catching an unprovable claim, a wrong reader, a structural flaw; unchecked, it writes correct, flat copy. Splitting the roles uses each where it is strong and constrains it where it is not — the reviewer is explicitly forbidden from rewriting, the rewriter from inventing.

Model pinning happens in the `model:` frontmatter of each file in `agents/`. It does not depend on the session model, which is why the plugin manifest matters even in the clone-into-skills install: without `.claude-plugin/plugin.json`, the subagents never load and every pass runs on whatever model the session happens to use.

## Progressive disclosure

`SKILL.md` stays around 150 lines because a loaded skill's body remains in context for the rest of the session. Procedures live in `references/` and load only when the relevant step runs — the audit procedure never enters context during a routine writing request, and the cold-outreach profile never loads when you are writing a changelog.

## Extension points

- **A new format** — add a section to `references/formats.md`, or a dedicated file when it deserves the depth `cold-outreach.md` gets.
- **Stricter review** — edit `references/review-rubric.md`. Severity levels drive the loop, so moving a check from P2 to P1 changes what blocks a send.
- **A different pipeline** — the agent files are independent. Swapping models, or adding a fourth pass, means editing `agents/` and the table in `SKILL.md`.
- **Project-specific rules** — put them in `.claude/writing-context.md`, not in the skill. The skill is meant to stay generic and updatable.

## What it deliberately does not do

No network calls, no scripts, no dependencies, no state outside the user's context file. The skill is instructions and reference text — everything it does is done by Claude Code's own tools, under the user's own permissions.
