# Step 3 — Select Counterargument Strategy

```
─── GATE: VERIFY S0–S2 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 1 — CLAIM_OPERATIONALIZED] emitted
☐ [STEP 2 — EVAL_RESULT] emitted (accurate | inaccurate | partial)
If unchecked → STOP. Return to the missing step.
```

## Purpose

Initiate the counterargument phase. Select the counterargument strategy that S4 will execute. **Currently `inversion` is the only strategy.** The strategy is emitted as an explicit token rather than implied so that the workflow remains structurally extensible — if other strategies are added (counterexample, regress-to-primitive, scope-restriction), they slot in here without restructuring.

## Inputs

- `[STEP 1 — CLAIM_OPERATIONALIZED]`
- `[STEP 2 — EVAL_RESULT]`

## Procedure

**1. Confirm S2 evaluation status.** The counterargument runs regardless of S2's result — `accurate`, `inaccurate`, and `partial` all proceed. **Do not skip the counterargument phase if S2 returned accurate.** A claim that survives the initial evaluation is precisely the claim the inversion mechanic is designed to test. Premature acceptance is the failure mode S3–S7 exists to prevent.

**2. Select strategy.** Default and only current strategy: **`inversion`**. The mechanic: take the operationalized claim's causal structure and invert the affecting variable, then ask whether the consequence still occurs.

Pre-inversion: *"If X is affected by Y in Z manner, then A would happen."*
Post-inversion: *"If X is **not** affected by Y in Z manner, would A still happen?"*

**3. Reserve other strategies as future-compat.** If a future version of this skill adds strategies, this is the file where the strategy selection logic expands. The current procedure is short because the strategy space is currently single-element.

**4. Inversion is not negation.** This is the load-bearing distinction. The inversion mechanic asks whether the consequence (A) is *robustly* tied to the cause-mechanism pair (Y, Z). It does **not** ask whether the user's claim is false. A claim where A still happens after inversion is conflictory — the consequence is not uniquely produced by the causal mechanism the user named, but the user's claim is not thereby falsified.

## Output (required emission)

```
[STEP 3 — STRATEGY: inversion]
[STEP 3 — RATIONALE: default; only current strategy in the skill's strategy space]
```

## Forward-compat

S4 reads `STRATEGY` to know which mechanic to apply. With `inversion`, S4 applies the standard pre→post transformation to the operationalized claim. S5 falsification check is mechanically tied to the inversion produced — it tests whether S1's evidence supports the post-inversion consequence. S6 counter-counter argument is the structural complement of S5's check.

**Strategy-selection trap.** If you find yourself wanting to invent a non-inversion strategy here ("I'll just argue against the claim differently"), STOP. The skill's discipline depends on the inversion mechanic specifically. Ad-hoc counterargument production reintroduces the unbounded reasoning the skill was built to constrain.
