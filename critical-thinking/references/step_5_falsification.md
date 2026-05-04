# Step 5 — Falsification Check Against S1 Evidence

```
─── GATE: VERIFY S1–S4 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 1 — INITIAL_SEARCH] with SOURCES_RETAINED emitted
☐ [STEP 2 — EVAL_RESULT] and EVAL_DETAIL emitted
☐ [STEP 4 — INVERSION] with PRE, POST, and COMPONENTS emitted
If unchecked → STOP. Return to the missing step.
```

## Purpose

Test the post-inversion question against the S1 evidence base. Determine whether the consequence (A) is supported when the cause-mechanism pair (Y, Z) is removed. **If S1 evidence supports both the pre-inversion AND the post-inversion claim, flag CONFLICTORY** — this is a load-bearing exit signal of the skill.

## Inputs

- `[STEP 1 — INITIAL_SEARCH]` (the source set)
- `[STEP 4 — INVERSION]` (PRE and POST forms)
- `[STEP 2 — EVAL_DETAIL]` (the per-source evidence trace)

## Procedure

**1. Re-read S1 sources against the post-inversion question.** For each source retained at S1, ask: *Does this source provide evidence that A occurs even when (Y, Z) is absent or replaced by alternative mechanisms?*

This is a **second pass** over the same sources, with a different question. Sources that supported the pre-inversion claim may also support the post-inversion claim — that is the conflictory signal, not a contradiction.

**2. Categorize each source.**
- **Pre-supporting only** — supports the user's original claim; silent or against the post-inversion.
- **Post-supporting only** — supports the post-inversion (A occurs via other pathways); silent or against the pre.
- **Both-supporting** — provides evidence for both pre and post. **This is the CONFLICTORY signal.**
- **Neither-supporting** — silent on both.

**3. Apply the conflictory rule (canonical definition).**

> CONFLICTORY: After inversion, presented data supports BOTH pre-inversion and post-inversion claims.

If any S1 source is `both-supporting`, the falsification check returns `CONFLICTORY: yes`. Multiple both-supporting sources strengthen the conflictory signal but a single one is sufficient to raise the flag.

**4. Do not collapse conflictory into a verdict.** Conflictory is **not** "the user is wrong" and not "the user is right." It is the structural finding that the consequence A is not uniquely produced by the user's named (Y, Z) pair. The user's claim may still be correct in its own terms; the conflictory flag indicates that the evidence base does not discriminate between the user's mechanism and at least one alternative.

**5. Continue to S6 regardless of conflictory state.** Even when CONFLICTORY: no, S6 runs the counter-counter argument as a structural check.

## Output (required emission)

```
[STEP 5 — FALSIFICATION_CHECK]
  PER_SOURCE_CATEGORIES:
    <source_1>: pre-supporting | post-supporting | both-supporting | neither-supporting
    <source_2>: ...
  ...
[STEP 5 — CONFLICTORY: yes | no]
[STEP 5 — CONFLICTORY_DETAIL: <if yes, which sources support both, and the alternative mechanism each implies>]
```

## Forward-compat

S6 counter-counter argument tests whether the post-supporting sources individually survive when re-read with the user's pre-inversion mechanism re-imposed. S7 second search constructs queries targeting the alternative mechanisms surfaced by `CONFLICTORY_DETAIL` — searching for fresh sources that confirm or falsify the alternatives. The 3-state exit at S7 inherits the CONFLICTORY flag if it is not resolved by S7's second search.

**Both-supporting trap.** When a source supports both pre and post, the temptation is to interpret it as supporting only the user's original claim because the user's claim was the prior frame. Resist. If A occurs via mechanism Z in some passages and via alternative mechanisms in other passages, the source is `both-supporting`, not `pre-supporting`.
