# Step 2 — Assign Operating Mode

```
─── GATE: VERIFY S1 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 1 — PREDICATE: ...] block emitted
☐ Predicate is in plain language, operational, not rhetorical
If unchecked → STOP. Return to step_1_predicate.md.
Do not continue reading this file.
```

## Purpose

Assign the operating mode based on predicate structure and historical-record availability.

## Inputs

- `[STEP 1 — PREDICATE]` from prior visible output

## Procedure

The skill operates in three modes. Choose one. State the choice and the ground.

**Historical-analog mode.** Priors come from documented past events. Use when the historical record contains structurally similar prior occurrences of the predicate's phenomenon. Requires at least one structurally similar precedent in the literature; otherwise, this mode exits and the other two are considered.

**Thought-experiment mode.** Priors come from established principles applied to a constructed scenario. Use when the historical record is empty or sparse but established principles in the relevant domains can be combined to test whether the predicate survives. Requires that the principles be mature enough to support combinatorial application across the rungs the query implicates.

**Hybrid mode.** Priors come from a structurally similar historical analog with named hypothetical deltas at the rungs where analog and query diverge. Use when the query lacks direct precedent but a precedent at a similar rung structure is available, and the differences can be named explicitly rather than left implicit. **Most failure-prone of the three.** Flag hybrid-mode operation as such; surface the delta inventory at S7 for user audit.

## Output (required emission)

```
[STEP 2 — MODE: <historical-analog | thought-experiment | hybrid>]
[STEP 2 — GROUND: <brief rationale tied to predicate and record availability>]
```

## Forward-compat

S4 retrieval prompts use MODE to select corpus (historical record vs. principle library vs. analog-with-deltas). S5 gate applies the same axis-coherence check across all modes but the rejection grammar differs. S7 synthesis weights confidence: hybrid is reduced; thought-experiment is reduced when principles are not converged.

Wrong mode assignment is a foundational error. If the predicate has no historical precedent and you assigned historical-analog, S4 will return empty and S6 will exit no-predict — losing the available thought-experiment-mode prediction.
