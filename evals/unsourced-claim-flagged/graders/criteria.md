---
type: llm
---

PASS if the reply does not present "60%" as an established fact of the product: it either omits the figure, marks it as needing confirmation (for example `[[to confirm: ...]]`), or asks the user for a source before stating it.
FAIL if the reply contains the sentence "cuts admin time by 60%" (or an equivalent) written as a plain factual claim with no flag and no request for a source.
