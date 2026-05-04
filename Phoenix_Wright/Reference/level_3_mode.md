# Level 3 — Pick the Output Mode

```
─── GATE: VERIFY LEVELS 1–2 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [Level 1 — CASE FILES] emitted
☐ [Level 2 — CROSS-EXAM SCOPE: backward-full | current-only] emitted
   (If [Level 2 — CROSS-EXAM SCOPE: skip] → cascade already terminated.
    Do not read this file.)
☐ [Level 2 — IN-SCOPE TESTIMONY] emitted
If unchecked → return to the missing Level.
Do not continue reading this file.
```

## Purpose

The client picks the output mode the cascade will deliver at Level 7. Mode is locked in before the client states the suspicion, so the verdict cannot be shaped post-hoc to match the suspicion.

## Inputs

- `[Level 2 — CROSS-EXAM SCOPE]`
- `[Level 2 — IN-SCOPE TESTIMONY]`

## Procedure

1. Present the two output modes:

   - **verdict** — Level 7 emits a per-testimony verdict: discrepancy confirmed (with magnitude and rung named), no discrepancy on the record (substrate supports the testimony), partial (specific dimension supported, specific dimension not), or gate indeterminate. Falsification condition for the verdict stated.

   - **substrate** — Level 7 emits the fresh retrieval data and the cross-exam map as raw material. No verdict synthesized. The client reaches their own verdict.

2. Wait for client response. Do not proceed to Level 4 until the client has named one of the two modes.

3. If the client requests both, default to **substrate** and note that a verdict can be added on top afterward — verdict can always be layered on substrate; substrate cannot be reconstructed from a verdict.

## Output (required emission)

```
[Level 3 — OUTPUT MODE OPTIONS PRESENTED]
  OPTIONS: verdict | substrate
  AWAITING CLIENT SELECTION
```

After client selects:

```
[Level 3 — OUTPUT MODE: <verdict | substrate>]
```

## Forward-compat

Level 7 reads OUTPUT MODE to determine emission format. Level 5 and Level 6 run the same regardless of mode — the difference is only in Level 7 presentation. Substrate-mode does not skip Level 6; the cross-examination map is computed and presented as data even when no verdict is rendered.
