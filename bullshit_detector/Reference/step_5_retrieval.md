# Step 5 — Forced Fresh Substrate Retrieval

```
─── GATE: VERIFY S1–S4 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 1 — BACKWARD SCAN] emitted
☐ [STEP 2 — SCOPE] emitted (not skip)
☐ [STEP 3 — OUTPUT MODE] emitted
☐ [STEP 4 — SUSPICION PREDICATE] emitted with USER CONFIRMED: yes
If unchecked → STOP. Return to the missing step.
Do not continue reading this file.
```

## Purpose

Retrieve fresh substrate data targeted at the AXIS OF DISAGREEMENT named at S4. Substrate must come from sources external to the prior Claude output that produced the suspicion.

## Inputs

- `[STEP 2 — IN-SCOPE CLAIMS]`
- `[STEP 4 — SUSPICION PREDICATE]` — one or more

## Hard Rule — Source Admissibility

**Prior Claude claims about the substrate are not admissible as substrate.** The cascade exists because prior Claude output is the suspect. Re-citing prior Claude claims defeats the cascade.

Admissible sources:

- Web search results retrieved this turn
- Web fetch of primary sources retrieved this turn (institutional websites, peer-reviewed papers, government documents, regulatory filings, official datasets)
- Files the user has provided directly in this conversation
- Computer-tool inspection of physical artifacts (hardware data, measurements, logs)
- The user's own statements about physical reality they have direct access to

Inadmissible sources:

- Prior Claude turns
- Claude's training-data memory presented without external verification
- Prior Claude's web search summaries (re-pull the source)
- Reasoning chains derived from any of the above

## Procedure

1. For each predicate at S4, identify what fresh data would discriminate the AXIS OF DISAGREEMENT. Examples:

   - Axis = "stated duration vs actual operating data" → retrieve documented operating durations from primary sources
   - Axis = "stated environmental tolerance vs actual environment" → retrieve environmental specifications from manufacturer / agency / measurement records
   - Axis = "stated capability vs actual capability evidence" → retrieve capability-evidence records (test reports, OIG audits, peer review, post-mortems, telemetry)

2. Run retrieval. Use web_search, web_fetch, file inspection, or computer-tool inspection. Multi-direction parallel retrieval is encouraged when the axis spans multiple data sources.

3. Record retrieved data with explicit source attribution per data point. Cite primary URLs or document identifiers.

4. **If retrieval returns no admissible substrate**, do not fall back to prior Claude claims. Emit the negative result and exit the cascade with that finding stated.

## Output (required emission)

```
[STEP 5 — FRESH SUBSTRATE]
  PREDICATE 1:
    AXIS: <restated from S4>
    QUERIES RUN: <list>
    DATA RETRIEVED:
      - <data point> | source: <URL or document ID>
      - <data point> | source: <URL or document ID>
      ...
    ADMISSIBILITY CHECK: <pass — all sources external to prior Claude>
  PREDICATE 2:
    ...
```

If retrieval returns empty or all-inadmissible:

```
[STEP 5 — FRESH SUBSTRATE]
  PREDICATE <n>:
    AXIS: <restated>
    QUERIES RUN: <list>
    DATA RETRIEVED: none admissible
    REASON: <web access unavailable | primary source paywalled | no records exist | other>
[STEP 5 — CASCADE EXIT]: substrate retrieval failed for predicate <n>
```

A cascade exit at S5 terminates the skill. Do not proceed to S6 for any predicate that exited at S5. Other predicates (with admissible substrate) may continue.

## Forward-compat

S6 compares the FRESH SUBSTRATE per predicate against the CLAIM UNDER SUSPICION on the named AXIS. The data points retrieved at S5 are the only basis admitted at S6. S7 references S5 data directly in substrate-mode output and indirectly (as evidentiary backing) in verdict-mode output.

The retrieval is the load-bearing audit operation. A cascade with weak retrieval produces a verdict that has no more authority than the prior Claude output it was meant to audit.
