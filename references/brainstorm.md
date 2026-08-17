# Brainstorm — turning the audit into the user's rules

The audit found what the product is. The brainstorm finds what the user wants said, to whom, and in whose voice. Skipping it produces copy that is accurate and useless.

This is a conversation, not a form. Run it in the user's language.

## Rules of engagement

- **At most 4 questions per message.** More is a form, and people abandon forms.
- **Every question carries a proposed default**, drawn from the audit, so `ok` is a complete answer. Format: `Question? (my read: X)`.
- **Never ask what the audit already answered.** Asking "what does your product do" after reading the repo destroys the user's trust in the audit.
- **Follow the answer, not the script.** A surprising answer is worth two follow-ups; a confirmed default is worth none.
- **Three rounds maximum.** If it is still fuzzy after that, write the context file with explicit assumptions marked `ASSUMED` and move on. The file is editable.

## Round 1 — the gaps that block writing

Pick the four that matter most for this product, from the audit's `GAPS`:

- Who exactly is the reader? Not a segment, a person: their role, what they are doing that day, what they already tried.
- What is the one thing you want them to believe after reading? One sentence.
- What proves it? Which numbers, demos, screenshots, or customer facts may be quoted, and which may not.
- What is the ask? One action, and what happens right after they take it.

## Round 2 — voice and boundaries

- **Samples.** "Send me one or two things you've written that sound right, and one that doesn't." This is the highest-value question in the whole brainstorm. From the good samples, extract observable rules only: average sentence length, first or second person, use of questions, whether numbers appear early, punctuation habits, emoji or none, formatting habits (lists, bold, line breaks). Write down rules, never adjectives — "sentences under 12 words, no metaphors, one number in the first two lines" is usable; "punchy and authentic" is not.
- **Banned list.** Words, phrases, and constructions that must never appear. Push for specifics: most people have a list they have never written down. Common triggers: "revolutionary", "seamless", "unlock", "in today's world", "game-changer", "I hope this email finds you well", em-dash-heavy rhythm, three-item lists everywhere.
- **Hard constraints.** Length, channel limits, compliance wording, mandatory disclaimers, legal review requirements.
- **Objections.** The two or three things a reader says to reject this product. Copy that ignores them fails silently.

## Round 3 — only if needed

- Competitors and how the user positions against them (only what they will state, never inferred).
- What the product deliberately does *not* do, and how to say it.
- The formats they will ask for most often — that decides which format profiles matter.
- Language and localisation: write in French, English, both? Same tone in each?

## Closing the brainstorm

Read back what you understood in under 10 lines and ask for corrections before writing the file. Cheap to fix now, expensive later — the context file drives every future piece of writing.

Then write `.claude/writing-context.md` from `assets/writing-context.template.md`, mark anything you assumed as `ASSUMED`, and tell the user:

- where the file is and that they can edit it by hand;
- that it will be committed if they commit their repo, so no secrets and no unannounced plans belong in it;
- that they can ask for a refresh after a product change.
