# Step 5 — Aggregate Overlap

```
─── GATE: VERIFY S4 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 4 — HITS] emitted for every alternative in S2
☐ Each table has Total, Hits, and Rate columns populated
☐ Snippet-truncation and off-topic exclusions are recorded
If unchecked → STOP. Return to step_4_count.md.
```

## Purpose

Aggregate the per-alternative hit tables from S4 into a single overlap measurement. The aggregate is the primary input to the verdict at S6, but per-alternative rates are preserved so the verdict can distinguish the paraphrase case (uniform high overlap) from the mixed case (some alternatives separate, others don't).

## Inputs

- All [STEP 4 — HITS] tables from the prior step
- Total result counts per alternative

## Procedure

**1. Compute totals across all alternatives.**
- Total results retrieved across the entire skill run: N_total = Σ M_i (sum of result counts for each alternative i)
- Per-excluded-term total hits: H_T = Σ h_T,i (sum of hits for term T across all alternatives)
- Per-excluded-term aggregate rate: R_T = H_T / N_total

**2. Preserve per-alternative breakdown.** Carry forward the per-alternative rate for each excluded term — do not collapse to aggregate-only. The mixed-verdict logic at S6 needs per-alternative data to identify which alternatives separate cleanly.

**3. Note the metric's asymmetry.** The reported rate measures *occurrences of excluded terms in alternative-term literature* — one direction only. Strict Jaccard would also measure *occurrences of alternative terms in excluded-term literature* and combine the two. The skill uses the asymmetric metric because the question being tested is asymmetric: "do the alternatives escape the excluded-term frame," not "do the two vocabularies overlap symmetrically." Naming the emission "JACCARD" is a loose convention; the value is asymmetric overlap.

**4. Do not normalize, weight, or transform.** Report raw rates. Weighting by source quality or paper recency introduces editorial discretion that the verdict should not depend on.

## Output (required emission)

```
[STEP 5 — JACCARD]
  Total results across all alternatives: N_total

  Aggregate rates (excluded term in alternative-term literature):
    <term1 canonical>: H1 / N_total = R1%
    <term2 canonical>: H2 / N_total = R2%
    ...

  Per-alternative rates:
    Alternative 1: <name>
      <term1>: <rate>
      <term2>: <rate>
      ...
    Alternative 2: <name>
      <term1>: <rate>
      ...
    ...

  METRIC NOTE: Reported rates are asymmetric overlap (excluded term presence in alternative-term literature only). Strict bidirectional Jaccard not computed.
```

## Forward-compat

S6 reads the aggregate rates to apply thresholds (separated / mixed / paraphrase). For the mixed-verdict case, S6 also reads the per-alternative rates to identify which alternatives carry the separation and which don't — that information is what the user needs to choose between candidates.

If the user requests strict bidirectional Jaccard, S5 may be re-run after additional searches that target the excluded-term literature for occurrences of the alternative terms. The threshold values in S6 are calibrated against the asymmetric metric and would need recalibration for strict-Jaccard interpretation; that recalibration is out of scope for the standard skill run.
