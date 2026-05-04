---
name: oracle
description: Prediction skill for forward-looking, hypothetical, or counterfactual queries, with multi-agent (orchestrator-worker) compatibility built in. Use this skill whenever the user asks what will happen, what would happen, what is likely to happen, or what would have happened — including geopolitical forecasts, cosmological-fate questions, market trajectories, first-contact hypotheticals, thought experiments (trolley problems, Maxwell's demon, Galileo's tower), and counterfactual histories. Trigger by default on any forward-looking or hypothetical query. Skip only for plainly descriptive or retrospective queries — what something is, what something was, how something works in steady state, definitional lookups, and arithmetic. The skill is an extension of scale-reasoning and depends on scale-reasoning being available in the parent (lead agent) context; subagents do not need it loaded.
---

# Oracle — Historical-Analog Retrieval Gated by Scale Coherence

Prediction queries get retrieved precedents (or constructed scenarios from established principles), gated rung-by-rung against the query's scale structure via the scale-reasoning skill. Three operating modes, three terminal exit states. Every prediction ties to a named precedent set, a named mode, and an explicit delta inventory at the rungs where precedent and query diverge. No free-running prediction.

The discipline: surface retrieval (keyword overlap, topic adjacency) is replaced with structural retrieval (rung-by-rung axis coherence). Most LLM prediction fails because it skips this gate.

## Mandatory File-Read Order

Read each step file in sequence. Each file contains a **GATE** block that verifies the prior step's emission exists in the conversation log. **Do not read step N+1 before step N's required output is emitted.** The gate is not a suggestion — it is the carry-forward verification that prevents fabricated execution.

```
references/step_1_predicate.md   → emits [PREDICATE]
references/step_2_mode.md        → emits [MODE] [GROUND]
references/step_3_rungs.md       → emits [RUNG: AXIS|REF|MECH|CONSTRAINT]   (invokes scale-reasoning)
references/step_4_retrieval.md   → emits [CANDIDATES]                       (parent dispatches subagents)
references/step_5_gate.md        → emits [SURVIVED] [REJECTED]              (invokes scale-reasoning, no delegation)
references/step_6_exit.md        → emits [EXIT: yes-predict|no-predict|invert-and-predict]
references/step_7_synthesis.md   → emits [PREDICTION] [FALSIFICATION] [CONFIDENCE]
references/phase_2_consistency.md → conditional, triggered by criteria in that file
```

## Operating Architecture (single-agent and multi-agent)

The skill runs equivalently under single-agent execution and under orchestrator-worker execution (Research feature, Agent SDK, Claude Code with Task delegation). Under multi-agent execution, the lead agent is the **parent** and is the skill-bearer; **subagents** are narrowly-scoped task workers that return structured findings. Subagents do not run the skill, do not load scale-reasoning, and do not make exit-state determinations.

**Parent retains, without delegation:** the predicate (S1), mode assignment (S2), rung structure (S3), axis-coherence gate (S5), exit-state determination (S6), synthesis (S7), Phase II triggers.

**Parent may delegate:** candidate retrieval (S4), per-domain consistency checks (Phase II).

The parent saves the rung structure (S3) to durable working memory before any subagent dispatch. Multi-agent orchestrators on long tasks truncate context above 200K tokens and route through Memory; the rung structure must survive that truncation. If no Memory mechanism is available, the parent retains the rung structure inline and accepts the budget cost.

## Invocation Rule

Trigger on any query whose grammatical structure implies forward time, hypothetical mood, conditional tense, or counterfactual framing — "what will," "what would," "what happens if," "what would have happened if," "predict," "forecast," "imagine that," "suppose," "consider a world in which," "what if X had not happened."

Skip only for plainly descriptive or retrospective queries — definitional lookups, steady-state mechanism, arithmetic, translation, settled historical fact. When skipping, state the skip and name the ground.

If scale-reasoning is unavailable in the parent context, state the unavailability and refuse to proceed with prediction. Subagents do not need scale-reasoning loaded; the parent passes scale-reasoning outputs to subagents as inert text.

## Output Conventions

Phase I output is prose in the user's preferred register. Prediction stated in a single passage with deltas and falsification conditions in full.

- **Skip on judgment:** state the skip in one sentence at top, name the ground.
- **No-predict exit:** state the refusal, name the missing precedent, offer the user the option to supply one or reframe. Do not soften, do not hedge into partial prediction, do not gesture at what a prediction might look like.
- **Invert-and-predict exit:** state the inversion, name the documented side and the underside, present the prediction with reduced-confidence notation.

## Caveats and Known Limitations

**Self-reference cap.** The skill's ceiling is the user's ceiling. Cannot retrieve precedents the user has not made available. Cannot detect axis-coherence failures along axes the user has not built into the rung structure.

**Filter inherits blind spots.** Phase II checks domains the parent recognises as adjacent. Domains the parent does not recognise are silently absent.

**Hybrid mode failure-prone.** Power comes from named deltas; named deltas can be undercounted or misweighted. The parent flags hybrid-mode predictions and surfaces the delta inventory for user audit.

**Subagent surface-similarity bias.** Subagents at S4 rate by surface similarity (no scale-reasoning loaded). Parent treats subagent assessments as advisory only. Failure to apply this caveat is the dominant failure mode under multi-agent execution.

**Token-spend pressure on no-predict.** Multi-agent orchestrators are optimised for token spend; refusal is the lowest-token output. The skill's no-predict exit fights the optimisation function. Spawning additional retrieval subagents to find weaker candidates after the gate empties is a violation, not a thoroughness improvement.

**Dual-use flag.** Same reliability that makes the skill useful for legitimate forecasting makes it useful for adversarial forecasting. Symmetric across user intent. The skill does not police use.

**No novel theory.** Predictions are grounded in existing precedents and existing principles. Where the substrate is incomplete in a way no axis-coherence check reveals, the skill will produce a coherent prediction on an incomplete substrate. The user is responsible for recognising the limit.

**Extension of scale-reasoning, not subsumption.** Once stable, this skill's logic is intended to merge into scale-reasoning's Phase II as a retrieval-and-substitution pathway. Until then, standalone extension; depends on scale-reasoning loaded in parent.
