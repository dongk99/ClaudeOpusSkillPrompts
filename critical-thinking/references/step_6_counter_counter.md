# Step 6 — Counter-Counter Argument

```
─── GATE: VERIFY S4–S5 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 4 — INVERSION] with POST form emitted
☐ [STEP 5 — FALSIFICATION_CHECK] with PER_SOURCE_CATEGORIES emitted
☐ [STEP 5 — CONFLICTORY] flag emitted (yes or no)
If unchecked → STOP. Return to the missing step.
```

## Purpose

Test whether the **inverted side survives** independently of the user's original claim. From S5's findings, ask: does the post-inversion claim hold on its own terms? The structural complement of S5 — S5 asked whether the original claim's evidence also supports the inverted claim; S6 asks whether the inverted claim has standing in its own right.

## Inputs

- `[STEP 4 — INVERSION]` (POST form)
- `[STEP 5 — FALSIFICATION_CHECK]` (PER_SOURCE_CATEGORIES)
- `[STEP 5 — CONFLICTORY]` and CONFLICTORY_DETAIL

## Procedure

**1. Identify the alternative mechanisms surfaced at S5.** Each `post-supporting` or `both-supporting` source implies one or more alternative mechanisms by which A could occur. Name each alternative explicitly using domain-specific vocabulary from the S1 field tags.

**2. Apply the counter-counter test.** For each alternative mechanism, ask: *Does the consequence A still happen via this alternative, even when the user's named (Y, Z) pair is fully removed from the picture?*

This is a stronger test than S5's both-supporting check. S5 asked whether the same source supports both. S6 asks whether the alternative survives **without the user's mechanism present at all**.

**3. Categorize each alternative.**
- **Survives counter-counter** — the alternative produces A independently of (Y, Z). The inverted side has standing.
- **Collapses without the user's mechanism** — the alternative looked independent at S5 but on closer reading requires (Y, Z) implicitly. The inverted side does not have standing.
- **Underdetermined** — the source cannot be read clearly enough to decide.

**4. Aggregate.** If any alternative survives counter-counter, the inverted side has documented standing. If all alternatives collapse, the conflictory signal from S5 weakens. If multiple alternatives are underdetermined, the conflictory signal persists into S7 as unresolved.

**5. Do not synthesize a verdict here.** S6's job is to test the inverted side's standing, not to render a final supported / not-supported / conflictory call. That is S7's role, after the second web_search.

## Output (required emission)

```
[STEP 6 — COUNTER_COUNTER]
  ALTERNATIVES_TESTED:
    <alternative_1>: survives | collapses | underdetermined
    <alternative_2>: ...
  INVERTED_SIDE_STANDING: documented | weakened | unresolved
[STEP 6 — CONFLICTORY_PERSISTS: yes | no | unresolved]
```

`CONFLICTORY_PERSISTS` is the carry-forward for S7. It is `yes` if at least one alternative survives, `no` if all alternatives collapse, `unresolved` if any alternative is underdetermined and none clearly survive.

## Forward-compat

S7's second web_search is constructed against the alternatives that survived or are underdetermined here. The search targets fresh sources distinct from S1 — the goal is to check whether the inverted side's standing holds when tested against literature outside the S1 source set.

S7 reads `CONFLICTORY_PERSISTS` to inform its 3-state exit: `unresolved` typically yields `EXIT: conflictory` unless S7's fresh sources clearly resolve the question.

**Survival-bias trap.** A surviving alternative is not by itself a falsification of the user's claim. It is documentation that A has at least two pathways. The user's pathway and the alternative may both be valid in their respective regimes. S7 will check the regime question.
