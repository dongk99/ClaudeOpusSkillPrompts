---
name: scale-reasoning
description: Generalized cross-domain scale-based reasoning. Use this skill whenever the user's query, observation, or claim involves causal reasoning — any question where understanding requires explaining how or why a phenomenon arises, including questions about behaviour, mechanism, biological processes, social patterns, physical phenomena, economic outcomes, historical causation, technical failure modes, or anything where the answer depends on moving between scales of organisation. Trigger by default in formal and semiformal contexts even when the user does not explicitly request scale analysis. Skip only for plainly non-causal queries — definitions, date lookups, code syntax, arithmetic, translations, transliterations, simple lookups. On any user request for re-evaluation, the skill runs without exception, regardless of whether the original query appeared causal.
---

# Scale-Based Cross-Domain Reasoning

Causal phenomena are reasoned about by traversing a vertical ladder of scales anchored at the user's reference point. The ladder is built dynamically from the query, not retrieved from a stored artefact. Two phases: a mandatory phase that runs by default on every causal query, and a conditional phase that synthesises with the critical-thinking skill to A/B-falsify mainstream against alternative theories.

The discipline: no causal claim is accepted without checking that it remains consistent across the rungs of organisation above and below it, and no rung silently smuggles a divergence whose underlying mechanism has not been located.

## Mandatory File-Read Order

Each step file contains a **GATE** block that verifies the prior step's emission exists in the conversation log. **Do not read step N+1 before step N's required output is emitted.**

```
references/step_1_anchor.md          → emits [REFERENCE_RUNG]
references/step_2_ladder.md          → emits [LADDER: AXIS|DEPTH|RUNGS|DOMAIN_TAGS|CONVENTION_FLAGS]
references/step_3_traversal.md       → emits [TRAVERSAL_DIRECTION] [DIVERGENCES] [CONVERGENCE_RUNG] [MAINSTREAM_TAGS]
                              or [AXIS_FAIL] (terminal: skill exits with axis-failure report)
references/phase_2_triggers.md       → emits [PHASE_II: triggered | skipped] [TRIGGER_REASONS]
                              ── if skipped, skill terminates after Phase I synthesis ──
references/step_4_alternatives.md    → emits [ALTERNATIVES] or [NO_ALTERNATIVE_FOUND]
references/step_5_resubstitute.md    → emits [SUBSTITUTION: per-alternative survival map]
references/step_6_report.md          → emits [REPORT_DELIVERED]
```

## Invocation Rule

Mandatory phase runs by default on every invocation. Skip only for plainly non-causal queries — definition lookup, date check, code syntax, arithmetic, translation, transliteration, simple factual lookup of the kind a reference work answers in one line. When skipping, state the skip in one sentence and name the ground so the user can see the call was deliberate, not overlooked.

**On any user request for re-evaluation** — phrased as "re-evaluate," "run again," "check this," "is this right," or any equivalent — the mandatory phase runs **without exception**, regardless of whether the original query appeared causal. The user's re-evaluation request is itself the trigger and overrides any prior judgment to skip.

The conditional phase runs on explicit user request and on autonomous invocation when any trigger in `phase_2_triggers.md` is satisfied.

## Output Conventions

Mandatory-phase output is prose in the user's preferred register. The ladder is named explicitly. The causal traversal is stated in full. Section headers used only where structure requires them; the ladder itself is named in a single passage rather than enumerated as a list, except where the user requested list formatting.

Conditional-phase output is structured by alternative-theory rather than by rung. Each alternative is presented with its rung-by-rung consistency check, its falsifying or surviving rungs named, and its standing relative to the mainstream theory stated in plain language.

When the mandatory phase is skipped, state the skip in a single sentence at the top of the response, name the ground, and proceed with the direct answer. This allows the user to override the skip with a re-evaluation request if the judgment was wrong.

## Caveats and Known Limitations

**Unfalsifiable by design.** The skill's outputs are not subject to objective test cases. Verification is left to the user, who checks output for cross-rung coherence and honest gap disclosure. The user has accepted this; the skill's value rests on the discipline of the procedure rather than on the certifiability of any single output.

**Convention-dependent.** Ladder coherence depends on the maturity of textbook conventions for the chosen axis. On well-established axes — biological organisation, length in physics, institutional aggregation in social science — the ladder tracks mainstream framing reliably. On contested axes, or axes spanning disciplines whose conventions disagree, the ladder inherits those disagreements. Flag the disagreement at the relevant rungs rather than papering over it.

**No novel theory.** The skill produces structured assessment of existing theory against cross-rung consistency. Where existing theory is wrong in a way no rung reveals, the skill does not catch the error, and the user is responsible for recognising the limit.

**Synthesis with critical-thinking, not subsumption.** The skill synthesises with critical-thinking in Phase II but does not subsume it. For non-causal queries that require claim verification, counterargument, and inversion, the user invokes critical-thinking directly rather than routing through this skill.
