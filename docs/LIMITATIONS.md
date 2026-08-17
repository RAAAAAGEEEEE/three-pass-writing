# Limitations

Honest list. Read it before relying on the skill.

## It cannot verify what your repo does not contain

The audit reads code, docs, and committed files. Sales numbers, customer quotes, conversion rates, anything living in a CRM, a dashboard, or your head — invisible to it. Those become brainstorm questions, and if you skip them, `[[to confirm: ...]]` placeholders.

## It measures nothing

No open rates, no reply rates, no A/B testing, no analytics integration. It never learns whether a piece worked. The review is a judgement against a rubric, not evidence from the field. Any claim that this method improves a metric would need data this skill does not collect.

## Quality depends on the context file

A ten-minute bootstrap with real samples and real numbers produces copy worth sending. A rushed one produces grounded but generic copy. The skill cannot compensate for an empty **Quotable proof** table.

## Pass 2 is a good reviewer, not an oracle

It catches unsourced claims, wrong readers, structural flaws, and banned words reliably, because those are checkable. It cannot tell you whether your positioning is right, whether the market wants this, or whether the reader will care. A `SHIP` verdict means "nothing verifiably wrong", not "this will work".

## Style transfer is approximate

Two samples get the register close, not exact. The gap narrows as you correct the **Voice** section, and it never closes completely. Expect to edit the final text — the skill's job is to make that edit small.

## Cost

Three subagent calls per piece, one of them on Opus, each carrying the full context file. Fine for a cold email; wasteful for fifty subject-line variants, which should be drafted and reviewed in batch instead.

## Model dependency

The pipeline assumes access to both Sonnet and Opus. Without Opus, pass 2 runs on whatever the session offers and loses most of its value — a fast model reviewing a fast model's output largely agrees with it.

## No publishing, by design

It does not send email, post to social platforms, open pull requests, or commit files on its own. If you want automation around it, you build that separately — and you own what goes out.

## Compliance guidance is not legal advice

`references/cold-outreach.md` lists practices that keep a message defensible (real identity, working opt-out, truthful subject, a stated reason for contact). Cold outreach law differs by jurisdiction and changes. For a real compliance question, ask a lawyer, not a skill.

## English-first internals

`SKILL.md` and the reference files are written in English; the skill writes in whatever language you use. Nothing breaks, but if you maintain a fork in another language, expect the reference files to drift from upstream.

## Single-product assumption

One context file describes one product. Monorepos need one file per product surface, and the skill will not detect on its own that you switched products mid-session.

## Not evaluated

There is no eval set for the review rubric yet. Nobody has measured how often pass 2 catches a planted false claim. It is on the roadmap; until then, treat the review as a strong reader, not a verified filter.
