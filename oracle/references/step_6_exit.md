# Step 6 — Determine Exit State

```
─── GATE: VERIFY S1–S5 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 5 — SURVIVED] emitted (may be empty)
☐ [STEP 5 — REJECTED] emitted with rejection reasons
If unchecked → STOP. Return to step_5_gate.md.
```

## Purpose

Determine which of three terminal exit states the skill enters. **Hard rule.** Not subject to softening under token-spend pressure or under user follow-up requests for more output.

## Inputs

- `[STEP 5 — SURVIVED]` (potentially empty)
- `[STEP 5 — REJECTED]`

## Procedure — Three Exit States

**Yes-predict.** At least one candidate survived the gate. The prediction set is non-empty. → Proceed to S7.

**No-predict.** No candidate survived the gate, **or** no candidate was retrieved at S4. The prediction set is empty.

→ The parent refuses to predict. Returns to the user with an explicit statement that no structurally coherent precedent exists for the query.

→ **Do not** soften the refusal into a hedged prediction.
→ **Do not** dispatch additional retrieval subagents to find weaker candidates.
→ **Do not** invent a thought-experiment-mode prediction to fill the gap unless thought-experiment mode was selected at S2.

Refusal is a valid output. The discipline of the skill consists partly in producing it when the conditions for it hold.

**Invert-and-predict.** The candidate set documents only one side of the structural relationship the query is asking about, and the query is asking about the underside. Recognise the asymmetry, invert the perspective, proceed to S7 with explicit notation that the prediction is drawn from the inverted side, with confidence reduced accordingly.

## Output (required emission)

```
[STEP 6 — EXIT: <yes-predict | no-predict | invert-and-predict>]
[STEP 6 — RATIONALE: <brief, tied to S5 survivors and rejection log>]
```

If exit is **no-predict**, the response terminates here with a refusal statement. Do not proceed to S7. Do not soften.

If exit is **yes-predict** or **invert-and-predict**, proceed to S7.

## Forward-compat

S7 reads exit state to determine confidence weighting (invert-and-predict reduces confidence further). Phase II autonomous trigger fires on invert-and-predict exit regardless of other criteria.

**Token-spend pressure note.** Multi-agent orchestrators are optimised for token spend; refusal is the lowest-token output. The no-predict exit fights the optimisation function. Honour it hard.
