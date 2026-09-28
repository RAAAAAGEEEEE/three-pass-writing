# Project audit: read-only

Goal: learn enough about this product to write about it without lying. You are looking for **what is true and provable**, not for a marketing summary.

Budget: 10–20 file reads. Stop when the questions below are answered or clearly unanswerable from the repo. Unanswerable is a finding: it becomes a brainstorm question.

## Safety boundary

Never read: `.env`, `.env.*` except `.env.example`, `secrets/`, `credentials*`, `*.pem`, `*.key`, `id_rsa*`, database dumps, customer exports, `.git/config`.

Never copy into the context file: a token, key, password, connection string, internal URL, customer name, email address, or any personal data. If a useful number sits next to a secret, take the number, leave the rest.

If the repository is private and the copy is public-facing, assume anything you extract could end up published. Filter accordingly.

## What to read, in order

Stop early when a source answers a question well.

1. `README.md`: positioning, install, feature list, badges, metrics.
2. `CLAUDE.md`, `AGENTS.md`, `.cursorrules`, `CONTRIBUTING.md`: how the team talks about the project.
3. Manifest: `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `composer.json`: name, description, dependencies, scripts.
4. Marketing surfaces, if present: landing page, `pricing`, `app/(marketing)`, `content/`, `i18n/` locale files. These carry the current public promise and the current tone.
5. `CHANGELOG.md`, recent release notes, recent commit subjects (`git log --oneline -30`): what actually shipped, and how recently.
6. Product surface: routes, pages, CLI commands, public API endpoints, DB models. This is the real feature list.
7. `docs/`, especially limitations, FAQ, troubleshooting: the honest edges, and the objections users already raise.
8. Tests and benchmarks: the only numbers you can quote without asking.
9. `TODO`, `FIXME`, `WIP`, feature flags, stubs, `NotImplemented`: the boundary between shipped and promised.

## The live surface outranks the repository

If the product has a public site, check it before trusting the repo for anything a customer sees: price, plan names, trial length, the headline promise, what the plans include.

A repository routinely lags production. Marketing copy, price tables and locale files are edited in place on the live site, or shipped from a branch that never came back. Writing a price from a stale file is the single most expensive audit error, and nothing downstream catches it: the number looks sourced, because it is.

So: fetch the pricing and home page, record what they say with the date you checked, and record the disagreement itself as a finding. When the repo and production disagree, **production wins for anything customer-facing**, and the discrepancy goes to the user; it is usually a real bug they did not know they had.

Same rule for a public app store listing, a docs site, or a status page.

## What to extract

**Identity.** What the product does, in one sentence, using the words the repo uses. If the README's sentence and the code disagree, the code wins and the disagreement is a finding.

**Audience.** Who it is for, and the evidence: pricing tiers, docs entry points, supported locales, integrations, the persona named in marketing copy. Distinguish stated audience from the audience the feature set actually serves.

**Shipped versus planned.** Two explicit lists. This is the single most valuable output of the audit: it is what stops the pipeline from promising a feature that does not exist.

**Quotable proof.** Numbers with a source: benchmark output, test coverage, pricing, plan limits, supported formats, response times, user counts if they appear in a committed file. Each one gets `path:line`, or the live URL and the date you checked it. No source means it does not exist for writing purposes.

**Vocabulary.** The nouns and verbs the product uses for its own concepts, and the ones it avoids. Copy that renames the product's own concepts reads as written by an outsider.

**Constraints.** Licence, compliance surface (payments, health, personal data, minors), disclaimers already in use, and any wording the repo standardises on.

**Competitive framing**, only if the repo states it. Never infer a competitor comparison.

**Voice sample.** The two or three paragraphs in the repo that sound most like a human wrote them deliberately, usually the README intro or a release note. They are the starting hypothesis for the tone.

## Output

At most 15 lines to the user. Every claim ends with `path:line`.

```
PRODUCT     <one sentence, repo vocabulary> (README.md:3)
AUDIENCE    <who> (<source>)
SHIPPED     <5 items max> (<sources>)
NOT SHIPPED <what is promised but absent> (<sources>)
PROOF       <quotable numbers> (<sources>)
TONE        <two adjectives + the file that shows it>
GAPS        <what the repo cannot tell you>
```

`GAPS` drives the brainstorm. A short gap list after a shallow audit is not a good audit; it is an incomplete one.
