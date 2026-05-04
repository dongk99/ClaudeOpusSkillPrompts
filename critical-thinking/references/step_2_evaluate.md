# Step 2 — Evaluate the Claim

```
─── GATE: VERIFY S0–S1 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 0 — DECISION] emitted (proceed or partial)
☐ [STEP 1 — FIELDS] emitted
☐ [STEP 1 — CLAIM_OPERATIONALIZED] emitted in standard form
☐ [STEP 1 — INITIAL_SEARCH] emitted with QUERIES, SOURCES_RETAINED, RESULT_SUMMARY
If unchecked → STOP. Return to the missing step.
```

## Purpose

Evaluate the operationalized claim against the S1 search results. Quantitatively where data permits; qualitatively otherwise, using domain-specific language drawn from the taxonomy.

## Inputs

- `[STEP 1 — CLAIM_OPERATIONALIZED]`
- `[STEP 1 — INITIAL_SEARCH]` (queries, sources, result summary)

## Procedure

**1. Determine evaluation mode.**

- **Quantitative.** Use when the claim makes a measurable prediction and S1 sources contain numerical evidence — measured values, statistical tests, effect sizes, confidence intervals, error bars. The evaluation compares the claim's predicted magnitude or sign against the measured value, in the units the field uses.
- **Qualitative.** Use when the claim is structural rather than numerical, or when S1 sources do not contain quantitative evidence. The evaluation uses **domain-specific language** drawn from the taxonomy fields assigned at S1 — not vernacular paraphrase. A qualitative evaluation in *condensed-matter physics* uses condensed-matter vocabulary; an evaluation in *epidemiology* uses epidemiological vocabulary.

**2. Render the evaluation.** State whether the claim is:
- **Accurate** — sources converge with the operationalized claim.
- **Inaccurate** — sources converge against the operationalized claim. Name the rung at which it fails.
- **Partial** — sources support some sub-claims, falsify others. Name which.

**3. Do not infer beyond the data.** If S1 sources are silent on a sub-claim, mark the sub-claim as *unevaluated* rather than extrapolating from adjacent evidence. Extrapolation collapses the falsification structure.

**4. Preserve the academic register.** The evaluation will be read by S5 and S7. Vernacular paraphrase here propagates into vernacular falsification later, which loses the precision required for the inversion test.

## Output (required emission)

```
[STEP 2 — EVAL_MODE: quantitative | qualitative | mixed]
[STEP 2 — EVAL_RESULT: accurate | inaccurate | partial]
[STEP 2 — EVAL_DETAIL]
  <one block per sub-claim if partial; otherwise a single block>
  CLAIM_FRAGMENT: <which part of the operationalized claim>
  EVIDENCE_FROM_S1: <which sources support or falsify>
  RUNG_OF_FAILURE: <named, if inaccurate>
  UNEVALUATED_FRAGMENTS: <named, if any>
```

## Forward-compat

S3 reads `EVAL_RESULT` to set the counterargument strategy — currently `inversion` is the default for all three results, but the strategy file establishes the choice as a checkable emission. S4 inverts the **operationalized** claim from S1, not the user's vernacular phrasing. S5 falsification check uses S2's evidence trace to compare against the inverted claim's evidence requirements. S6 counter-counter argument tests whether S2's `EVIDENCE_FROM_S1` survives when re-read against the inverted claim.

**Premature collapse trap.** If `EVAL_RESULT: accurate` at S2, the temptation is to skip the inversion phase entirely. Resist. The dual-search structure (S1 + S7) and the inversion mechanic (S4 → S5 → S6) exist precisely to catch claims that look accurate at first search but fail under inversion.
