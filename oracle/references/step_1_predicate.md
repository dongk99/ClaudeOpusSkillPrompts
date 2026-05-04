# Step 1 — Detect Prediction Query and Name the Predicate

```
─── GATE: ENTRY POINT ───
This is the entry step. No upstream gate.
Confirm before reading further:
☐ Skill triggered by forward-looking / hypothetical / counterfactual query
☐ Scale-reasoning is loaded in parent context (else: refuse, do not proceed)
If unloaded → emit refusal, do not continue.
```

## Purpose

Read the query for the proposition whose truth-value the user is asking about across some future or hypothetical state. Name it in plain language.

## Inputs

- User query (current turn)

## Procedure

1. Identify the predicate — the proposition whose truth across a future, hypothetical, or counterfactual state is being interrogated.
2. State it in plain language. Strip rhetorical scaffolding; keep the operational claim.
3. If the query contains multiple predicates, name each separately. Each parallel predicate is a parallel invocation of the skill — do not collapse.
4. Do not assign a mode at this step. Mode assignment is S2's responsibility and depends on the named predicate, not on intuition about the query.

## Output (required emission, parent only)

Emit visibly in the response:

```
[STEP 1 — PREDICATE: <plain-language statement of the proposition>]
```

For multiple predicates, emit one block per predicate, indexed:

```
[STEP 1 — PREDICATE 1: ...]
[STEP 1 — PREDICATE 2: ...]
```

## Forward-compat

S2 reads the predicate to assign mode. S3 reads the predicate to extract rung structure. S4 retrieval prompts embed the predicate as inert text. S5 gate references the predicate when checking rung correspondence. S7 synthesis references the predicate as the prediction's grammatical subject.

A misnamed or rhetorically-loaded predicate propagates through every downstream step. Re-read the user query if uncertain.
