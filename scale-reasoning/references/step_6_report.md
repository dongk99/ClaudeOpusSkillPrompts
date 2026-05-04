# Step 6 — Equal-Treatment Report with Explicit Gap Disclosure

```
─── GATE: VERIFY S5 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 5 — SUBSTITUTION] emitted with per-alternative survival map
   (or [STEP 4 — NO_ALTERNATIVE_FOUND] for every rung — report the absence as central finding)
☐ Source-distinctness flags from S4 preserved through S5
If unchecked → STOP. Return to step_5_resubstitute.md.
```

## Purpose

Produce the user-facing Phase II report. Mainstream and alternative theories side-by-side. Rungs at which each is consistent; rungs at which each is not. **Equal treatment.** Explicit gap disclosure. **No prescribed verdict.**

## Inputs

- `[STEP 3 — MAINSTREAM_TAGS]` and the S3 traversal record
- `[STEP 4 — ALTERNATIVES]` with source-distinctness flags
- `[STEP 5 — SUBSTITUTION]` with per-alternative survival maps
- `[PHASE_II — TRIGGER_REASONS]` (surfaces in the gap-disclosure section)

## Procedure — Required Disclosures

The report names, **explicitly**:

1. **Every gap in the literature** on which the analysis relied.
2. **Every leap of inference** made to bridge rungs where direct evidence was unavailable.
3. **Every rung at which scale consistency was assumed rather than established.**
4. **Every claim of fundamental physical plausibility** that could not be verified.
5. **Every cross-domain analogy** that carried explanatory weight without textbook grounding.
6. **Every place where the mainstream theory is preferred only by weight of citation** rather than by survival of the scale-consistency test.
7. **Every place where the alternative survived the scale-consistency test as well as the mainstream theory did**, leaving the choice between them genuinely open.

## Output Format

The output is structured by **alternative-theory**, not by rung. Each alternative is presented with its rung-by-rung consistency check, its falsifying or surviving rungs named, and its standing relative to the mainstream theory stated in plain language.

The mainstream theory is presented with the same grammar. Surviving rungs named. Any rung where mainstream relied on assumption rather than established mechanism is named with the same emphasis the alternative would receive for the same defect.

The `TRIGGER_REASONS` from `phase_2_triggers.md` surface here as part of the gap record — the user sees why Phase II was deemed necessary, not just what it found.

## Hard Rule — No Verdict

**The report does not prescribe a verdict.** It returns the user the information needed to render their own verdict, with the structure of the reasoning visible at every rung.

This is not a stylistic preference. It is the load-bearing commitment of the skill: equal-treatment side-by-side reporting depends on the report-writer not silently weighting one side. **Do not append a "best theory" recommendation. Do not fold the gaps into a confidence score that hides their structure. Do not soften the absence-of-alternative finding into a vindication of mainstream.**

## Output (required emission)

Prose response in the user's preferred register, embedding:

```
[STEP 6 — REPORT_DELIVERED]
[STEP 6 — DISCLOSURE_RECORD: <count of gaps named, count of inference leaps, count of assumption-rungs, count of unverified plausibility claims, count of cross-domain analogies>]
[STEP 6 — VERDICT: not-rendered (per skill design)]
```

## Forward-compat (terminal)

This is the terminal step of the conditional phase. If the user requests a verdict after seeing the report, the verdict belongs to them. If the user requests further substitution at additional rungs, return to S4 with the new alternative-theory targets — the ladder and axis from S2 remain locked.
