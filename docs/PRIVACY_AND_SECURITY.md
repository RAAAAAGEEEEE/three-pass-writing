# Privacy and security

## What the skill is

Markdown instructions. No scripts, no binaries, no dependencies, no network calls of its own. Everything it does happens through Claude Code's own tools, under your own permission settings — it can do nothing you have not already allowed Claude Code to do.

## What it reads

During the audit, in your project only: `README.md`, agent instruction files, package manifests, marketing and docs pages, changelog, recent commit subjects, routes and models, tests and benchmarks, TODO/FIXME markers.

Ten to twenty files, chosen to answer specific questions. It does not crawl the whole repository.

## What it never reads

`.env` and every `.env.*` except `.env.example`, `secrets/`, `credentials*`, `*.pem`, `*.key`, `id_rsa*`, database dumps, customer exports, `.git/config`.

This is an instruction in `references/project-audit.md`, enforced by the model following it — not a sandbox. If you handle regulated data, use Claude Code's own [permission rules](https://code.claude.com/docs/en/permissions) to deny those paths at the tool level. That is the enforceable boundary.

## What it writes

One file: `.claude/writing-context.md` in your project. Nothing else, unless you explicitly ask for a file.

It never commits, never pushes, never opens a pull request on its own.

## The context file is public if your repo is

Plain markdown, committed like any other file. The skill is built so it never needs a secret: no tokens, no keys, no customer names, no personal data, no internal URLs. Numbers carry a source path rather than the raw data behind them.

Before committing it the first time, read it once. If your repo is public, treat the file as public — including unreleased plans you would rather not announce.

To keep it local:

```gitignore
.claude/writing-context.md
```

## What leaves your machine

The same thing that leaves during any Claude Code session: the content of your prompts and the files the model reads, sent to Anthropic's API under your own account and its data policy. The skill adds no other destination — no telemetry, no analytics, no third-party service, no webhook.

Three subagent calls per piece means the context file and the draft are sent as part of those prompts. If a section of the context file is too sensitive for that, it is too sensitive for the file.

## Recipient data, in outreach

`references/cold-outreach.md` restricts personalisation to what the recipient published themselves — posts, docs, job ads, pricing pages. It rules out aggregated personal details, data-broker sources, and anything that would make a reader ask how you know it.

The skill will not write messages that disguise the sender, fake a prior relationship, or evade spam filtering.

## Nothing is sent

No email, no post, no message, no publication, ever, without an explicit request from you in that turn. The skill produces text; shipping it is a human action, deliberately.

## Reporting a problem

Security issues: see [SECURITY.md](../SECURITY.md). Never paste a real `.claude/writing-context.md`, a real prospect list, or a real customer email into a public issue.
