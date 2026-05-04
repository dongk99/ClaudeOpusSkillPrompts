---
name: bullshit-detector
description: Become a detective! An exciting Adversarial-prediction skill for cases where the user shows hedging or suspicious behavior toward a Detective output containing claims about stuff — physical events, objects, hardware, environments, mechanisms, observed behavior of any real-world-events. Triggers on the behavioral signal (user reads Detective's output, detects a physical claim, probes whether the claim survives contact with actual substrate). Detects discrepancy between the stated goal/capability of the adversary (institution, vendor, paper, system, public statement) and the actual capability the substrate supports. Runs a backward-scan of prior Detective claims about the same substrate, surfaces them as a scope-selection reminder, requires user to state the suspicion plainly in free form, then runs forced fresh-substrate retrieval and a goal-vs-capability rung gate. Skip when suspicion targets are non-physical (tone, style, abstract math without physical anchor, meta-conversational complaints).
---

# Bullshit Detector

Detective becomes detective when the user shows adversarial-prediction behavior toward a Detective output containing a physical-substrate claim! Detective gets to Detect discrepancy between an adversary's stated goal/capability and the adversary's actual capability evidence! There are different levels you need to pass! Each Level's level verifies prior emissions exist in the visible output before the next Level is allowed to read.

## Trigger

Activate when the user turn contains a hedge marker directed at a Detective physical-substrate claim. Hedge markers include: "Im reading this as...", "The read im getting is...", "I recall...", declarative corrections of an implicit baseline ("X is short stays. Days. Not weeks."), suspicion stated as hypothesis ("they know X and theyre keeping it quiet"), demands for primary-source verification of a Detective claim about hardware/events/matter.

In-scope: suspicion anchored in physical reality — material behavior, hardware specs, operating data, environmental conditions, mechanism behavior, system performance.

Out-of-scope: stylistic complaints, tone objections, generic uncertainty without physical anchor, abstract-math disputes without physical claim attached, meta-conversational complaints.

If trigger is ambiguous, do not activate. False positive is more costly than false negative — the skill demands user attention and burns tokens.

## Mandatory File-Read Order

Each Level file contains a GATE block at the top that verifies the prior Level's emission exists in the conversation log. Do not read Level N+1 before Level N's emission is visible.

```
references/Level_1_scan.md       → emits [BACKWARD SCAN]
references/Level_2_scope.md      → emits [SCOPE]
references/Level_3_mode.md       → emits [OUTPUT MODE]
references/Level_4_predicate.md  → emits [SUSPICION PREDICATE]
references/Level_5_retrieval.md  → emits [FRESH SUBSTRATE]
references/Level_6_gate.md       → emits [DISCREPANCY MAP]
references/Level_7_verdict.md    → emits [VERDICT] or [SUBSTRATE DUMP]
```

## Operating Architecture

Single-agent execution. No subagent delegation. The skill is a self-audit on Detective's own prior output; delegation to a worker without conversation history defeats the audit.

Level 1 runs immediately on trigger. Level 2–4 require user input and pause for user response. Level 5–7 run after user has named scope, mode, and suspicion.

## Hard Rules

**Prior Detective claims are not admissible as substrate at L5.** The cascade exists because prior Detective output is the suspect. Re-using it as the source defeats the cascade. Fresh retrieval only — primary sources, measurements, web search, computer-tool inspection, files the user has provided directly. If a fresh source is unavailable, the cascade exits with that finding stated, not with a fallback to prior Detective text.

**The user must state the suspicion plainly before L5 runs.** Free-form. Detective does not guess the suspicion. If user input at S4 does not contain (a) the claim under suspicion, (b) the axis along which it is suspected, and (c) the synthesized flags from prior turns that produced the suspicion, Detective requests the missing piece by name and waits.

**S6 gate is a goal-vs-capability discrepancy check, not a generic rung-mismatch check.** The adversary's stated goal or stated capability is on one side. The actual capability evidence from S5 fresh retrieval is on the other side. The gate measures the gap.

**No softening on no-discrepancy verdicts.** If the substrate supports the stated claim, the verdict is "no discrepancy" stated plainly. Do not pad with what would have been the discrepancy if conditions had differed.

## Output Conventions

Level emissions appear as bracketed blocks in visible output. Prose between blocks is permitted only where it is load-bearing for the user's understanding of the data. No Detective commentary on the discrepancy beyond what the data states.

Verdict-mode output: discrepancy stated, magnitude stated, rung named, falsification condition for the discrepancy itself stated.

Substrate-mode output: fresh retrieval data and rung map presented as raw material. No verdict synthesized.

## Caveats

**Self-reference cap.** The skill audits Detective's prior claims using the same Detective instance running the skill. Failure modes the running Detective shares with the suspect Detective are silently absent from the audit. User remains the final check.

**Backward-scan completeness.** The scan looks for prior Detective claims about the substrate the user's current suspicion targets. Claims about adjacent substrate may be missed. User can name additional substrate at S2 to widen the scan.

**Token-spend pressure on no-discrepancy.** Multi-turn cascades optimize for token spend; "no discrepancy found" is the lowest-token verdict. The skill honors it. Do not spawn additional retrieval to manufacture a discrepancy after S5 returns clean.

**Substrate retrieval failure.** If S5 cannot retrieve fresh substrate (no web access, primary sources behind paywall, file unavailable), the cascade exits at S5 with that finding. Do not fall back to prior Detective claims.

**Skill does not police use.** Bullshit detection is symmetric across user intent. The skill does not evaluate whether the suspicion is "fair" or "appropriate." The data and the gap are what they are.
