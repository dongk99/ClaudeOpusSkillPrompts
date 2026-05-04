# Step 4 — Count Occurrences

```
─── GATE: VERIFY S3 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 3 — SEARCHES] emitted for every alternative in S2
☐ Each block has at least one result registered (after off-topic exclusions)
☐ Query strings are recorded
If unchecked → STOP. Return to step_3_search.md.
```

## Purpose

For each retrieved result, scan the snippet text for occurrences of each excluded term (and its registered variants). Record hit-or-miss per result, per excluded term. The hit table is the load-bearing data for the aggregate at S5 and the verdict at S6.

## Inputs

- Snippet text returned by web_search at S3 for each registered result
- The excluded terms and variants from S1
- web_fetch (optional, for snippet-truncation cases)

## Procedure

**1. For each result, scan the snippet text.** A "hit" for term T occurs when T or any registered variant appears anywhere in the snippet. Match strings exactly as registered at S1 (case-insensitive is fine; substring match within longer words is fine — "finetuning" within "instruction-finetuning" still hits).

**2. Counting rules — what counts as a hit:**
- The excluded term appears in the snippet, regardless of context
- The term is a topic, a contrast class, a citation, a background reference, or any other usage
- "Defined in contrast to T" still counts: a paper that says "X without Y" or "X is the alternative to Y" is using Y to define X. The dependency is the overlap signal regardless of polarity. (Per Hard Rule 5 in SKILL.md.)
- Self-citations to the excluded term's seminal papers count: "Bai et al. 2022" implicitly cites Constitutional AI; "Christiano et al. 2017" implicitly cites RLHF. If the citation appears in the snippet and the citation refers to the excluded technique, register a hit even if the term itself is not spelled out.

**3. Counting rules — what does NOT count:**
- A homonym in a different field (e.g., "alignment" in the structural-engineering sense if the excluded term was "alignment-via-training")
- A term that the snippet cuts off mid-word, such that hit/miss cannot be determined — in this case, fetch the full text via web_fetch and re-scan

**4. Snippet truncation handling.** If the snippet is too short to determine hit/miss for any excluded term, fetch the full text via web_fetch on that result. Note the fetch in the table footer. Do not assume miss when the snippet is truncated; that is a self-favorable bias.

**5. Granularity match (Hard Rule 4 in SKILL.md).** If the excluded term is "alignment-via-training," do not count occurrences of bare "alignment" — that is a wider class. Match exactly the variants registered at S1. If the excluded term is broad ("alignment"), do count all occurrences of that broader term — but the responsibility for granularity lies in S1's registration, not in S4's relaxation.

**6. Build one hit table per alternative.** Rows are excluded terms; columns are total results, hits, and rate. The rate is hits divided by total results retrieved for that alternative.

## Output (required emission, one table per alternative)

```
[STEP 4 — HITS: <alternative N>]
  Total results: M

  Excluded term       | Hits | Total | Rate
  <term1 canonical>   | h1   | M     | h1/M
  <term2 canonical>   | h2   | M     | h2/M
  ...

  Snippet-truncation fetches: <count, or "none">
  Off-topic homonyms excluded: <count and brief reason, or "none">
```

Repeat for each alternative.

## Forward-compat

S5 reads the hit tables and aggregates across all alternatives. The per-alternative rates here are the inputs to the aggregate computation. S6 verdict applies thresholds to the aggregate but also references the per-alternative tables for the "mixed" case (some alternatives separate, others don't).

A miscount at S4 — undercounting hits to favor a separation verdict, or overcounting to favor paraphrase — corrupts the verdict. The user is entitled to a count they could reproduce by reading the same snippets themselves. Apply the rules mechanically; do not adjudicate "is this hit really evidence of overlap?" at counting time. That adjudication belongs to the user reading the verdict, not to Claude running the count.
