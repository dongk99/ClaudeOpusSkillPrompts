# Step 7 — Emit Verdict or Substrate Dump

```
─── GATE: VERIFY S1–S6 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 1 — BACKWARD SCAN] emitted
☐ [STEP 2 — SCOPE] emitted (not skip)
☐ [STEP 3 — OUTPUT MODE] emitted
☐ [STEP 4 — SUSPICION PREDICATE] emitted with USER CONFIRMED: yes
☐ [STEP 5 — FRESH SUBSTRATE] emitted (or cascade exited at S5 for some predicates)
☐ [STEP 6 — DISCREPANCY MAP] emitted for predicates that did not exit at S5
If unchecked → STOP. Return to the missing step.
Do not continue reading this file.
```

## Purpose

Emit the user-selected output: verdict per predicate, or substrate dump. One emission format per cascade run, determined by the OUTPUT MODE locked in at S3.

## Inputs

- `[STEP 3 — OUTPUT MODE]`
- `[STEP 5 — FRESH SUBSTRATE]`
- `[STEP 6 — DISCREPANCY MAP]`

## Procedure

Branch on OUTPUT MODE.

### Verdict Mode

For each predicate with a S6 outcome:

1. State the verdict in one sentence. Use the OUTCOME from S6 directly: discrepancy confirmed / no discrepancy / partial discrepancy / gate indeterminate.

2. State the magnitude and rung from S6.

3. State the falsification condition for the discrepancy itself — what observation would change the verdict. Examples:
   - For DISCREPANCY CONFIRMED: "verdict reverses if <substrate observation> turns up at <rung>"
   - For NO DISCREPANCY: "verdict reverses if <axis> data turns up showing <gap>"
   - For PARTIAL: "supported dimension reverses if <X>; unsupported dimension reverses if <Y>"
   - For GATE INDETERMINATE: "verdict resolvable if <retrieval> can be done with <source>"

4. No commentary. No softening. No prose padding.

### Substrate Mode

For each predicate with a S6 outcome:

1. Reproduce the S5 fresh substrate data points for the predicate.

2. Reproduce the S6 STATED / ACTUAL / AXIS / RUNG / MAGNITUDE entries for the predicate.

3. Do not state a verdict. Do not characterize the discrepancy as "significant" or "minor." The data and the structural map are the output. The user reaches the verdict.

## Output (required emission)

### Verdict mode

```
[STEP 7 — VERDICT]
  PREDICATE 1:
    VERDICT: <discrepancy confirmed | no discrepancy | partial | indeterminate>
    MAGNITUDE: <from S6>
    RUNG: <from S6>
    FALSIFIED IF: <observation that would change the verdict>
  PREDICATE 2:
    ...
```

### Substrate mode

```
[STEP 7 — SUBSTRATE DUMP]
  PREDICATE 1:
    FRESH SUBSTRATE: <S5 data points reproduced with sources>
    DISCREPANCY MAP: <S6 STATED, ACTUAL, AXIS, RUNG, MAGNITUDE entries reproduced>
  PREDICATE 2:
    ...
```

### Cascade-exit predicates (from S5)

For any predicate that exited at S5 due to substrate unavailability:

```
[STEP 7 — PREDICATE <n> CASCADE EXIT REPORTED AT S5]
  REASON: <restated from S5>
  No verdict. No substrate dump. The skill could not audit this predicate.
```

## Hard Rules

**Verdict-mode output does not include the underlying substrate inline by default.** A user who wants the substrate after a verdict can request it; switching from verdict to substrate after S7 does not require re-running the cascade — the S5 and S6 emissions are already in the conversation log.

**Substrate-mode output does not include a verdict, even as a parenthetical.** The user requested raw material. Inserting a verdict defeats the user's selection.

**No padding paragraphs. No commentary on what the discrepancy "means" in the broader picture.** The skill's output is the discrepancy. The broader picture is the user's territory.

## Forward-compat

The skill terminates at S7. The cascade does not loop back. If the user wants to re-run on a different scope, suspicion, or mode, the skill restarts at S1 with a new backward scan.

If the user is dissatisfied with the verdict and asserts a different suspicion, treat the assertion as a new trigger event. The skill restarts. Prior cascade results remain in the conversation log as audit trail.
