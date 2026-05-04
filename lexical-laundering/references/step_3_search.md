# Step 3 — Search Per Alternative

```
─── GATE: VERIFY S2 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 2 — ALTERNATIVES] emitted
☐ At least one canonical search form is registered
☐ COUNT N is stated
If unchecked → STOP. Return to step_2_alternatives.md.
```

## Purpose

Run web_search on each registered alternative to retrieve the literature that the count in S4 will operate over. The search must be unbiased — the excluded term must not appear in the query, because that would inflate the overlap rate artificially.

## Inputs

- The alternatives list from S2
- The excluded terms from S1 (used only as a NEGATIVE constraint on query construction)
- web_search tool

## Procedure

**1. Construct queries.** For each alternative, build a search query using:
- The alternative term itself, in quotes if multi-word
- One or two disambiguating context words drawn from the domain (e.g., "LLM," "language model," "AI safety," "evaluation")

**Forbidden:** appending the excluded term or its variants to the query. Doing so biases results toward overlap and corrupts the test. Confirmation searches are explicitly named as a Bad Ending in SKILL.md.

Example query construction:
- Alternative: "in-context alignment" → query: `"in-context alignment" LLM behavior`
- Alternative: "behavioral elicitation" → query: `"behavioral elicitation" model evaluation`
- Alternative: "specification gaming" → query: `"specification gaming" LLM probing`

**2. Run one web_search per alternative.** Target 5–8 results per alternative for adequate sample size. Minimum 4 if budget-constrained. The skill's threshold values in S6 are calibrated against samples of this size; substantially smaller samples produce unreliable verdicts.

**3. Record the result inventory.** For each alternative, list the retrieved results with a brief identifier (source name, paper title or first few words, URL fragment). The full snippets are processed at S4; this step records only that the result exists and where it came from.

**4. Do not pre-filter results.** Every result returned by the search is registered, including:
- Aggregator content
- Wikipedia / encyclopedia entries
- SEO-optimized blog posts
- Preprints and peer-reviewed papers alike

If a result is genuinely off-topic (the search returned a homonym in a different domain), exclude it with explicit reason. Do not exclude on-topic results because they happen to mention the excluded term — that is the data S4 is collecting.

## Output (required emission, one block per alternative)

```
[STEP 3 — SEARCHES: <alternative N>]
  QUERY: <exact query string used>
  RESULTS:
    1. <source / brief identifier> | URL fragment
    2. <source / brief identifier> | URL fragment
    ...
  EXCLUDED_OFF_TOPIC: <list of results dropped with reason, or "none">
```

Repeat the block for each alternative in S2. If S2 registered N alternatives, S3 emits N blocks.

## Forward-compat

S4 reads the snippet text returned by web_search for each registered result and counts excluded-term occurrences. The result identifiers recorded here are the row labels in S4's hit tables. S5 aggregates across all results from all alternatives — accurate result counting at S3 is required for the denominator.

A search that retrieved fewer than 4 results for an alternative produces an unreliable per-alternative rate. If this happens, S3 may be re-run with a slightly broader query (different disambiguating context words, never the excluded term). Note the re-run in the EXCLUDED_OFF_TOPIC field with reason. Do not silently swap queries between runs.
