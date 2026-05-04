# Step 2 — Identify Alternatives

```
─── GATE: VERIFY S1 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 1 — EXCLUDED] emitted
☐ TERMS lists at least one canonical form
☐ REASON is stated
If unchecked → STOP. Return to step_1_excluded_set.md.
```

## Purpose

List the candidate alternative terms that have been proposed (or are about to be proposed) as the replacement vocabulary. State each in the form it would appear in literature search, not in the form Claude wrote it for prose flow.

## Inputs

- Claude's prior or in-flight proposal of alternative terms
- The excluded set from S1 (used only for distinctness check, not for biasing the search at S3)

## Procedure

**1. Enumerate the candidate alternatives.** Each candidate gets one entry. If Claude's proposal contained variants of the same term clustered together (e.g. "prompt-based steering" and "prompt-level steering" appearing in the same bullet), keep them separate — they may have different overlap rates in the literature.

**2. State each candidate as it would be searched.** Use the canonical literature form. Strip prose-only phrasing. Examples:
- Claude wrote: "what you might call in-context alignment work" → canonical: "in-context alignment"
- Claude wrote: "this is closer to behavioral elicitation territory" → canonical: "behavioral elicitation"
- Claude wrote: "runtime constitutional engineering" → canonical: "constitutional AI" (the literature term) — note divergence from prose phrasing

**3. Distinctness check.** If a candidate alternative IS one of the excluded terms (literally — same canonical form), flag it and remove. The skill's purpose is to test whether *different* terms separate; testing the excluded term against itself is undefined.

**4. Do not pre-judge which alternatives will pass.** All listed candidates proceed to S3 search regardless of Claude's prior intuition about which will separate cleanly.

## Output (required emission)

```
[STEP 2 — ALTERNATIVES]
  1. <canonical search form>
  2. <canonical search form>
  ...
  N. <canonical search form>

  COUNT: N candidates registered for testing.
  FLAGGED_DUPLICATES: <any candidates removed for matching the excluded set, or "none">
```

## Forward-compat

S3 issues one web_search per registered alternative — the count N here determines the search budget. S4 builds one HITS table per alternative, using the same canonical forms as the table headers. S5 aggregates across all alternatives, weighting each result equally regardless of which alternative produced it. S6 verdict considers per-alternative rates separately for the "mixed" case.

A pre-filtered candidate list (Claude removing alternatives that "obviously won't pass") corrupts the test. List every candidate that was proposed, even those Claude expects will paraphrase-fail. The user is entitled to the data on Claude's confident-but-wrong predictions.
