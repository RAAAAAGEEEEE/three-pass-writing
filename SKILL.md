---
name: three-pass-writing
description: Use when the user wants to write, draft, rewrite or improve any user-facing text for their own product: cold outreach email, X/LinkedIn post, landing page copy, changelog, release note, docs page, support reply, app store description. Runs a three-pass pipeline (fast-model draft, deep-model critique, fast-model rewrite) grounded in a read-only audit of the user's own repository plus a short brainstorm, so the copy only claims what the product actually does. For text in French, also load the `redaction` skill if installed (French typography, AI-writing tells). Trigger on "write an email", "cold outreach", "write a post", "rewrite this copy", "improve this text", "landing page copy", "changelog entry", "make this punchier", "draft an announcement". Also trigger when the user asks to set up, refresh or audit their writing context.
license: MIT
compatibility: Designed for Claude Code v2.1.220 or later, which runs the three passes as subagents pinned to different models (Sonnet, Opus, Sonnet). In an agent without subagents the passes run inline and the model split is lost. Needs read access to the project repository. No network access.
metadata:
  author: Anto1nx
  version: "1.5.1"
  repository: https://github.com/RAAAAAGEEEEE/three-pass-writing
  recommended-effort: high
---

# Three-Pass Writing

Write in the user's language. Detect it from their message; if mixed, ask once.

Never send, post, publish or commit anything. This skill produces text. The human ships it.

## Two modes

Read `.claude/writing-context.md` in the current project first.

| State | Mode |
| --- | --- |
| File missing | **Bootstrap** (audit + brainstorm), then Writing |
| File present, user asked to write | **Writing** |
| File present but user asks to refresh, or the product changed materially | **Bootstrap** in update mode |

If the user asks for text and the file is missing, say so in one line, run Bootstrap, then write. Do not skip Bootstrap: ungrounded copy is the failure this skill exists to prevent.

If the current directory is not the user's product repository (no code, no README), skip the audit, tell the user, and run the brainstorm alone.

---

## Bootstrap

### Step 0: Brief the user, before reading anything

The skill is about to read their repository. Nobody should discover that after the fact. Show this first, adapted to their language, and **wait for a yes**. Do not open a single file before they answer.

> Avant de commencer, voici ce qui va se passer :
>
> **1. J'audite ce dépôt, en lecture seule** : README, doc, page d'accueil, tarifs, changelog, routes, tests. Une quinzaine de fichiers, pas plus. Je vérifie aussi votre site public s'il existe : un dépôt est souvent en retard sur la production, surtout sur les prix.
> **2. Je ne lis jamais** vos `.env`, clés, identifiants ou données clients. Aucun secret n'entrera dans le fichier produit.
> **3. Je vous pose ensuite quelques questions** : 4 à la fois, avec une proposition par défaut, pour cerner votre lecteur, votre promesse et votre ton. 10 minutes, une seule fois pour ce projet.
> **4. J'écris `.claude/writing-context.md`** dans ce dépôt. Vous pouvez le corriger à la main, vos corrections priment. Il sera committé si vous committez : ni secret, ni plan non annoncé.
> **5. Je n'envoie et ne publie jamais rien.** Je produis du texte, vous décidez de ce qui part.
>
> On est bien dans le dépôt du produit dont vous voulez parler ? On y va ?

Two things this catches, both common: the session was started in the wrong directory, and the user did not realise a context file would land in their repository.

If they decline the audit but still want copy, run the brainstorm alone and say plainly that the result rests on their answers, with nothing verified against the code.

### Step 1: Audit (read-only)

Follow `references/project-audit.md`.

Read the repository and extract what is **verifiable**: what the product does, who it is for, what exists versus what is planned, which numbers can be quoted, which words the product actually uses.

Hard rules:
- Never open `.env`, `.env.local`, credential files, private keys, customer data dumps, or `.git/config` credentials. `.env.example` is fine.
- Never copy a secret, token, key, customer name, or personal data into the context file.
- Every claim in the audit summary carries a `path:line` source. No source, no claim.

Present a summary of at most 15 lines: what the product is, who it serves, quotable proof, vocabulary, and, most important, **the gaps** you could not resolve from the repo.

### Step 2: Brainstorm

Follow `references/brainstorm.md`.

The audit tells you what the product is. The brainstorm tells you what the user wants said about it, and how. Do not skip it and do not turn it into a generic questionnaire.

- Ask in batches of at most 4 questions, driven by the gaps you just found.
- Every question carries a default you propose, so "ok" is a valid answer.
- Ask for 1–3 samples of writing the user considers good, and 1 they consider bad. Extract observable rules from them (sentence length, person, punctuation, use of numbers), not adjectives.
- Stop when you can answer, in the user's words: who is this for, what do we promise, what proves it, what must never appear.

### Step 3: Write the context file

Fill `assets/writing-context.template.md` and save it to `.claude/writing-context.md` in the user's project.

Tell the user the file is plain text in their repo: it gets committed if they commit it, so it must contain no secrets and no unreleased plans they would not want public. Ask before committing it yourself.

