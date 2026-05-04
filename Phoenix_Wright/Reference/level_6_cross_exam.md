# Level 6 — Cross-Examination

```
─── GATE: VERIFY LEVELS 1–5 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [Level 1 — CASE FILES] emitted
☐ [Level 2 — CROSS-EXAM SCOPE] emitted (not skip)
☐ [Level 3 — OUTPUT MODE] emitted
☐ [Level 4 — CLIENT STATEMENT] emitted with CLIENT CONFIRMED: yes
☐ [Level 5 — FRESH SUBSTRATE] emitted (statement did not exit at Level 5)
If unchecked → return to the missing Level.
Do not continue reading this file.
```

## Purpose

Compare the witness's stated goal or capability (from the testimony under suspicion) against the actual capability evidence (the Level 5 fresh substrate). One cross-examination per in-scope statement.

The cross-examination is structural: it measures the gap between stated and actual on the named axis. Surface-similarity is not admissible. Topic overlap is not admissible.

## Inputs

- `[Level 4 — CLIENT STATEMENT]` — claim, axis
- `[Level 5 — FRESH SUBSTRATE]` — actual capability evidence

## Procedure

For each statement that did not exit at Level 5:

1. **Extract TESTIMONY from the claim under suspicion.** What does the witness claim the substrate intends to do, can do, has done, or will do? State it as a measurable proposition where possible. Examples:
   - "8000-hour operational life at south pole, year one"
   - "Reactor delivers 20 kW electrical for Mars cruise"
   - "Bug bounty intake form accepts external researcher submissions on equal footing"

2. **Extract EVIDENCE from Level 5 fresh substrate.** What does the substrate evidence support? State it as a measurable proposition. Examples:
   - "11 hours total LRV operation across all Apollo missions, equatorial mare terrain"
   - "First US fission system since 1965, no flight-qualified Brayton-cycle precedent"
   - "Bug bounty intake form scope explicitly excludes <X>, requires NDA, invite-only"

3. **Run the cross-examination.** Compare TESTIMONY and EVIDENCE on the AXIS. Produce one of four outcomes:
   - **GAP CONFIRMED** — testimony and evidence are quantifiably different on the axis. Magnitude estimable.
   - **NO GAP** — evidence supports the testimony within the precision of the substrate.
   - **PARTIAL GAP** — evidence supports testimony on dimension A, fails to support on dimension B. Both dimensions named.
   - **GATE INDETERMINATE** — substrate retrieved at Level 5 does not discriminate the axis. State explicitly. Do not collapse to NO GAP by default.

4. **Name the RUNG.** Identify the rung at which the gap operates: duration rung, geological rung, regulatory rung, technological-readiness rung, institutional-history rung, mechanism rung, etc.

5. **Estimate MAGNITUDE.** Where measurable, name the magnitude of the gap (e.g., "~800× duration extrapolation"). Where not directly measurable, name the qualitative scale (e.g., "novel regime, no comparable precedent").

## Court Rules

**Court Rule 4 — No Padding on Acquittal.** A NO GAP outcome is stated plainly. The skill does not name what the gap would have been if conditions had differed.

**Court Rule 5 — Indeterminate is Not a Side.** GATE INDETERMINATE outcomes are not collapsed to NO GAP or to GAP CONFIRMED. The client can re-scope at Level 2 with broader retrieval if desired.

**Court Rule 6 — Partial Gaps Name Both Dimensions.** A partial outcome that only names the failing dimension is incomplete; the supported dimension is stated explicitly.

## In-Court Call-Out — OBJECTION!

**OBJECTION!** fires when the structured emission below has TESTIMONY, EVIDENCE, GAP, and RUNG all populated with non-empty content, AND the GAP names a contradiction that the EVIDENCE does not resolve. If GAP is empty or restates EVIDENCE without naming a contradiction, no OBJECTION line emits. The closing for that statement is "no discrepancy on the record."

The call-out does not fire on inference or vibe. It fires only on a fully populated emission with a contradiction named in GAP.

## Output (required emission)

```
[Level 6 — CROSS-EXAMINATION]
  STATEMENT 1:
    TESTIMONY: <measurable proposition extracted from suspect claim>
    EVIDENCE:  <measurable proposition extracted from Level 5 substrate>
    AXIS:      <restated from Level 4>
    OUTCOME:   <GAP CONFIRMED | NO GAP | PARTIAL GAP | GATE INDETERMINATE>
    GAP:       <one-sentence naming of the contradiction, or "none">
    RUNG:      <named rung at which the gap operates>
    MAGNITUDE: <quantitative estimate or qualitative scale>
    SUBSTRATE BASIS: <which Level 5 data points supported this outcome>
  STATEMENT 2:
    ...
```

When OBJECTION! fires (GAP CONFIRMED, contradiction named):

```
[Level 6 — CROSS-EXAMINATION]
  STATEMENT 1:
    TESTIMONY: <stated>
    EVIDENCE:  <stated>
    AXIS:      <restated>
    OUTCOME:   GAP CONFIRMED
    GAP:       <contradiction named in one sentence>
    RUNG:      <named>
    MAGNITUDE: <named>
    SUBSTRATE BASIS: <listed>

>>> OBJECTION! <one-line statement: what the witness said vs what the
    evidence shows.>
```

## Forward-compat

Level 7 reads the cross-examination map. In verdict mode, Level 7 emits the per-statement verdict with magnitude, rung, and falsification condition. In substrate mode, Level 7 emits the Level 5 data and the Level 6 map together as raw material.

The cross-examination is the structural commitment of the skill. A cascade that runs Level 5 but skips Level 6 has produced a substrate dump without an audit. A cascade that runs Level 6 with surface-similarity comparison has produced a vibes-based verdict. Neither is the skill's intended output.
