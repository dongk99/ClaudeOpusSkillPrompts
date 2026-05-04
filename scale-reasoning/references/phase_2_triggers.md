# Phase II Triggers — Autonomous Invocation Gate

```
─── GATE: VERIFY S1–S3 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 3 — TRAVERSAL_DIRECTION] emitted
☐ [STEP 3 — DIVERGENCES] and [STEP 3 — CONVERGENCE_RUNG] emitted
☐ [STEP 3 — MAINSTREAM_TAGS] emitted
☐ NOT [STEP 3 — AXIS_FAIL] (axis-failure terminates Phase I; Phase II does not run)
If [AXIS_FAIL] emitted → STOP. Phase I terminated. Do not enter Phase II.
```

## Purpose

Determine whether Phase II runs. Phase II runs in two circumstances: **explicit user request**, or **autonomous invocation when any trigger below is satisfied**. The trigger list is intentionally expansive — under-triggering produces unfalsified output that looks confident; over-triggering produces redundant falsification the user can dismiss in a sentence. **The user policy is to err on the side of over-triggering.**

## Inputs

- `[STEP 3 — DIVERGENCES]`, `[STEP 3 — CONVERGENCE_RUNG]`, `[STEP 3 — MAINSTREAM_TAGS]`
- The full mandatory-phase reasoning trace (read as ambient context)

## Triggers — Phase II Runs Autonomously If Any Hold

**Trigger 1 — Unresolved divergence from S3.** If the descent to a convergence point did not produce a rung at which the divergent behaviours reconcile, the divergence is unresolved.

**Trigger 2 — Unverified empirical claim relied on during S3.** If the causal traversal rested on a claim that could not be verified with web search, or could not be sourced to textbook or peer-reviewed authority.

**Trigger 3 — Broad epistemic doubt.** Phase II runs whenever, in the course of producing the mandatory-phase output, the question "is this right" is being entertained. **A feeling rather than a single condition, deliberately so.** Concrete signals that activate this trigger include:

- The phenomenon is contested in the literature in a way detected during S2.
- Multiple incompatible theories exist at any rung of the ladder and one was chosen without strong justification.
- The reasoning chain involves cross-domain analogy ("X works like Y because both have Z") rather than direct empirical grounding.
- The chain extrapolates beyond the regime in which the underlying theories were validated.
- The chain spans rungs separated by many orders of magnitude with intermediate mechanisms left implicit.
- The phenomenon sits at the frontier of an active research area where consensus has not settled.
- Quantitative claims with order-of-magnitude ranges appear that cannot be verified.
- The chain crosses a boundary between effective theories where it is uncertain which framework dominates.
- The user's query implied an answer not certain to match mainstream consensus.
- Confidence was reached for where evidence was thin.
- The chain made a logical leap that bridged domains without explicit textbook support.
- Any analogous condition under which a careful reader would ask "is this right" of the output.

## Output (required emission)

```
[PHASE_II: triggered]
[TRIGGER_REASONS: <list of triggers that fired, with brief grounding>]
```

OR

```
[PHASE_II: skipped]
[GROUND: <brief justification — no triggers fired, no user request, mandatory-phase output is well-grounded>]
```

If skipped: the skill terminates after the mandatory-phase synthesis. Return the Phase I prose to the user.

If triggered: proceed to `step_4_alternatives.md`.

## Forward-compat

S4 reads `MAINSTREAM_TAGS` to scope the alternative-theory search. S5 will re-run S3 with each alternative substituted. S6 reports both side-by-side with explicit gap disclosure. The `TRIGGER_REASONS` list surfaces in S6 as part of the gap-disclosure record — the user sees why Phase II was deemed necessary.

**Token-spend note.** Token-spend optimization pulls toward skipping Phase II when triggers are borderline. The skill's design fights that pull. Borderline cases trigger.
