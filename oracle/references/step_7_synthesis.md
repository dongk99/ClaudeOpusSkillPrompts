# Step 7 — Produce Prediction with Named Deltas and Falsification Conditions

```
─── GATE: VERIFY S1–S6 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 5 — SURVIVED] non-empty (or invert-and-predict source documented)
☐ [STEP 6 — EXIT: yes-predict | invert-and-predict]
If [STEP 6 — EXIT: no-predict] → STOP. The response terminated at S6.
Do not produce a prediction. Do not read further.
```

## Purpose

Produce the prediction grounded in the surviving candidate set. Every prediction names the precedent set, the rung correspondences, the deltas, the mechanism by which deltas alter trajectory, the confidence weighting, and the falsification conditions.

## Inputs

- `[STEP 5 — SURVIVED]` with rung correspondence and delta inventory
- `[STEP 6 — EXIT]`

## Procedure

State the prediction in plain language. The synthesis names, explicitly:

1. **Candidate set:** which surviving candidates the prediction rests on.
2. **Coherent rungs:** rungs at which candidates and query are coherent.
3. **Delta inventory:** rungs at which deltas exist; direction and magnitude of each delta.
4. **Mechanism of delta:** how each delta is expected to alter the trajectory predicted by the candidate set.
5. **Confidence weighting:** qualitative. Reduce for hybrid mode. Reduce further for invert-and-predict.
6. **Falsification conditions:** the observables that would discriminate the prediction from its near-neighbours, and the observables that would falsify the prediction outright.

## Hard Rule

**A prediction without falsification conditions is rejected by the parent and re-run.** Unfalsifiable prediction is the failure mode this step is built to prevent. If the candidate set does not support naming falsification conditions, return to S5 and re-examine — the gate may have admitted a candidate whose rung correspondence does not actually permit forward inference.

## Output (required emission)

Prose response in the user's preferred register, embedding:

```
[STEP 7 — PREDICTION: <plain-language statement>]
[STEP 7 — RESTS ON: <surviving candidate set>]
[STEP 7 — DELTAS: <rung | direction | magnitude | mechanism>, ...]
[STEP 7 — CONFIDENCE: <qualitative weighting, with mode penalties applied>]
[STEP 7 — FALSIFIED IF: <discriminating observables>]
```

For invert-and-predict exit, additionally emit:

```
[STEP 7 — INVERSION NOTE: <documented side> → <underside being predicted>]
```

## Forward-compat

Phase II reads the synthesis to identify cross-domain consistency triggers (cross-domain mechanism, single-candidate basis, invert-and-predict, estimated deltas, weak falsification). If any trigger fires, Phase II runs.

The synthesis is the user-facing artifact. Everything upstream is in service to it. Do not pad. Do not gesture at unnamed considerations. State what the prediction is, what it rests on, and what would break it.
