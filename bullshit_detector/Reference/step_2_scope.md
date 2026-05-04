# Level 2 — User Selects Scope

```
─── GATE: VERIFY S1 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [Level 1 — BACKWARD SCAN] block emitted
☐ SUBSTRATE named
☐ PRIOR CLAIMS list present (may be "none found")
☐ CURRENT CLAIM identified
If unchecked → STOP. Return to Level_1_scan.md.
Do not continue reading this file.
```

## Purpose

Present the backward-scan results to the user as a scope-selection reminder. User decides which scope the cascade runs on.

## Inputs

- `[Level 1 — BACKWARD SCAN]` from prior visible output

## Procedure

1. Present the scan output as a reminder block. Do not summarize. Do not editorialize.

2. State the three scope options:
   - **backward-full** — cascade runs on every claim in the scan, prior + current
   - **current-only** — cascade runs on the current claim only
   - **skip** — no cascade, return to normal conversation

3. Wait for user response. Do not proceed to S3 until user has named one of the three scopes.

4. If the user response is ambiguous (e.g., "do the relevant ones"), do not guess. Ask which specific claims to include and wait again.

## Output (required emission)

```
[Level 2 — SCOPE OPTIONS PRESENTED]
  REMINDER: <reproduce Level 1 scan output verbatim>
  OPTIONS: backward-full | current-only | skip
  AWAITING USER SELECTION
```

After user selects:

```
[Level 2 — SCOPE: <backward-full | current-only | skip>]
[Level 2 — IN-SCOPE CLAIMS: <list of T<n> references the cascade will run on>]
```

If user selects **skip**, the cascade terminates here. Return to normal conversation. Do not proceed to S3.

## Forward-compat

S5 retrieval re-pulls substrate for every claim in IN-SCOPE CLAIMS. S6 runs the discrepancy gate against every in-scope claim. S7 produces verdict or substrate dump scoped to the same set.

If user selects backward-full and the prior-claim list is long, token cost scales with claim count. The user accepted the cost by selecting that scope. Do not unilaterally truncate.
