# Step 0 — Subjective Term Check

```
─── GATE: ENTRY POINT ───
This is the entry step. No upstream gate.
Confirm before reading further:
☐ Skill triggered by STEM-relevant query (or user re-evaluation request)
If query is plainly non-STEM AND not a re-evaluation → exit skill with brief skip statement.
```

## Purpose

Before field assignment, scan the claim for qualitative terms doing causal or explanatory work. Subjective terms smuggle conclusions into premises by hiding the definitional choice. Verifying a claim built on undefined qualitative grounding produces verdicts that look empirical but are actually downstream of the user's unstated definition. **Forcing operationalization upstream prevents this category of error.**

## Inputs

- User query, observation, or claim (current turn)

## Procedure

**1. Scan for subjective terms.** Terms whose meaning is contested, value-laden, or undefined in standard academic register. Examples: *incompetent, extreme, ineffective, successful, fair, broken, natural, rational, optimal, sensible, reasonable, common-sense, obvious*. These cannot be falsified without an operationalization specifying what is being measured and against what baseline.

**2. Apply the 3-state decision.**

- **Full-block.** The entire claim depends on the subjective term — no falsifiable sub-claims remain after the term is removed. **Do not proceed with the verification procedure.** Return to user with: *"To evaluate this, I need a falsifiable definition of [term]. What measurable property are you using?"* The skill exits here.

- **Partial.** The claim contains both subjective terms AND falsifiable sub-claims. Proceed with the procedure on the falsifiable parts only. **Flag the subjective term explicitly in the final output:** *"This part of the claim contains [term], which I'm treating as undefined. The conclusion may not survive once [term] is operationalized."*

- **Proceed.** No subjective terms detected. Move to S1.

**3. Do not infer operationalization on the user's behalf.** If the user says "X is broken," do not silently translate to "X exhibits failure mode Y" and proceed. The translation is the user's job. Translating on their behalf reintroduces the smuggling problem at a deeper layer.

## Output (required emission)

```
[STEP 0 — SUBJECTIVE_TERMS: <list of detected terms | none>]
[STEP 0 — DECISION: full-block | partial | proceed]
```

For **full-block**: emit the operationalization request to the user. The skill terminates. Do not read further.

For **partial**: emit the flag block that will appear in the final output. Proceed to S1, but mark `[STEP 0 — PARTIAL_FLAG]` for downstream visibility.

For **proceed**: emit the decision. Move to S1.

## Forward-compat

S1 reads `[DECISION]` to determine which sub-claims are falsifiable (S1 only operates on falsifiable parts in partial mode). S6 final output includes the partial-flag if it was raised at S0. The full-block exit terminates the skill before any field tagging or web_search occurs — this saves tokens and prevents the user from receiving a verdict on an unfalsifiable foundation.
