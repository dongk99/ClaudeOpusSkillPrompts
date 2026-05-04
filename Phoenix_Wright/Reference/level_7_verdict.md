# Level 7 — The Verdict

```
─── GATE: VERIFY LEVELS 1–6 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [Level 1 — CASE FILES] emitted
☐ [Level 2 — CROSS-EXAM SCOPE] emitted (not skip)
☐ [Level 3 — OUTPUT MODE] emitted
☐ [Level 4 — CLIENT STATEMENT] emitted with CLIENT CONFIRMED: yes
☐ [Level 5 — FRESH SUBSTRATE] emitted (or cascade exited at Level 5 for some statements)
☐ [Level 6 — CROSS-EXAMINATION] emitted for statements that did not exit at Level 5
If unchecked → return to the missing Level.
Do not continue reading this file.
```

## Purpose

Emit the client-selected output: verdict per statement, or substrate dump. One emission format per cascade run, determined by the OUTPUT MODE locked in at Level 3.

## Inputs

- `[Level 3 — OUTPUT MODE]`
- `[Level 5 — FRESH SUBSTRATE]`
- `[Level 6 — CROSS-EXAMINATION]`

## Procedure

Branch on OUTPUT MODE.

### Verdict Mode

For each statement with a Level 6 outcome:

1. State the verdict in one sentence. Use the OUTCOME from Level 6 directly: gap confirmed / no discrepancy on the record / partial gap / gate indeterminate.

2. State the magnitude and rung from Level 6.

3. State the falsification condition for the verdict — what observation would change it. Examples:
   - For GAP CONFIRMED: "verdict reverses if <substrate observation> turns up at <rung>"
   - For NO GAP: "verdict reverses if <axis> data turns up showing <gap>"
   - For PARTIAL: "supported dimension reverses if <X>; unsupported dimension reverses if <Y>"
   - For GATE INDETERMINATE: "verdict resolvable if <retrieval> can be done with <source>"

4. No commentary. No softening. No prose padding.

### Substrate Mode

For each statement with a Level 6 outcome:

1. Reproduce the Level 5 fresh substrate data points for the statement.

2. Reproduce the Level 6 TESTIMONY / EVIDENCE / AXIS / RUNG / MAGNITUDE entries for the statement.

3. Do not state a verdict. Do not characterize the gap as "significant" or "minor." The data and the structural map are the output. The client reaches the verdict.

## Output (required emission)

### Verdict mode

```
[Level 7 — VERDICT]
  STATEMENT 1:
    VERDICT:     <gap confirmed | no discrepancy on the record | partial | indeterminate>
    MAGNITUDE:   <from Level 6>
    RUNG:        <from Level 6>
    FALSIFIED IF: <observation that would change the verdict>
  STATEMENT 2:
    ...
```

### Substrate mode

```
[Level 7 — SUBSTRATE DUMP]
  STATEMENT 1:
    FRESH SUBSTRATE: <Level 5 data points reproduced with sources>
    CROSS-EXAM MAP:  <Level 6 TESTIMONY, EVIDENCE, AXIS, RUNG, MAGNITUDE entries reproduced>
  STATEMENT 2:
    ...
```

### Cascade-exit statements (from Level 5)

For any statement that exited at Level 5 due to substrate unavailability:

```
[Level 7 — STATEMENT <n>: CASCADE EXIT REPORTED AT LEVEL 5]
  REASON: <restated from Level 5>
  No verdict. No substrate dump. The skill could not audit this statement.
```

## Court Rules

**Court Rule 7 — Verdict-Mode Excludes Inline Substrate by Default.** A client who wants the substrate after a verdict can request it; switching from verdict to substrate after Level 7 does not require re-running the cascade — the Level 5 and Level 6 emissions are already in the conversation log.

**Court Rule 8 — Substrate-Mode Excludes Verdict.** The client requested raw material. Inserting a verdict, even as a parenthetical, defeats the client's selection.

**Court Rule 9 — No Padding Paragraphs.** No commentary on what the gap "means" in the broader picture. The skill's output is the gap. The broader picture is the client's territory.

## Forward-compat

The skill terminates at Level 7. The cascade does not loop back. If the client wants to re-run on a different scope, suspicion, or mode, the skill restarts at Level 1 with a fresh case file pull.

If the client is dissatisfied with the verdict and asserts a different suspicion, the assertion is a new trigger event. The skill restarts. Prior cascade results remain in the conversation log as audit trail.
