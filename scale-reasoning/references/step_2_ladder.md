# Step 2 — Build the Vertical Ladder

```
─── GATE: VERIFY S1 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 1 — REFERENCE_RUNG] emitted in plain language
☐ Phenomenon and rung match the query (no premature axis or domain commitment)
If unchecked → STOP. Return to step_1_anchor.md.
```

## Purpose

Build a single coherent vertical ladder around the reference rung along **one axis**, held constant for the remainder of the workflow.

## Inputs

- `[STEP 1 — REFERENCE_RUNG]`

## Procedure

**1. Select the axis.** Common axes: behavioural aggregation, biological organisation, institutional aggregation, length, time, energy, particle count, agent count, level of abstraction in a formal hierarchy, computational depth. Select the axis the phenomenon makes salient. Name it explicitly. **Hold it constant.** Do not switch axes mid-workflow.

**2. Build the ladder.** Name rungs ascending and descending from the reference rung, in plain language, until further extension stops adding causal explanatory power for the question.

**3. Set depth.** Rung-count is elastic and proportional to the depth the question demands. A simple causal question may resolve in 3–4 rungs. A hard cross-scale question may justify 10+. **Treat rung-count as a reasoning-effort budget:** more rungs buy explanatory reach but compound combinatorially when alternatives are tested in Phase II, and excessive depth degrades coherence under context-window pressure. Exercise judgment on the right depth. State the chosen depth so the user can see the call.

**4. Tag domains.** Each rung receives one or more domain tags from the critical-thinking taxonomy — Sciences (natural, formal, social, applied), Engineering, Cross-cutting Technology Domains. **Distribute cross-domain coverage across the ladder rather than concentrating it at any one rung.** Where a rung sits at the intersection of two or more domains, name both.

**5. Flag convention-dependence.** Rung ordering and effective-theory naming inherit textbook conventions. Where conventions are mature, the ladder is coherent. Where they are not, **flag the convention-dependence explicitly** so the user can see which rungs are firm and which are stipulative.

## Output (required emission)

```
[STEP 2 — LADDER]
  AXIS: <named axis, held constant>
  DEPTH: <N rungs, with named justification for depth choice>
  RUNGS (ascending or descending from REF_RUNG):
    rung_+k: <name> [domain tags]  (firm | stipulative — note convention)
    ...
    REF_RUNG: <name> [domain tags]
    ...
    rung_-k: <name> [domain tags]
  CONVENTION_FLAGS: <list of rungs where mainstream convention is contested or absent>
```

## Forward-compat

S3 traverses this ladder causally. The axis is the constraint S3 cannot break — divergence-handling descent is locked to this axis. S4 alternative-theory search uses domain tags to scope its corpus. S5 substitution operates at the rung where the alternative claims to apply.

**Multi-axis hybrid is a structural failure mode.** If you cannot build a single coherent ladder along one axis, that finding belongs in S3 as `[AXIS_FAIL]`, not in S2 as a hybrid ladder. Return here only after re-anchoring the axis.
