# Step 1 — Field Assignment and Initial Web Search

```
─── GATE: VERIFY S0 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 0 — DECISION] emitted
☐ NOT [DECISION: full-block] (full-block terminates the skill — do not enter S1)
☐ If [DECISION: partial] → operate only on falsifiable sub-claims
If unchecked → STOP. Return to step_0_subjective_check.md.
```

## Purpose

Tag the claim across academic fields using `reference_taxonomy.md`. Run an initial `web_search` to validate or falsify the claim within those fields. Restate the claim as an operationalized, falsifiable proposition.

## Inputs

- User claim (or falsifiable sub-claims if `[DECISION: partial]`)
- `reference_taxonomy.md` — authoritative field naming
- Tools: `web_search` (mandatory at this step)

## Procedure

**1. Assign fields.** Consult `reference_taxonomy.md`. Assign **up to 4 fields** — more is better when the claim genuinely spans domains. Field tags are drawn from three top-level sections of the taxonomy:
- **§1 Sciences** (Natural / Formal / Social / Applied)
- **§2 Engineering**
- **§3 Cross-cutting Technology Domains**

Where a claim sits at the intersection of two or more fields, name all relevant ones. **Vocabulary must be academic register** — if the user wrote semiformally, find the closest academic equivalent rather than propagating the user's vocabulary into the field tag.

**2. Operationalize the claim.** Restate the claim as a falsifiable proposition. The operationalized form names: the subject (X), the affected entity (Y), the manner of effect (Z), and the predicted consequence (A). The standard form: *"If X is affected by Y in Z manner, then A would happen."* This form is what S4 will invert.

**3. Run web_search.** For each assigned field, search for evidence that validates or falsifies the operationalized claim. Use field-specific terminology drawn from the taxonomy. Prefer primary sources, peer-reviewed reviews, and textbook treatments over aggregator content.

**4. Record the search trace.** Queries used, sources returned, and which sources were retained as evidence. The trace is required because S7 will run a second search, and source-distinctness between S1 and S7 is the load-bearing falsification mechanic.

## Output (required emission)

```
[STEP 1 — FIELDS: <up to 4, taxonomy-sourced names>]
[STEP 1 — CLAIM_OPERATIONALIZED: <If X is affected by Y in Z manner, then A would happen>]
[STEP 1 — INITIAL_SEARCH]
  QUERIES: <list>
  SOURCES_RETAINED: <list with URL/citation>
  RESULT_SUMMARY: <claim is <supported | partially-supported | unsupported> by retained sources>
```

If the relevant field is absent from `reference_taxonomy.md`:

```
[STEP 1 — FIELD_TAXONOMY_GAP: <description of the missing field>]
```

Name the absence rather than mis-tagging into the nearest available field.

## Forward-compat

S2 reads `[INITIAL_SEARCH — RESULT_SUMMARY]` to determine evaluation mode (quantitative vs qualitative) and to render the formal evaluation. S4 inverts the operationalized claim — a sloppily-operationalized claim cannot be cleanly inverted. S7 second search must target sources distinct from `[SOURCES_RETAINED]` — record them explicitly so the distinctness check is mechanical at S7.
