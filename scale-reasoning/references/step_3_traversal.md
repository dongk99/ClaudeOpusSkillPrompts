# Step 3 — Causal Traversal with Divergence Handling

```
─── GATE: VERIFY S1–S2 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 1 — REFERENCE_RUNG] emitted
☐ [STEP 2 — LADDER] emitted with AXIS, DEPTH, RUNGS, DOMAIN_TAGS, CONVENTION_FLAGS
☐ Single axis named — no multi-axis hybrid
If unchecked → STOP. Return to the missing step.
```

## Purpose

Traverse the ladder causally in the direction the question requires. Locate divergences. Find convergence points. **Lock to the S2 axis.** If the explanation cannot lie on the chosen axis, declare axis-failure rather than producing a multi-axis hybrid.

## Inputs

- `[STEP 1 — REFERENCE_RUNG]`, `[STEP 2 — LADDER]`

## Procedure

**1. Determine traversal direction.**
- Higher-rung phenomenon explained from below: traverse **upward**. Lower rungs supply mechanism; upper rung is the explanandum.
- Lower-rung mechanism producing higher-rung outcome: traverse **downward**. Upper rung sets constraint; lower rung supplies substrate.
- Lateral comparison at the same rung: traverse **both** directions to establish that the comparison rests on consistent underlying mechanisms.

**2. Check for unintuitive direction.** In several mature fields, the test of an upper-rung observable runs through a lower-rung theory rather than the other way around — cosmological phenomena tested against particle-physics and condensed-matter at scales many decades smaller; evolutionary claims tested against molecular biology and biochemistry; macroeconomic claims tested against microeconomic and behavioural-science evidence at the agent level. **When uncertain whether the dominant test direction in a field runs upward or downward, web-search the relevant literature** to detect directional blindness rather than defaulting to the surface reading of the question.

**3. Handle divergence.** When the causal explanation diverges between adjacent rungs — the behaviour at one rung fails to produce the behaviour at the next, or the upper-rung outcome cannot be reconstructed from the lower-rung mechanism — flag the divergence explicitly and **descend along the same axis** to lower rungs to locate the convergence point. The convergence point is the rung at which the divergent behaviours collapse into a single substrate-level explanation.

**4. Lock to the axis.** The descent is constrained to the original S2 axis. **Do not switch axes mid-descent.** Do not, for example, start on a behavioural-aggregation axis and switch to a biological-organisation axis when the mechanism crosses from behaviour into physiology. If the phenomenon resists explanation along the original axis, **declare axis-failure** — name the axis on which the explanation would lie, and return to the user with that finding rather than producing a multi-axis hybrid that obscures the structure of the failure.

**5. Tag mainstream theory at each traversed rung.** Distinguish mainstream from alternative based on textbook authority, peer-reviewed literature, and weight of citation. **Not deferred to popularity alone** — established consensus and frontier dispute often disagree.

## Output (required emission)

```
[STEP 3 — TRAVERSAL_DIRECTION: up | down | both | unintuitive (and named)]
[STEP 3 — DIVERGENCES: <rung pair> + brief description, ... | none]
[STEP 3 — CONVERGENCE_RUNG: <rung> | not-found-on-axis]
[STEP 3 — MAINSTREAM_TAGS: rung_k → <theory>; ...]
```

If the explanation cannot lie on the S2 axis:

```
[STEP 3 — AXIS_FAIL: <S2 axis> insufficient.
                    EXPLANATION_AXIS: <name correct axis>.
                    REASON: <brief>.]
```

Axis-failure terminates Phase I. Return the axis-failure report to the user. Do not proceed to Phase II.

## Forward-compat

`phase_2_triggers.md` reads `DIVERGENCES`, `CONVERGENCE_RUNG`, and `MAINSTREAM_TAGS` to evaluate autonomous triggers (unresolved divergence is the first trigger). S4 search targets `MAINSTREAM_TAGS` to find alternatives at the same rung. S5 substitution preserves the same direction and divergence structure with the alternative substituted.
