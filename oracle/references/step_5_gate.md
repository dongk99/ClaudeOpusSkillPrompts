# Step 5 — Gate Candidates Through Axis-Coherence Check

```
─── GATE: VERIFY S1–S4 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 1 — PREDICATE] emitted
☐ [STEP 2 — MODE] emitted
☐ [STEP 3 — RUNG STRUCTURE] emitted, persistence confirmed
☐ [STEP 4 — CANDIDATES] emitted (or [STEP 4 — NEGATIVE RESULTS] confirms empty)
☐ Scale-reasoning skill loaded in parent
If unchecked → STOP. Do not delegate this step. Parent only.
```

## Purpose

For each S4 candidate, invoke scale-reasoning to check whether the candidate's rung structure is coherent with the query's rung structure on the axis identified at S3. **Structural check, not topical.** The question is not whether candidate and query share keywords or subject matter, but whether they operate at the same rung along the same axis with mechanisms at adjacent rungs that map onto each other under explicit substitution.

## Inputs

- `[STEP 3 — RUNG STRUCTURE]`
- `[STEP 4 — CANDIDATES]`
- Scale-reasoning loaded in parent

## Procedure

For each candidate:

1. Invoke scale-reasoning to extract the candidate's rung structure.
2. Compare candidate rung structure to query rung structure on the S3 axis.
3. Reject candidates that fail any of the four common failure modes:
   - **Rung mismatch:** candidate at a different scale.
   - **Axis mismatch:** candidate's axis differs from query's even with surface topic shared.
   - **Mechanism mismatch:** adjacent rungs in candidate populated by mechanisms that do not map to query's adjacent rungs.
   - **Effective-theory mismatch:** candidate's regime of validity does not cover query's regime.
4. **Subagent advisory assessments do not bypass the gate.** Subagents over-rate surface similarity (no scale-reasoning). Treat advisory notes as input to be checked, not as conclusions to be ratified.
5. For surviving candidates, record rung-by-rung correspondence to the query and the rungs at which deltas exist (in hybrid mode).

**Canonical failure illustration:** Black-Death-to-COVID. Surface similarity high. Axis-coherence fails: medical-knowledge rung, communication-speed rung, governance-density rung diverge by orders of magnitude. Reject the analog at this step rather than letting surface match drive prediction.

## Output (required emission)

```
[STEP 5 — SURVIVED]
  <candidate>: rung-correspondence map; deltas (if hybrid)
  ...
[STEP 5 — REJECTED]
  <candidate>: rejection reason (rung | axis | mechanism | effective-theory mismatch)
  ...
```

## Forward-compat

S6 reads survivor count to determine exit state. S7 references the rung correspondence and delta inventory directly in the prediction. The rejection log is preserved — it documents the discipline and is visible to the user in the final response.

**Do not delegate this step.** Subagents lack scale-reasoning and produce surface-similarity assessments. The gate is the load-bearing structural commitment of the skill.