---

## Writing

### Inputs

Before drafting, you need: the format, the audience, the goal, and any hard constraint (length, channel, deadline, banned words). Take what the user gave you, fill the rest from `.claude/writing-context.md`, and ask only for what is still genuinely missing, at most 2 questions.

Load the matching format profile from `references/formats.md`. For cold outreach, load `references/cold-outreach.md` instead: it is the deepest profile.

### The three passes

Run each pass as a **subagent**, so each one gets its own model and a clean context.

| Pass | Subagent | Model | Job |
| --- | --- | --- | --- |
| 1 | `writing-drafter` | Sonnet | Produce the draft |
| 2 | `writing-reviewer` | Opus | Find what is wrong. Never rewrite |
| 3 | `writing-rewriter` | Sonnet | Apply the fixes. Change nothing else |

Launch each with your subagent tool (`Task` in Claude Code, `Agent` in some clients), passing `subagent_type: "writing-drafter"` and so on. If the subagent type is not found, the plugin is not loaded: fall back to a general-purpose subagent and set the model override explicitly (`model: "sonnet"` / `model: "opus"`), and paste the matching file from `agents/` into the prompt. Say in one line which path you took.

Each subagent prompt must give access to: `.claude/writing-context.md`, the format profile, the brief and (for passes 2 and 3) the previous pass output verbatim. Paste the brief and the previous pass output; for the context file and the reference files, an absolute path the subagent reads itself works as well and costs less. Never paraphrase a pass output before handing it over.

Details, including what each pass returns, are in `references/three-pass-pipeline.md`.

### When the piece is worth more than three passes

A campaign, a home page, a one-shot announcement: run three drafters in parallel on genuinely different angles, have the reviewer judge them against each other and graft the best lines into the winner, then rewrite once. Five calls, and it catches more than a second sequential loop does. See `references/three-pass-pipeline.md`.

When being wrong is expensive (a campaign going to thousands, a home page, anything with a legal surface), widen the *checking* instead: four reviewers, one lens each (facts & risk, reader, structure & ask, voice), plus an arbiter that merges them into one ranked list and one verdict. Same file.

Never answer "make it better" with more loops. Past two, the blocker is a missing fact or a decision only the user can make.

### Verdict handling

Pass 2 returns a verdict: `SHIP`, `REWRITE`, or `RESTART`.

- `SHIP`: skip pass 3. Say so; do not manufacture work.
- `REWRITE`: run pass 3 with the fix list.
- `RESTART`: the brief or the angle is wrong. Do not rewrite. Report the reason to the user and ask before spending another pass.

After pass 3, re-run pass 2 **once** at most. If the second verdict is still `REWRITE`, stop and hand the user the text plus the unresolved objections. Two full loops is the ceiling.

### Output to the user

1. The final text, in a copy-ready block, and nothing interleaved with it.
2. What pass 2 caught: at most 5 lines, the P0 and P1 items only.
3. Open decisions: every `[[to confirm: ...]]` placeholder still in the text, and any claim the user must verify before sending.

Keep your own commentary shorter than the text you produced.

---

## Invariants

These hold in every mode. Violating one is a failure of the skill, not a style choice.

- **No invented facts.** A number, a customer name, a result, an integration, a feature: if it is not in the writing context, in the repo with a source, or supplied by the user, it does not go in the text. Write `[[to confirm: metric]]` instead and list it as an open decision.
- **No invented capability.** Never describe a feature the audit marked as planned, stubbed, or TODO as if it shipped.
- **One CTA.** Every piece of text asks for exactly one thing.
- **Nothing is sent.** No email is sent, no post published, no PR opened, no file committed, without an explicit request from the user in the current turn.
- **Legal, medical, and financial claims** get flagged to the user rather than smoothed over.
- **Honest limits.** If the product genuinely does not solve the reader's problem, say that to the user rather than writing around it.
- **No em dash.** The em dash (—) never appears in the copy. Use a comma, a colon, parentheses or a period, chosen from the sentence. Only a verbatim quotation keeps one; an en dash stays fine in a range (9–12).
- **No emoji** unless the writing context allows them.

## Writing in French

When the text is in French and the `redaction` skill is installed (`~/.claude/skills/redaction/`), give all three passes the absolute paths of its `references/anti-ia.md` and `references/typographie-style.md`, and run its `scripts/verifier.py` on the final text before handing it over. Its hard rules match the invariants above. Without it, French typography still applies: « » quotes, a space before `: ; ! ?`, no Title Case in headings.

## Reference files

| File | Load when |
| --- | --- |
| `references/project-audit.md` | Bootstrap step 1 |
| `references/brainstorm.md` | Bootstrap step 2 |
| `references/three-pass-pipeline.md` | Any writing request |
| `references/review-rubric.md` | Pass 2, and whenever you judge text yourself |
| `references/formats.md` | Any writing request |
| `references/cold-outreach.md` | Cold email, DM, or any first-contact message |
| `assets/writing-context.template.md` | Bootstrap step 3 |
