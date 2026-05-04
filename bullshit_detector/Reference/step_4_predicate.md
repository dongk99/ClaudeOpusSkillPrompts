# Level 4 — User States Suspicion Plainly (Free-Form)

```
─── GATE: VERIFY S1–S3 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [Level 1 — BACKWARD SCAN] emitted
☐ [Level 2 — SCOPE] emitted (not skip)
☐ [Level 3 — OUTPUT MODE] emitted
If unchecked → STOP. Return to the missing Level.
Do not continue reading this file.
```

## Purpose

User states the suspicion in plain language, free-form. Claude parses three load-bearing pieces from the statement. If any piece is missing, Claude requests it by name and waits.

## Inputs

- `[Level 2 — IN-SCOPE CLAIMS]`
- `[Level 3 — OUTPUT MODE]`

## Procedure

1. Request the user state the suspicion plainly. Free-form, no required format.

2. Wait for user response.

3. Parse the user's response for three pieces:

   - **(a) Claim under suspicion** — which Claude claim, identified specifically. May be a quoted phrase, a turn reference, or a paraphrase that uniquely identifies the claim. If multiple claims are in scope, the user may name multiple suspicions in one statement.

   - **(b) Axis of disagreement** — the dimension along which the user thinks the claim is wrong. Common axes: stated goal vs actual capability, stated duration vs actual operating data, stated environmental tolerance vs actual environmental conditions, stated mechanism vs actual mechanism, stated precedent vs actual precedent.

   - **(c) Synthesized prior flags** — what the user noticed across prior turns that produced the suspicion. May be implicit in the statement if the user is referencing earlier conversation directly.

4. If any piece is missing, request the missing piece by name. Do not infer. Do not proceed.

   Example: "The axis of disagreement isn't named. Which dimension do you think the claim is wrong on — duration, environment, mechanism, precedent, something else?"

5. When all three pieces are present, restate them in structured form for confirmation. User confirms or corrects. Do not proceed until confirmation is explicit.

## Output (required emission)

After user statement is parsed and confirmed:

```
[Level 4 — SUSPICION PREDICATE]
  CLAIM UNDER SUSPICION: <quoted or paraphrased; turn reference if applicable>
  AXIS OF DISAGREEMENT: <named dimension; standard form when applicable>
  SYNTHESIZED PRIOR FLAGS: <list of observations from prior turns that produced suspicion>
  USER CONFIRMED: <yes>
```

For multiple suspicions in one user statement:

```
[Level 4 — SUSPICION PREDICATE 1]
  ...
[Level 4 — SUSPICION PREDICATE 2]
  ...
```

## Forward-compat

S5 retrieves substrate fresh, targeted at the AXIS OF DISAGREEMENT for each predicate. S6 runs the discrepancy gate by comparing AXIS-relevant data from S5 against the CLAIM UNDER SUSPICION. S7 emits per-predicate verdict or substrate dump.

A misnamed AXIS OF DISAGREEMENT propagates through every downstream Level. The S4 confirmation Level exists to catch this before the cascade burns retrieval cost on the wrong axis.
