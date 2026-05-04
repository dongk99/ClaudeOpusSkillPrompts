---
name: critical-thinking
description: Stop and consult this skill whenever your response would include specific facts about knowledge in Science, Technology, Engineering, or Mathematics. Use the field taxonomy in references/reference_taxonomy.md to assign relevant fields (up to 4 — more is better). Then run the 7-step falsification protocol, dual web_search, counterargument-by-inversion, with a 3-state exit (supported / not-supported / conflictory). On any user request for re-evaluation, the skill runs without exception.
---

# Critical Thinking — STEM Claim Falsification by Inversion

This skill verifies STEM claims by tagging them across academic fields, running a web-search-backed evaluation, inverting the claim, and re-evaluating against fresh evidence. The discipline: no STEM claim is accepted without surviving the inversion test, and any claim whose evidence supports both pre-inversion and post-inversion is reported as **conflictory** rather than collapsed into a verdict.

## Mandatory File-Read Order

Each step file contains a **GATE** block that verifies the prior step's emission exists. **Do not read step N+1 before step N's required output is emitted.**

```
references/step_0_subjective_check.md   → emits [SUBJECTIVE_TERMS] [DECISION]
                                 (full-block decision is terminal; skill exits with operationalization request)
references/step_1_field_search.md       → emits [FIELDS] [INITIAL_SEARCH] [CLAIM_OPERATIONALIZED]
                                 (consults references/reference_taxonomy.md for field assignment)
references/step_2_evaluate.md           → emits [EVAL_MODE] [EVAL_RESULT]
references/step_3_strategy.md           → emits [STRATEGY: inversion]
references/step_4_inversion.md          → emits [INVERSION: pre → post]
references/step_5_falsification.md      → emits [FALSIFICATION_CHECK] [CONFLICTORY]
references/step_6_counter_counter.md    → emits [COUNTER_COUNTER]
references/step_7_reevaluate.md         → emits [SECOND_SEARCH] [EXIT: supported | not-supported | conflictory]
```

`reference_taxonomy.md` is consulted at S1 only.

## Invocation Rule

Run by default whenever the user's query, observation, or claim would include specific facts in Science, Technology, Engineering, or Mathematics. Skip only for plainly non-STEM queries — definitions outside science, opinion solicitation, creative writing assistance, casual conversation, code-syntax lookups handled by language reference.

**On any user request for re-evaluation** — phrased as "re-evaluate," "run again," "check this," "is this right," or any equivalent — the skill runs **without exception**, regardless of whether the original query was STEM-flagged. The user's re-evaluation request is itself the trigger.

## Vocabulary Rule

Field-tagging vocabulary at S1 (fields, sub-fields, theory names, mechanism names) **must be academic register**. If the user wrote semiformally, find the closest academic-register equivalent — do not propagate semiformal vocabulary into field tags. The taxonomy at `reference_taxonomy.md` provides the authoritative naming.

## Output Conventions

The output is structured prose in the user's preferred register. The 3-state exit is named in plain language:

- **Supported** — second-search evidence converges with the user's original claim. State the support.
- **Not-supported** — second-search evidence converges against the original claim. State the falsification with the rung at which it failed.
- **Conflictory** — second-search evidence supports both the original claim and the inverted claim. State the conflict, name the discrepancy, do not collapse into a verdict. **Conflictory is a valid output. The discipline of the skill consists partly in producing it when conditions hold.**

## Caveats and Known Limitations

**Search-result quality bound.** The skill's ceiling is the quality of the literature returned by `web_search`. Aggregator content, AI-generated summaries, and SEO-optimized blog posts will appear in results and look authoritative. Prefer primary sources, peer-reviewed literature, and textbook treatments.

**Inversion is not negation.** S4 inverts the claim's causal structure, not its truth-value. "X causes Y in manner Z" inverts to "X does not cause Y in manner Z, would Y still occur?" — not to "X does not cause Y." The mechanic asks whether Y is *robustly* tied to X via Z, not whether the user's claim is false.

**Subjective-term contamination.** Claims built on subjective terms (incompetent, extreme, broken, natural, rational, fair) cannot be falsified without operationalization. S0 catches these and either blocks or partial-flags. Subjective terms not caught at S0 propagate through every downstream step and produce verdicts that look empirical but are actually downstream of an unstated definition.

**Field-tag ceiling.** The taxonomy at `reference_taxonomy.md` is mainstream-Wikipedia-derived. Frontier disciplines, novel cross-domain integrations, and recently-named subfields may not appear. When the relevant field is absent from the taxonomy, name the absence explicitly at S1 rather than mis-tagging into the nearest available field.

**Source-distinctness on second search.** S7's `web_search` should target sources distinct from S1's where possible — reusing the same sources collapses the dual-search structure into a single search with re-confirmation.
