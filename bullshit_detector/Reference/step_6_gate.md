# Step 6 — Goal-vs-Capability Discrepancy Gate

```
─── GATE: VERIFY S1–S5 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [STEP 1 — BACKWARD SCAN] emitted
☐ [STEP 2 — SCOPE] emitted (not skip)
☐ [STEP 3 — OUTPUT MODE] emitted
☐ [STEP 4 — SUSPICION PREDICATE] emitted with USER CONFIRMED: yes
☐ [STEP 5 — FRESH SUBSTRATE] emitted (predicate did not exit at S5)
If unchecked → STOP. Return to the missing step.
Do not continue reading this file.
```

## Purpose

Compare adversary's stated goal/capability (extracted from the suspect Claude claim) against actual capability evidence (the S5 fresh substrate). Output a discrepancy map per predicate.

The gate is structural: it measures the gap between stated and actual on the named axis. Surface-similarity is not admissible. Topic overlap is not admissible.

## Inputs

- `[STEP 4 — SUSPICION PREDICATE]` — claim under suspicion, axis of disagreement
- `[STEP 5 — FRESH SUBSTRATE]` — actual capability evidence

## Procedure

For each predicate that did not exit at S5:

1. **Extract the stated side from the claim under suspicion.** What does the adversary claim it intends to do, can do, has done, or will do? State it as a measurable proposition where possible. Examples:
   - "8000-hour operational life at south pole, year one"
   - "Reactor delivers 20 kW electrical for Mars cruise"
   - "Bug bounty intake form accepts external researcher submissions on equal footing"

2. **Extract the actual side from S5 fresh substrate.** What does the substrate evidence support? State it as a measurable proposition. Examples:
   - "11 hours total LRV operation across all Apollo missions, equatorial mare terrain"
   - "First US fission system since 1965, no flight-qualified Brayton-cycle precedent"
   - "Bug bounty intake form scope explicitly excludes <X>, requires NDA, invite-only"

3. **Run the gate.** Compare stated and actual on the AXIS OF DISAGREEMENT. The gate produces one of four outcomes:

   - **DISCREPANCY CONFIRMED** — stated and actual are quantifiably different on the named axis. Magnitude estimable.
   - **NO DISCREPANCY** — actual supports the stated claim within the precision of the substrate.
   - **PARTIAL DISCREPANCY** — actual supports stated on dimension A, fails to support on dimension B. Both dimensions named.
   - **GATE INDETERMINATE** — substrate retrieved at S5 does not discriminate the axis. State explicitly. Do not collapse to NO DISCREPANCY by default.

4. **Name the rung.** Identify the rung at which the discrepancy operates: duration rung, geological rung, regulatory rung, technological-readiness rung, institutional-history rung, mechanism rung, etc.

5. **Estimate magnitude.** Where measurable, name the magnitude of the gap (e.g., "~800× duration extrapolation"). Where not directly measurable, name the qualitative scale (e.g., "novel regime, no comparable precedent").

## Hard Rules

**No-discrepancy outcomes are not softened.** If the substrate supports the claim, state it plainly.

**Indeterminate outcomes are not collapsed to a side.** If the substrate cannot discriminate the axis, the verdict is indeterminate. The user can re-scope at S2 with broader retrieval if desired.

**Partial discrepancies require both dimensions named.** A partial verdict that only names the failing dimension is incomplete; the supported dimension must be stated explicitly to prevent overclaiming.

## Output (required emission)

```
[STEP 6 — DISCREPANCY MAP]
  PREDICATE 1:
    STATED: <measurable proposition extracted from suspect claim>
    ACTUAL: <measurable proposition extracted from S5 substrate>
    AXIS: <restated from S4>
    OUTCOME: <DISCREPANCY CONFIRMED | NO DISCREPANCY | PARTIAL DISCREPANCY | GATE INDETERMINATE>
    RUNG: <named rung at which the discrepancy operates>
    MAGNITUDE: <quantitative estimate or qualitative scale>
    SUBSTRATE BASIS: <which S5 data points supported this outcome>
  PREDICATE 2:
    ...
```

## Forward-compat

S7 reads the discrepancy map. In verdict mode, S7 emits the per-predicate verdict with magnitude, rung, and falsification condition. In substrate mode, S7 emits the S5 data and the S6 map together as raw material for the user to interpret.

The gate is the structural commitment of the skill. A skill that runs S5 but skips S6 has produced a substrate dump without an audit. A skill that runs S6 with surface-similarity comparison has produced a vibes-based verdict. Neither is the skill's intended output.
