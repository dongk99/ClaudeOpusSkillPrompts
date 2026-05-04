# Step 4 — Retrieve Candidate Precedents

```
─── GATE: VERIFY S1–S3 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 1 — PREDICATE] emitted
☐ [STEP 2 — MODE] emitted with ground
☐ [STEP 3 — RUNG STRUCTURE] emitted (AXIS, REF_RUNG, MECH_RUNGS, CONSTRAINT_RUNGS all populated)
☐ [STEP 3 — PERSISTENCE] confirms rung structure saved
If unchecked → STOP. Return to the missing step.
```

## Purpose

Dispatch retrieval subagents (multi-agent execution) or run retrieval directly (single-agent execution) to surface candidate precedents. Subagents do not synthesize, do not recommend, do not gate — they return structured candidate lists for the parent to gate at S5.

## Inputs

- `[STEP 1 — PREDICATE]`, `[STEP 2 — MODE]`, `[STEP 3 — RUNG STRUCTURE]`
- All three embedded as inert text in subagent prompts

## Procedure

1. Construct one retrieval subagent prompt per substantively distinct candidate-search direction, using the template below.
2. Dispatch all retrieval subagents in parallel. Wait for all to return.
3. Collect returned candidate lists. Do not act on subagent "preliminary assessments" — they are advisory only and bypass the S5 gate.
4. **If a subagent returns "no candidates found": record the negative result. Do not dispatch additional retrieval subagents to find weaker candidates.** Token-spend optimization will pull toward expanding the search; the parent resists. Absence of survivors is a valid result.

## Retrieval Subagent Prompt Template

```
TASK: Retrieve candidate precedents for a prediction query. You are operating
as a retrieval worker. You will not synthesize predictions, recommend a single
best precedent, or perform structural-similarity gating. You return all viable
candidates the parent will gate.

QUERY PREDICATE: [parent fills in from STEP 1]
OPERATING MODE: [historical-analog | thought-experiment | hybrid]
RUNG STRUCTURE OF QUERY (treat as authoritative input):
  Axis: [from STEP 3]
  Reference rung: [from STEP 3]
  Adjacent rungs supplying mechanism: [from STEP 3]
  Adjacent rungs setting constraint: [from STEP 3]

YOUR TASK:
1. Search the corpus relevant to your assigned mode for candidate precedents
   operating at the reference rung along the named axis.
   - Historical-analog: documented historical record.
   - Thought-experiment: established principles in the implicated domains.
   - Hybrid: documented precedents with adjacent rung structure.
2. For each candidate found, return:
   - Name and one-sentence description
   - Primary source (URL, citation, or principle reference)
   - Apparent rung-by-rung correspondence to the query (which rungs match,
     which differ, named explicitly)
   - Preliminary assessment of structural similarity, advisory only
3. If no candidate is found, return that finding and state what was searched.

OUTPUT FORMAT: Structured list, one entry per candidate. No prose synthesis.
No recommendation.
```

## Output (required emission)

```
[STEP 4 — CANDIDATES]
  <one entry per returned candidate, with name, source, rung map, advisory note>
[STEP 4 — NEGATIVE RESULTS: <subagents that returned empty, with what they searched>]
```

## Forward-compat

S5 gates each candidate through axis-coherence using scale-reasoning. S6 reads the survivor count to determine exit state. Subagent advisory notes are visible at S5 but do not bypass the gate.
