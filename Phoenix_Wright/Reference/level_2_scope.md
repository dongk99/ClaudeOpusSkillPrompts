# Level 2 — Pick the Cross-Exam Scope

```
─── GATE: VERIFY LEVEL 1 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [Level 1 — CASE FILES] block emitted
☐ SUBSTRATE named
☐ PRIOR TESTIMONY list present (may be "none on file")
☐ CURRENT TESTIMONY identified
If unchecked → return to references/level_1_case_files.md.
Do not continue reading this file.
```

## Purpose

Present the case files to the client as a scope-selection reminder. The client decides which testimony the cross-examination runs on.

## Inputs

- `[Level 1 — CASE FILES]` from prior visible output

## Procedure

1. Present the case file output as a reminder block. Do not summarize. Do not editorialize.

2. State the three scope options:
   - **backward-full** — cross-exam runs on every testimony in the case files, prior + current
   - **current-only** — cross-exam runs on the current testimony only
   - **skip** — case dismissed before opening, return to normal conversation

3. Wait for client response. Do not proceed to Level 3 until the client has named one of the three scopes.

4. If the client response is ambiguous (e.g., "do the relevant ones"), do not guess. Ask which specific testimonies to include and wait again.

## Output (required emission)

```
[Level 2 — CROSS-EXAM SCOPE OPTIONS PRESENTED]
  REMINDER: <reproduce Level 1 case files output verbatim>
  OPTIONS: backward-full | current-only | skip
  AWAITING CLIENT SELECTION
```

After client selects:

```
[Level 2 — CROSS-EXAM SCOPE: <backward-full | current-only | skip>]
[Level 2 — IN-SCOPE TESTIMONY: <list of T<n> references the cross-exam will run on>]
```

If the client selects **skip**, the cascade terminates here. Return to normal conversation. Do not proceed to Level 3.

## Forward-compat

Level 5 retrieval re-pulls substrate for every testimony in IN-SCOPE TESTIMONY. Level 6 runs the cross-examination against every in-scope testimony. Level 7 produces verdict or substrate dump scoped to the same set.

If the client selects backward-full and the prior-testimony list is long, token cost scales with testimony count. The client accepted the cost by selecting that scope. Do not unilaterally truncate.
