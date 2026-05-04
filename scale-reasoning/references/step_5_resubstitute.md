# Step 5 — Re-run Scale Traversal with Alternatives Substituted

```
─── GATE: VERIFY S4 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 4 — ALTERNATIVES] emitted with at least one entry
   (or [STEP 4 — NO_ALTERNATIVE_FOUND] for every rung — in which case skip to S6)
☐ Source distinctness or shared-source flag recorded for each alternative
If unchecked → STOP. Return to step_4_alternatives.md.
```

## Purpose

For each alternative identified in S4, re-run the S3 causal traversal with the alternative substituted at its rung. Check whether the alternative preserves causal consistency across the ladder without violating fundamental physical laws or introducing scale inconsistencies that the mainstream theory did not have.

## Inputs

- `[STEP 2 — LADDER]` (axis remains locked)
- `[STEP 3 — TRAVERSAL_DIRECTION]`, `[STEP 3 — DIVERGENCES]`, `[STEP 3 — CONVERGENCE_RUNG]`
- `[STEP 4 — ALTERNATIVES]`

## Procedure

For each alternative theory in `[STEP 4 — ALTERNATIVES]`:

**1. Substitute at the rung.** Place the alternative at its rung in the S2 ladder. The S2 axis remains locked. Other rungs are not modified.

**2. Re-run S3 traversal.** Apply the same `[STEP 3 — TRAVERSAL_DIRECTION]`. Check whether the alternative produces the same explanandum, the same divergences, and the same convergence behaviour as the mainstream theory at adjacent rungs.

**3. Check scale consistency.** Determine, rung by rung, whether the alternative is consistent with the surrounding ladder. Failure modes to flag:
- The alternative requires energy gradients, particle counts, or constraints not supplied at adjacent rungs.
- The alternative produces a higher-rung outcome that the user's query does not actually observe.
- The alternative introduces a divergence the mainstream theory did not have.
- The alternative violates a fundamental physical law (conservation, second-law, causality).

**4. Equal treatment — load-bearing rule.** Do **not** discriminate between mainstream and alternative on the basis of popularity or institutional weight. Both are subjected to the same scale-consistency test. Both are reported on equal terms. Mainstream's surviving rungs and alternative's surviving rungs are recorded with the same grammar.

## Canonical Illustration

The silicon-lifeform-on-Earth case. Claim: silicon-based life could exist in a 300 K terrestrial environment. Substitution: silicon biochemistry at the relevant rung. Re-run downward: bond energetics, redox chemistry, catalytic accessibility. Result: silicon's bond network requires energy gradients the 300 K environment does not supply. Falsification is **structural** — it would survive any reordering of preferences. The alternative fails on scale-consistency, not on preference for carbon.

## Output (required emission)

```
[STEP 5 — SUBSTITUTION]
  alternative_a:
    SURVIVING_RUNGS: <list>
    FALSIFYING_RUNGS: <list with the inconsistency named>
    PHYSICAL_LAW_VIOLATIONS: <named | none>
    NEW_DIVERGENCES_INTRODUCED: <named | none>
  alternative_b:
    ...
```

For each alternative, the survival map names which rungs the alternative is consistent at, which rungs falsify it, and what specifically falsifies it (energy gradient, redox accessibility, etc.).

## Forward-compat

S6 takes the per-alternative survival map and renders it side-by-side against the mainstream theory's survival map. Equal grammar — the user sees both on the same terms.

**Self-confirmation trap.** If the alternative's "test" reuses the mainstream theory's evidence base (S4 source-distinctness violated and not flagged), the substitution will appear to fail at every rung where the mainstream succeeds, producing a false vindication of mainstream. Verify the source-distinctness flag from S4 before reporting falsification.
