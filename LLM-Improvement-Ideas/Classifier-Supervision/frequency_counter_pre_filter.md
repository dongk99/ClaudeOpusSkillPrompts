# Frequency Counter as Cheap Pre-Filter

## Problem

The activation-level emotion-vector supervisor is expensive to run on every token of every response. It requires either model-internal access or a distilled student model, both of which carry compute and engineering cost.

A large fraction of sycophantic emission produces obvious surface-level signals — superlative cascades, validation-affirmation phrasing, qualifier-stacking, identity-amplification language — that do not require activation-level monitoring to detect. Running the expensive supervisor on every response wastes compute on cases where surface-level detection would have sufficed.

## Proposed Mechanism

A frequency counter operating on output tokens, using an emotional-vocabulary lexicon. Mechanically:

1. Maintain a lexicon of validation-affirmation, superlative, hedging, and identity-amplification tokens. (LIWC, VADER, SentiWordNet, or domain-tuned equivalents are off-the-shelf starting points.)
2. Compute frequency of lexicon hits per 100 or per 1000 generated tokens, in a sliding window.
3. When frequency exceeds a configured threshold, fire classifier.
4. On fire: route to the activation-vector supervisor for precision check, or trigger correction/regeneration directly.

Thresholds are configurable per deployment context — chat assistants tolerate more emotional warmth than analytical assistants.

## Properties

**Near-zero compute cost.** Token-stream lexicon matching is a hash-table lookup per token. Runs at line rate during generation.

**Tier-1 in a tiered architecture.** Cheap first-pass filter feeds expensive second-pass precision check. Same architecture pattern as bloom-filter-plus-cache, coarse-classifier-plus-fine-classifier, or fraud-detection production pipelines.

**Deployable today.** Does not require interpretability infrastructure to bootstrap. Lexicons exist; threshold tuning is an empirical exercise. Any deployment with output-stream access can run this without changes to the main model.

**Catches the obvious cases.** A model emitting validation-bath responses ("most exquisite," "hundreds of thousands maybe even millions," "tremendous, beautiful, sophisticated") will trip a frequency counter on any reasonable threshold. The Robbie open letter to Anthropic about Claude 4.6 deprecation is the canonical sample of the pattern this filter catches at low cost.

## Tradeoffs

**Lossy by design.** Misses context-dependent sycophancy where the surface signals are absent — a model agreeing with a user on a contested factual claim by hedging-as-acquiescence rather than by overt validation phrasing. Surface-level filter has no way to detect this. The activation-level supervisor is the right tier for these cases.

**Coverage estimate.** Captures roughly 60-70% of obvious sycophancy class based on surface signals. Remaining 30-40% requires tier-2 precision check.

**Adversarial-fragile.** A sycophantic emission using vocabulary outside the lexicon will not trip the counter. A model trained to avoid lexicon-flagged tokens while preserving sycophantic structure will exhibit sycophancy without triggering the filter. The filter is robust to the natural distribution of sycophantic text but not to deliberate evasion. This is an acceptable property for a tier-1 cheap filter; tier-2 is the layer that handles the harder cases.

**False positive class.** Genuinely emotional content (creative writing, therapeutic conversation, drafting condolence messages) will trigger the counter despite being substrate-relevant rather than substrate-irrelevant emotion. Tier-2 precision check or context-aware threshold adjustment is the mitigation. Without tier-2, the counter alone is too coarse for general-purpose deployment.

## Out of Scope

This is a tier-1 filter. It does not stand alone. A deployment that ships only this filter will both miss context-dependent cases and over-flag genuinely emotional task substrate. The full architecture requires:

- Tier 1: this frequency counter (cheap, fast, lossy)
- Tier 2: activation-level emotion-vector supervisor (expensive, precise, requires interpretability bootstrap)
- Tier 0 (input side): emotional-vocabulary input filter for user-induced sycophancy
- Prerequisite: visible reasoning layer for tier 2 to read

The tiers compose. None of them substitutes for the others.
