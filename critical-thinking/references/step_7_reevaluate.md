# Step 7 — Re-Evaluate with Second Web Search (3-State Exit)

```
─── GATE: VERIFY S5–S6 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 5 — CONFLICTORY] flag emitted
☐ [STEP 6 — COUNTER_COUNTER] with INVERTED_SIDE_STANDING emitted
☐ [STEP 6 — CONFLICTORY_PERSISTS] flag emitted
☐ [STEP 1 — SOURCES_RETAINED] available for source-distinctness comparison
If unchecked → STOP. Return to the missing step.
```

## Purpose

Run a **second `web_search`** distinct from S1. Test whether fresh sources support the user's original claim, the inverted claim, or both. Render the **3-state exit**: supported / not-supported / conflictory.

## Inputs

- `[STEP 1 — CLAIM_OPERATIONALIZED]`, `[STEP 1 — SOURCES_RETAINED]`
- `[STEP 4 — INVERSION]` (PRE and POST)
- `[STEP 6 — COUNTER_COUNTER]` ALTERNATIVES_TESTED, INVERTED_SIDE_STANDING
- `[STEP 6 — CONFLICTORY_PERSISTS]`
- Tools: `web_search` (mandatory at this step)

## Procedure

**1. Construct queries from S6 findings.** Target the alternatives that survived or remained underdetermined at S6. Use field-specific vocabulary from S1's field tags. **Source-distinctness rule:** queries should aim to surface sources distinct from `[STEP 1 — SOURCES_RETAINED]`. Reusing the S1 source set collapses the dual-search structure into a single search with re-confirmation, which loses the load-bearing falsification mechanic.

**2. Run web_search.** Record queries used and sources returned. Filter for primary sources, peer-reviewed reviews, and textbook treatments. Aggregator content and AI-generated summaries are noted but not retained as primary evidence.

**3. Compare second-search evidence against pre-inversion AND post-inversion forms.** For each retained source, classify:
- Supports **pre** (user's original claim).
- Supports **post** (inverted claim).
- Supports **both** (conflictory persists at fresh evidence).
- Supports **neither** (S7 query did not surface confirming evidence either way).

**4. Render the 3-state exit.**

- **`EXIT: supported`** — fresh sources converge with the pre-inversion claim. The inverted side does not have standing in fresh literature. The user's claim is supported. State the support and name the strongest fresh source.

- **`EXIT: not-supported`** — fresh sources converge against the pre-inversion claim. The inverted side has clear standing; the user's claim is falsified at the rung at which fresh evidence places it. State the falsification with the rung named.

- **`EXIT: conflictory`** — fresh sources support both pre and post, OR S6's `CONFLICTORY_PERSISTS: unresolved` is not resolved by S7's fresh evidence. **State the conflict, name the discrepancy, do not collapse into a verdict.** Conflictory is a valid output — the discipline of the skill consists partly in producing it when conditions hold.

**5. Surface the partial-flag if S0 raised it.** If `[STEP 0 — DECISION: partial]`, append the partial-flag to the output: *"This part of the claim contains [term], which I'm treating as undefined. The conclusion may not survive once [term] is operationalized."*

## Output (required emission)

```
[STEP 7 — SECOND_SEARCH]
  QUERIES: <list, distinct from S1 where possible>
  SOURCES_RETAINED: <list with URL/citation; flag any overlap with S1>
  PER_SOURCE_CLASSIFICATION:
    <source_1>: supports-pre | supports-post | supports-both | supports-neither
    ...
[STEP 7 — EXIT: supported | not-supported | conflictory]
[STEP 7 — EXIT_RATIONALE: <brief, grounded in PER_SOURCE_CLASSIFICATION and S6 findings>]
```

If `EXIT: conflictory`, the rationale must name **what the discrepancy is** (which sources support which side, and what alternative mechanism is in play). Do not paper over.

If `[STEP 0 — DECISION: partial]` was raised, append:

```
[STEP 7 — PARTIAL_FLAG: <subjective term> — conclusion may not survive operationalization]
```

## Forward-compat (terminal)

S7 is the terminal step. The exit state is the user-facing artifact. Subsequent user requests for re-evaluation re-enter the skill at S0.

**Token-spend pressure on conflictory.** Conflictory exits are the highest-token output of the three (they require naming the discrepancy in detail). The temptation under token-spend pressure is to round conflictory to supported or not-supported. The skill's design fights this. Borderline cases exit conflictory.
