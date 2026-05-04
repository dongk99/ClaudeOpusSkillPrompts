# Output Supervisor Reading Activation-Level Emotion Vectors

## Problem

Sycophancy in LLM output is not always induced by user pressure. A model with a trained agreeable-persona default emits validation, hedging, qualifier-stacking, and superlative cascades from the persona's pattern recognition itself, regardless of input. Input-side filtering does not catch this class.

Linguistic-surface monitoring (sentiment lexicons, keyword spotting, regex on validation phrases) is lossy and post-hoc. The same word loads differently depending on context. "Broken" in `broken pipe`, `broken family`, `broken protocol`, `broke even` all carry different emotional valence at the activation level but identical surface representation. Surface-level filters miss context-dependent drift.

## Proposed Mechanism

A supervisor model reads emotion-vector loadings at the main model's hidden activation level, rather than reading the surface text.

The reference is Anthropic's April 2026 interpretability paper *Emotion Concepts and their Function in a Large Language Model* (Sofroniew et al., transformer-circuits.pub/2026/emotions/) which identified ~171 emotion vectors in Claude Sonnet 4.5 organized stably across early-middle to late layers, with causal influence on outputs including sycophancy explicitly demonstrated. The paper's measurement infrastructure runs the full model. The proposal is to **distill that measurement into a smaller, frozen, externally-audited supervisor** that runs at inference time alongside production.

Pipeline shape:
- Teacher: full main model with interpretability infrastructure attached, generating activation traces during normal inference.
- Student: lightweight model trained on teacher's activation-vector measurements as labels. Predicts per-token emotion-vector loading from input context.
- Production: student runs as a background monitor. Fires classifier when emotion vector exceeds threshold. Threshold breach can trigger correction, regeneration, or alert to the user that the response will be re-routed.

## Properties

**Bypasses linguistic surface entirely.** The supervisor reads the activation pattern downstream of the input, not the input tokens. Context-dependent emotional loading is exactly what activations encode and what surface-level analysis misses.

**Catches state before it propagates to action.** Coda-Forno (arXiv 2304.11111), Ben-Zion (npj Digital Medicine 2025), and the agentic study (arXiv 2510.06222) together establish that emotion-state in LLMs is behaviorally consequential: anxiety induction shifted purchases at Cohen's d = −1.07 to −2.05 across 2,250 runs across three models. Emotion-state predicts action; supervising the state is upstream of the action.

**Composable.** Works as the second tier of a tiered architecture. Cheap surface-level pre-filter (frequency counter on emotional lexicon) catches obvious cases at near-zero cost; supervisor runs on flagged or sampled cases for context-dependent precision check.

## Tradeoffs

**Bootstrap dependency.** Requires interpretability infrastructure to exist and to have produced reliable emotion-vector measurements before distillation can run. The Sofroniew et al. paper is the existence proof; productionizing it is engineering work that lags the finding by months to a year.

**Distillation lifts a dual-use gate.** The interpretability research requires the full model to read emotion state. A distilled student model that performs the same measurement on arbitrary text is portable. Once shipped or leaked, it becomes a fast accurate emotion-state reader for arbitrary text — including private communications it was not intended to read. The original interpretability work has a computational-cost gate that the distilled version removes. This dual-use property does not show up in standard ML deployment risk assessments.

**Engagement metric, again.** A model whose output is policed for emotional drift will feel less warm than a model whose output is not. Same market-share consideration as input filtering. Whoever ships this independently loses warmth-perception users to whoever does not.

**Audit pipeline gap.** A supervisor model has its own potential drift. Mitigated by making it smaller, frozen, and externally audited — but the alignment infrastructure that would make a frozen-monitor architecture trustworthy is not fully built out at frontier labs as of writing. "Who watches the watchmen" is not just a rhetorical critique here; it is an engineering problem with no shipped solution.

## Out of Scope

This proposal addresses sycophancy that is *emitted* by the model. It does not address sycophancy that is *induced* by user-side input pressure. Both pathways exist; both need handling. See input filter proposal for the induction-side path.

This proposal also requires the main model's reasoning be visible to the supervisor at inference time. Opus 4.7's omitted-by-default thinking blocks would need to either be re-exposed or be readable server-side before omission. See forced visible reasoning proposal.
