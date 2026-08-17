# Review rubric

Used by pass 2, and by you whenever you judge text without running the pipeline.

Check in this order. The first checks kill drafts; the last ones polish them. Never spend the review on rhythm while an unverifiable claim sits in paragraph one.

## 1. Truth (P0)

- Every factual claim traced to the writing context, the repository with a `path:line`, or the user's own brief.
- Every number sourced. "Faster", "most", "many" are claims too — sourced or cut.
- Every named customer, logo, partner, or integration confirmed as real and quotable.
- Product, person, and company names spelled as they are spelled.
- No feature described as shipped that the context lists as planned, stubbed, flagged, or TODO.
- No implied certification, compliance, or endorsement the product does not hold.

A claim that is *probably* true is a P0. Plausibility is exactly the failure mode of a fast first draft.

## 2. Risk (P0)

- Legal, medical, financial, health, or employment promises → flag, never smooth.
- Guarantees of outcome ("you will get X") → flag.
- Comparative claims about a named competitor → flag unless the context supplies proof.
- Anything that would embarrass the sender if quoted publicly, or that leaks internal information.
- Personal data about the recipient beyond what they published themselves.

## 3. Reader fit (P1)

- The piece addresses the reader in the brief, not a generic buyer.
- The first line survives the reader's first three seconds. If it could open any other product's message, it fails.
- The reader's actual objection is met somewhere in the piece.
- Jargon is either the reader's own vocabulary or explained on first use.
- The piece assumes only what the reader already knows.

## 4. Structure (P1)

- The main idea appears before the reader has to decide whether to keep reading.
- The format profile's structure is followed, or its deviation is justified.
- Exactly one call to action, concrete and easy: what to do, and what happens next.
- Within the length target. Over target is a P1 and cutting is the fix, not compressing whitespace.
- One idea per paragraph; no paragraph exists only to transition.

## 5. Voice (P1 or P2)

- Banned words and constructions absent, including obvious synonyms — P1.
- Tone matches the context rules, not a generic professional register — P1.
- Observable style rules from the samples respected: sentence length, person, punctuation, number placement — P2 unless the deviation is systematic.
- No filler opener, no throat-clearing, no closing apology.
- No LLM tells: triads everywhere, "it's not just X, it's Y", "in today's landscape", symmetrical paragraph lengths, an adjective before every noun.

## 6. Craft (P2)

- Strong verbs; adverbs deleted where the verb can carry the weight.
- No repeated word or construction within a few lines unless the repetition is deliberate.
- Concrete over abstract: a number, a name, an object beats a category.
- The last line lands rather than trails off.

## Calibration

Two failure modes, both real:

- **Inflation** — manufacturing P1s to look thorough. It burns a rewrite pass on nothing and trains the user to ignore findings.
- **Deference** — nodding at a draft because it reads well. Fluent copy is exactly the kind that smuggles an unprovable claim past a tired reader.

A well-written draft with one invented number is `REWRITE`, and that number is P0. A dull draft that is entirely true and correctly aimed can be `SHIP` with P2s listed.
