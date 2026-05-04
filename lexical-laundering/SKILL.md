---
name: lexical-laundering
description: Verification skill for cases where Claude is proposing alternative vocabulary to a specified excluded term, and the alternatives are being treated as evidence that the work-being-described is categorically different from the excluded term. Runs a Jaccard-style overlap test against the actual literature for each proposed alternative, counting occurrences of the excluded term(s) in retrieved results. Catches the failure mode where Claude generates "alternative" vocabulary that lives entirely inside the excluded term's literature ecosystem — paraphrase smuggled as separation. Skip when the alternative-term proposal is for stylistic register only (e.g., formal-vs-casual), not for categorical separation.
---

# Lexical-Laundering — Vocabulary Substitution Test

A user (or Claude itself) has proposed alternative vocabulary to a term that was excluded from the response. The proposal is being treated as evidence that the underlying activity is *not* the excluded term — that it lives in a different conceptual category. This skill tests that claim against the actual literature.

The discipline: Claude's confidence about whether two terms separate in vocabulary space is not evidence. The literature's actual co-occurrence pattern is. If "alternative" terms live in papers that extensively use the excluded term, the alternatives are paraphrase, not separation. The skill produces the count and the verdict.

## Trigger

The skill activates when **any** of the following is met. Structural triggers describe the failure-mode shape; keyword triggers are explicit user-invocation overrides that bypass structural-trigger ambiguity.

**Structural triggers (3):**

1. Claude has been asked to propose alternative vocabulary to one or more specified excluded terms.
2. Claude is generating such alternatives unprompted as part of describing user-side work.
3. The alternatives are being used (or about to be used) as evidence that the activity-being-described is categorically distinct from the excluded term — not just stylistically different.

**Keyword triggers (4):**

4. User prompt contains the substring "laundering" anywhere. Catch-all; subsumes 5–7 but is listed separately so the skill does not require disambiguation before activating.
5. User prompt contains the interrogative "are you laundering" (with or without object: "are you laundering me," "are you laundering this," "are you laundering X").
6. User prompt contains a declarative accusation: "you're laundering," "you laundered," "you just laundered," "stop laundering."
7. User prompt explicitly invokes the skill by name: "lexical laundering," "run lexical-laundering," "lexical-laundering check."

Keyword triggers override the structural-trigger out-of-scope clause. If a keyword trigger fires, the skill activates regardless of whether the structural conditions are met — the user's explicit invocation is sufficient evidence that the test is wanted.

**In-scope (structural triggers):** claims of categorical separation (e.g. "this is not fine-tuning, it's X"), claims of distinct conceptual space, claims of vocabulary-level differentiation that bear on professional positioning, application content, or technical attribution.

**Out-of-scope (structural triggers only):** register adjustment ("formal vs casual phrasing"), translation between languages, synonym substitution for prose flow, requests for varied wording where no separation claim is attached.

If only the structural triggers are ambiguous and no keyword trigger fires — if the user just wants different words for prose reasons — decline. The skill demands web-search budget; structural false positives are more expensive than structural false negatives. Keyword triggers do not have this ambiguity problem; they are explicit invocation and always activate.

## Mandatory File-Read Order

Each step file contains a **GATE** block that verifies the prior step's emission exists in the visible conversation log. **Do not read step N+1 before step N's required output is emitted.**

```
references/step_1_excluded_set.md   → emits [EXCLUDED]
references/step_2_alternatives.md   → emits [ALTERNATIVES]
references/step_3_search.md         → emits [SEARCHES] (one block per alternative)
references/step_4_count.md          → emits [HITS] (one table per alternative)
references/step_5_aggregate.md      → emits [JACCARD]
references/step_6_verdict.md        → emits [VERDICT: separated | mixed | paraphrase]
```

## Hard Rules

**Rule 1 — Literature is the ground truth.** Claude's prior confidence that two terms separate in concept space is irrelevant. Only retrieved literature counts. If Claude generated the alternatives confidently, that confidence does not survive a high-overlap result.

**Rule 2 — No selection-down.** All retrieved results are scored. Do not exclude unfavorable results to lower the overlap rate. If a result is genuinely irrelevant (wrong topic, wrong domain), state the exclusion explicitly with reason.

**Rule 3 — Pre-verdict counting.** The count is run before the verdict is named. Do not name a verdict in advance and then count toward it. Step 4 must complete before Step 6 reads.

**Rule 4 — Granularity match.** If the excluded term is "alignment-via-training," do not count occurrences of bare "alignment" — that's a wider class. Match the registered variants from Step 1 only. Granularity drift is a self-favorable bias.

**Rule 5 — Self-defining-via-contrast counts as overlap.** A paper that says "X without Y" or "X is the alternative to Y" is a paper that uses Y to define X. The dependency on Y is the overlap signal, regardless of polarity.

## Output Conventions

The skill emits all six bracket blocks in order. The verdict block is the load-bearing output; the user should be able to read the verdict without reading the prior blocks if they trust the procedure.

Verdict block must include the concession sentence when the verdict is mixed or paraphrase. Do not soften — if the alternatives are paraphrase, name the substrate-honest framing the user would use instead. The point of the skill is to produce that framing, not to leave the user in vocabulary limbo.

## Bad Endings

**Bad Ending — Confirmation searches.** Searching with the excluded term included in the query biases results toward overlap. Step 3 explicitly forbids this. Use disambiguating context words instead.

**Bad Ending — Manufactured separation.** Searching only for results that exclude the excluded term, or selecting down to results that do, produces a false separation verdict. Rule 2 forbids this.

**Bad Ending — Stylistic-claim drift.** The structural triggers are for categorical-separation claims. If the user's actual goal was register adjustment ("phrase this less formally") and no keyword trigger fired, running the skill burns budget on a claim the user wasn't making. The trigger gate exists to prevent this; honor it.

**Bad Ending — Aggregate hiding.** Reporting only the aggregate rate without per-term breakdown can hide a mixed verdict (one excluded term separates cleanly, another doesn't). Step 5 emits per-term rates; preserve them.

**Bad Ending — Verdict laundering.** After getting a paraphrase verdict, generating new alternatives and not running the skill on them. The skill must be re-run on any new candidate set, regardless of how confident Claude is that the new candidates separate.

## Caveats and Known Limitations

**Search-engine bias.** Results returned by web_search reflect search-engine ranking, which favors recent and SEO-optimized content. The Jaccard estimate is conditional on what surfaces in the top results; deep-archive papers may show different patterns. The skill's verdict is honest about top-of-search literature, not about the full citation graph.

**Snippet truncation.** web_search returns snippets, not full text. A term that appears in the body but not the snippet will be missed. For high-stakes verdicts near threshold, fetching full text via web_fetch on a sample of results tightens the count.

**Field-specific drift.** A term may have low overlap with the excluded term in one field and high overlap in another. The skill does not auto-disaggregate by field. If the user is positioning their work for a specific audience (e.g. an Anthropic Honesty team, a particular journal), search queries should be field-anchored where possible.

**Asymmetry of the metric.** The reported overlap is one-directional. Strict Jaccard would also count occurrences of the alternative terms in the excluded term's literature. The skill's threshold values are calibrated against the asymmetric metric; recalibration is required if the user wants strict-Jaccard interpretation.

**No novel verdict.** The skill does not adjudicate whether the underlying activity *should* be categorized under the excluded term. It only adjudicates whether the proposed alternative vocabulary carves the claimed separation in the literature. Categorical questions outside vocabulary substitution are out of scope.
