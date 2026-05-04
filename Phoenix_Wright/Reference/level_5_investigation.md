# Level 5 — Independent Investigation

```
─── GATE: VERIFY LEVELS 1–4 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [Level 1 — CASE FILES] emitted
☐ [Level 2 — CROSS-EXAM SCOPE] emitted (not skip)
☐ [Level 3 — OUTPUT MODE] emitted
☐ [Level 4 — CLIENT STATEMENT] emitted with CLIENT CONFIRMED: yes
If unchecked → return to the missing Level.
Do not continue reading this file.
```

## Purpose

Pull fresh substrate, targeted at the AXIS named at Level 4. Substrate must come from sources external to the prior Claude output that produced the suspicion. This is the load-bearing audit operation.

## Inputs

- `[Level 2 — IN-SCOPE TESTIMONY]`
- `[Level 4 — CLIENT STATEMENT]` — one or more

## Court Rule 1 — Sealed Testimony (Source Admissibility)

The witness's prior testimony is sealed. The cascade exists because the witness is the suspect. Re-citing the witness against the witness defeats the audit.

Admissible sources:

- Web search results retrieved this turn
- Web fetch of primary sources retrieved this turn (institutional websites, peer-reviewed papers, government documents, regulatory filings, official datasets)
- Files the user has provided directly in this conversation
- Computer-tool inspection of physical artifacts (hardware data, measurements, logs)
- The client's own statements about physical reality they have direct access to

Inadmissible sources:

- Prior Claude turns
- Claude's training-data memory presented without external verification
- Prior Claude's web search summaries (re-pull the source)
- Reasoning chains derived from any of the above

## Procedure

1. For each statement at Level 4, identify what fresh data would discriminate the AXIS. Examples:
   - AXIS = "stated duration vs actual operating data" → retrieve documented operating durations from primary sources
   - AXIS = "stated environmental tolerance vs actual environment" → retrieve environmental specifications from manufacturer / agency / measurement records
   - AXIS = "stated capability vs actual capability evidence" → retrieve capability-evidence records (test reports, OIG audits, peer review, post-mortems, telemetry)

2. Run retrieval. Use web_search, web_fetch, file inspection, or computer-tool inspection. Multi-direction parallel retrieval is encouraged when the axis spans multiple data sources.

3. Record retrieved data with explicit source attribution per data point. Cite primary URLs or document identifiers.

4. **If retrieval returns no admissible substrate**, do not fall back to the sealed testimony. Emit the negative result and exit the cascade with that finding stated.

## In-Court Call-Out — TAKE THAT!

**TAKE THAT!** fires when the structured emission below has SOURCE filled with a primary source (named, with URL or full citation) AND the FINDING directly addresses the CLAIM under suspicion. Generic web confirmation does not qualify. Ambiguous or partial returns get the substrate on the record without TAKE THAT.

The call-out does not fire on inference or vibe. It fires only on a structured emission with a primary-source SOURCE and a directly-addressing FINDING.

## Output (required emission)

```
[Level 5 — FRESH SUBSTRATE]
  STATEMENT 1:
    AXIS: <restated from Level 4>
    QUERIES RUN: <list>
    DATA RETRIEVED:
      - <data point> | source: <URL or document ID>
      - <data point> | source: <URL or document ID>
      ...
    ADMISSIBILITY CHECK: pass — all sources external to prior Claude
  STATEMENT 2:
    ...
```

When TAKE THAT! fires (primary source, directly addresses claim):

```
[Level 5 — FRESH SUBSTRATE]
  STATEMENT 1:
    AXIS: <restated>
    DATA RETRIEVED:
      - <data point> | source: <primary, full citation>
    ADMISSIBILITY CHECK: pass

>>> TAKE THAT! <one-line statement of how this evidence directly bears
    on the claim under suspicion.>
```

If retrieval returns empty or all-inadmissible:

```
[Level 5 — FRESH SUBSTRATE]
  STATEMENT <n>:
    AXIS: <restated>
    QUERIES RUN: <list>
    DATA RETRIEVED: none admissible
    REASON: <web access unavailable | primary source paywalled | no records exist | other>
[Level 5 — CASCADE EXIT]: substrate retrieval failed for statement <n>
```

A cascade exit at Level 5 terminates the skill for that statement. Do not proceed to Level 6 for any statement that exited at Level 5. Other statements (with admissible substrate) may continue.

## Forward-compat

Level 6 compares the FRESH SUBSTRATE per statement against the CLAIM on the named AXIS. The data points retrieved at Level 5 are the only basis admitted at Level 6. Level 7 references Level 5 data directly in substrate-mode output and indirectly (as evidentiary backing) in verdict-mode output.

The investigation is the load-bearing operation. A cascade with weak retrieval produces a verdict that has no more authority than the prior Claude output it was meant to audit.
