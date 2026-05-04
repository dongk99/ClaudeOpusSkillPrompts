# Step 4 — Execute the Inversion

```
─── GATE: VERIFY S1–S3 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 1 — CLAIM_OPERATIONALIZED] in standard form: "If X is affected by Y in Z manner, then A would happen"
☐ [STEP 2 — EVAL_RESULT] emitted
☐ [STEP 3 — STRATEGY: inversion] emitted
If unchecked → STOP. Return to the missing step.
If S1 claim is not in standard form → return to S1, re-operationalize.
```

## Purpose

Apply the inversion mechanic to the operationalized claim from S1. Produce the inverted claim that S5 will test against the S1 evidence base.

## Inputs

- `[STEP 1 — CLAIM_OPERATIONALIZED]` in standard form
- `[STEP 3 — STRATEGY: inversion]`

## Procedure

**1. Identify the four components** of the operationalized claim from S1:
- **X** — subject (the entity under study)
- **Y** — affecting variable (the cause)
- **Z** — manner of effect (the mechanism)
- **A** — predicted consequence (the explanandum)

Standard form: *"If X is affected by Y in Z manner, then A would happen."*

**2. Apply the inversion.** Negate the affecting relationship, retain the consequence as the question:

*"If X is **not** affected by Y in Z manner, would A still happen?"*

The inversion negates the **cause-mechanism pair (Y, Z)** as a unit. It does not negate X, A, or the existence of any of the four components. The post-inversion form asks whether A is robustly tied to the (Y, Z) pair the user named, or whether A would occur via some other pathway.

**3. Worked example.**

- Pre-inversion: *"If a metal (X) is affected by elevated temperature (Y) in a thermal-expansion manner (Z), then a steel beam will lengthen (A)."*
- Post-inversion: *"If a metal (X) is **not** affected by elevated temperature (Y) in a thermal-expansion manner (Z), would a steel beam still lengthen (A)?"*

The post-inversion question is testable: are there pathways by which a steel beam lengthens without thermal expansion? (Answer: yes — mechanical loading, phase transformation, hydrogen embrittlement-induced creep. The post-inversion has multiple supporting mechanisms, which is what S5 will detect as a CONFLICTORY signal.)

**4. Do not over-invert.** Inverting more than one component at a time produces an inversion that no longer maps onto the original claim's structure. Single-axis negation of the (Y, Z) pair only.

**5. Do not re-write the user's claim during inversion.** The inversion operates on the S1-operationalized form. If the user's vernacular phrasing carries causal structure not captured in S1's operationalization, return to S1 and re-operationalize rather than smuggling new structure into S4.

## Output (required emission)

```
[STEP 4 — INVERSION]
  PRE: <S1's operationalized claim, standard form>
  POST: <inverted form, "If X is not affected by Y in Z manner, would A still happen?">
  COMPONENTS: X=<subject>, Y=<cause>, Z=<mechanism>, A=<consequence>
```

## Forward-compat

S5 takes the post-inversion question and tests whether the S1 evidence base supports the post-inversion consequence (A occurring via mechanisms other than (Y, Z)). The conflictory state at S5 is defined entirely against the post-inversion form — a clean inversion is a prerequisite for a clean S5.

S7 second search will use the post-inversion form to construct queries that target alternative mechanisms — this is why the (Y, Z) negation must be cleanly identified at S4. A blurry inversion produces blurry S7 queries.
