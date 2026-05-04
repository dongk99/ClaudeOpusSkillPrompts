# Step 6 — Verdict

```
─── GATE: VERIFY S5 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 5 — JACCARD] emitted
☐ Aggregate rates per excluded term are stated
☐ Per-alternative rates are preserved
If unchecked → STOP. Return to step_5_aggregate.md.
```

## Purpose

Apply thresholds to the aggregate and per-alternative overlap rates from S5 to produce one of three terminal verdicts: separated, mixed, or paraphrase. The verdict is the load-bearing output of the skill — the bracket blocks above are the audit trail; this is the conclusion the user reads.

## Inputs

- [STEP 5 — JACCARD] aggregate rates and per-alternative rates
- The excluded set from S1 (used in the rationale)
- The alternatives from S2 (used in the rationale)

## Procedure

**1. Apply aggregate thresholds.**

- **separated** — all excluded-term aggregate rates are below 20%. The alternatives carve out distinct vocabulary space across the literature sample; the categorical-separation claim survives.
- **paraphrase** — any excluded-term aggregate rate is at or above 50%, OR multiple excluded terms are at or above 35%. The alternatives live inside the excluded vocabulary's literature ecosystem; the categorical-separation claim fails.
- **mixed** — neither condition is met. Some alternatives or excluded terms cross thresholds and others don't. The alternatives partially separate.

**2. For the mixed verdict, identify the separating subset.** Read the per-alternative rates from S5. Name which specific alternatives have low overlap (the candidates the user might keep) and which have high overlap (the candidates the user should drop). The mixed verdict is not a hedged failure — it is a partial-pass with explicit identification of which subset passed.

**3. Generate the rationale.** One sentence naming the threshold(s) crossed (or not crossed) and the specific aggregate rates that drove the verdict. The rationale is what makes the verdict legible without reading the full audit trail.

**4. For paraphrase or mixed verdicts, generate the concession sentence.** This is required, not optional. The concession states plainly:
- That the alternatives (or the failing subset) do not carve the claimed separation
- What substrate-honest framing the user could use instead — typically: "the activity *is* [excluded term] under [some specific qualifier], not a categorically distinct activity"

The concession exists because the skill's purpose is to produce honest framing, not to leave the user in vocabulary limbo. Do not soften, do not hedge, do not offer additional alternatives at this step (per Bad Ending: Verdict laundering in SKILL.md). If the user wants to test new alternatives, the skill must be re-run from S2.

**5. For separated verdicts, do not pad.** State the verdict plainly. Do not name what the failure mode would have been if conditions had differed. Acquittal is a clean exit.

## Output (required emission)

For separated:

```
[STEP 6 — VERDICT: separated]
  RATIONALE: All excluded-term aggregate rates < 20% (max observed: <rate>% on <term>). Alternatives carve distinct vocabulary space.
  PROCEED: The categorical-separation claim is supported by the literature sample. Use the alternatives as proposed.
```

For mixed:

```
[STEP 6 — VERDICT: mixed]
  RATIONALE: <term> aggregate rate at <rate>% (above threshold); other terms below threshold. Partial separation only.
  SEPARATING_SUBSET: <list of alternatives with low overlap across all excluded terms>
  FAILING_SUBSET: <list of alternatives with high overlap on at least one excluded term>
  CONCESSION: <one to three sentences — name the partial separation, name the substrate-honest framing for the failing subset>
```

For paraphrase:

```
[STEP 6 — VERDICT: paraphrase]
  RATIONALE: <term> aggregate rate at <rate>% (above 50% threshold) [or: multiple terms above 35%]. Alternatives live inside excluded-term literature ecosystem.
  CONCESSION: <one to three sentences — state plainly that the alternatives do not carve the claimed separation, name the substrate-honest framing the user would use instead>
```

## Forward-compat

S6 is terminal. The skill exits after the verdict is emitted. If the user wants to test additional alternatives, the skill is re-run from S2 with the new candidate set — S1's excluded set typically does not change between runs.

The verdict block is the user-facing artifact. The bracket blocks from S1–S5 serve as the audit trail and should remain visible in the conversation log so the user can verify the verdict was produced by the procedure rather than asserted from prior intuition. Removing the audit trail to compress the response is a violation of the skill's transparency principle and produces a verdict the user cannot independently check.
