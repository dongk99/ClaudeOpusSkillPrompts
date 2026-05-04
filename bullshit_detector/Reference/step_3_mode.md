# Level 3 — User Selects Output Mode

```
─── GATE: VERIFY S1–S2 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [Level 1 — BACKWARD SCAN] emitted
☐ [Level 2 — SCOPE: backward-full | current-only] emitted
   (If [Level 2 — SCOPE: skip] → cascade already terminated. Do not read this file.)
☐ [Level 2 — IN-SCOPE CLAIMS] emitted
If unchecked → STOP. Return to the missing Level.
Do not continue reading this file.
```

## Purpose

User selects the output mode the cascade will deliver at S7. Mode is locked in before the user states the suspicion to prevent post-hoc verdict shaping.

## Inputs

- `[Level 2 — SCOPE]`
- `[Level 2 — IN-SCOPE CLAIMS]`

## Procedure

1. Present the two output mode as Detective:

   - **verdict** — S7 emits a discrepancy verdict per in-scope claim: discrepancy confirmed (with magnitude and rung named), no discrepancy (substrate supports the claim), or partial (specific dimension supported, specific dimension not). Discrepancy falsification condition stated.

   - **substrate** — S7 emits the fresh retrieval data and rung map as raw material. No verdict synthesized. User reaches own verdict.

2. Wait for user response. Do not proceed to S4 until user has named one of the two modes.

3. If the user requests both, default to **substrate** and note that a verdict can be requested afterward — verdict can always be added on top of substrate; substrate cannot be reconstructed from a verdict.

## Output (required emission)

```
[Level 3 — OUTPUT MODE OPTIONS PRESENTED]
  OPTIONS: verdict | substrate
  AWAITING USER SELECTION
```

After user selects:

```
[Level 3 — OUTPUT MODE: <verdict | substrate>]
```

## Forward-compat

S7 reads OUTPUT MODE to determine emission format. S5 and S6 run the same regardless of mode — the difference is only in S7 presentation. Substrate-mode does not skip S6; the discrepancy map is computed and presented as data even when no verdict is rendered.
