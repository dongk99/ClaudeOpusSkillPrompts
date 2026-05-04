# Step 3 — Extract Query Rung Structure

```
─── GATE: VERIFY S1–S2 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 1 — PREDICATE] emitted
☐ [STEP 2 — MODE] emitted with ground
☐ Scale-reasoning skill is loaded in parent context
If unchecked → STOP. Return to the missing step.
Do not continue reading this file.
```

## Purpose

Extract the rung structure of the query — the axis along which the predicate operates, the reference rung, and the surrounding rungs that supply mechanism or set constraint. **This step requires the scale-reasoning skill.**

## Inputs

- `[STEP 1 — PREDICATE]`
- `[STEP 2 — MODE]`
- Scale-reasoning skill loaded in parent context

## Procedure

1. **Invoke scale-reasoning.** Run scale-reasoning's procedure on the predicate to extract:
   - **Axis:** length, time, behavioural aggregation, institutional aggregation, energy, etc.
   - **Reference rung:** the rung at which the predicate sits.
   - **Adjacent rungs supplying mechanism:** the rung(s) below or above that produce the causal substrate.
   - **Adjacent rungs setting constraint:** the rung(s) that bound the regime of validity.
2. Name the ladder in plain language. Do not abbreviate axis names beyond field convention.
3. **Save the rung structure to durable working memory** before any subagent dispatch (Memory file or equivalent). Multi-agent orchestrators truncate context above 200K tokens; the rung structure must survive truncation. If no Memory mechanism exists, retain inline and accept the budget cost.

**Loss of the rung structure mid-workflow is a fatal skill failure.** S5 cannot run without it. Do not proceed to S4 if the rung structure is incomplete or unsaved.

## Output (required emission)

```
[STEP 3 — RUNG STRUCTURE]
  AXIS: <named axis>
  REF_RUNG: <reference rung at which predicate operates>
  MECH_RUNGS: <adjacent rung(s) supplying mechanism>
  CONSTRAINT_RUNGS: <adjacent rung(s) setting regime of validity>
[STEP 3 — PERSISTENCE: <Memory file path | inline retained>]
```

## Forward-compat

S4 embeds the full rung structure in retrieval subagent prompts as inert text. S5 invokes scale-reasoning again to gate candidates against this structure. S7 references rung correspondence and rung deltas in the prediction synthesis. Phase II uses the axis to identify cross-domain consistency triggers.

The rung structure is the basis of every downstream gate and every retrieval prompt. Any error here propagates everywhere.
