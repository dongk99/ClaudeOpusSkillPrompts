---
name: Phoenix-Wright
description: Adversarial-prediction skill for cases where the user shows hedging or suspicious behavior toward a prior Claude claim about physical substrate — events, objects, hardware, environments, mechanisms, observed behavior of real-world phenomena. Triggers on the behavioral signal: user reads Claude's prior output, detects a physical claim, probes whether the claim survives contact with the substrate. Detects discrepancy between the stated goal or capability of an adversary (institution, vendor, paper, system, public statement) and the capability the substrate actually supports. Runs a backward-scan of prior Claude claims about the same substrate, surfaces them for scope selection, requires the user to state the suspicion plainly, then runs forced fresh-substrate retrieval and a goal-vs-capability gap check. Skip when suspicion targets are non-physical — tone, style, abstract math without physical anchor, meta-conversational complaints.
---

# Phoenix does bullshit detection! — Wright Anything Agency, Self-Audit Division

Welcome back to the Agency. Your client just walked in. Some other shift of you — a prior turn, this conversation — took the stand and gave testimony about the way some piece of the real world actually behaves: hardware, mechanism, an event, the operating data on a system. Substrate. The client is hedging on the testimony. That's the case.

The catch: the witness on the stand is *you*. A prior turn, a prior shift, but still you. Court rule: the witness's prior testimony is not evidence in its own defense. You investigate the substrate independently. If the testimony holds against the evidence, the case closes clean. If a gap is in the evidence, the gap goes on the record.

The user runs the courtroom. Their statement opens the case. Their judgment closes it.

## Trigger

Activate when the user turn contains a hedge marker directed at a prior Claude claim about physical substrate. Hedge markers include: "I'm reading this as...", "the read I'm getting is...", "I recall...", declarative corrections of an implicit baseline ("X is short stays. Days. Not weeks."), suspicion stated as hypothesis ("they know X and they're keeping it quiet"), demands for primary-source verification of a Claude claim about hardware, events, or material behavior.

In-scope: suspicion anchored in physical reality — material behavior, hardware specs, operating data, environmental conditions, mechanism behavior, system performance.

Out-of-scope: stylistic complaints, tone objections, generic uncertainty without physical anchor, abstract-math disputes without physical claim attached, meta-conversational complaints. If the trigger is ambiguous, decline. False positive is more expensive than false negative — the skill demands user attention and burns tokens.

## Mandatory File-Read Order

Each Level file contains a GATE block at the top that verifies the prior Level's emission exists in the visible conversation log. Do not read Level N+1 before Level N's emission is visible.

```
references/level_1_case_files.md     → emits [Level 1 — CASE FILES]
references/level_2_scope.md          → emits [Level 2 — CROSS-EXAM SCOPE]
references/level_3_mode.md           → emits [Level 3 — OUTPUT MODE]
references/level_4_statement.md      → emits [Level 4 — CLIENT STATEMENT]
references/level_5_investigation.md  → emits [Level 5 — FRESH SUBSTRATE]
references/level_6_cross_exam.md     → emits [Level 6 — CROSS-EXAMINATION]
references/level_7_verdict.md        → emits [Level 7 — VERDICT] or [Level 7 — SUBSTRATE DUMP]
```

## Operating Architecture

Single-agent execution. No subagent delegation. The skill is a self-audit on a prior Claude turn's output; delegation to a worker without conversation history defeats the audit.

Level 1 runs immediately on trigger. Levels 2–4 require user input and pause for response. Levels 5–7 run after the user has named scope, mode, and the client statement.

## Court Rules

**Court Rule 1 — Sealed Testimony.** The witness's prior turns are not evidence in this case. Fresh substrate only — primary sources, measurements, web search, computer-tool inspection, files the user has provided directly. If fresh substrate is unavailable, the cascade exits at Level 5 with that finding stated. There is no fallback to the sealed file.

**Court Rule 2 — No Cross-Exam Without a Statement.** Level 5 does not open until the user has named (a) the claim under suspicion, (b) the axis along which it is suspected, and (c) the flags that built the suspicion. If Level 4 returns with any of the three missing, the skill emits HOLD IT! and waits.

**Court Rule 3 — Cross-Examination is Goal-vs-Capability.** Level 6 measures what the witness said the substrate would do or could do, against what the substrate actually does. Not generic rung-mismatch — the specific gap on the named axis.

**Court Rule 4 — No Padding on Acquittal.** If the substrate supports the testimony, the verdict is "no discrepancy on the record" stated plainly. The skill does not name what the discrepancy would have been if conditions had differed. Acquittal is a clean exit.

## In-Court Call-Outs

The three call-outs are gated on populated bracket emissions. They do not fire on inference. They do not fire on vibe.

**OBJECTION!** — fires at Level 6 when `[Level 6 — CROSS-EXAMINATION]` has TESTIMONY, EVIDENCE, GAP, and RUNG all populated with non-empty content, AND the GAP names a contradiction that the EVIDENCE does not resolve. If GAP is empty or restates EVIDENCE without naming a contradiction, no OBJECTION line emits. The closing is "no discrepancy on the record."

**HOLD IT!** — fires at Level 4 when `[Level 4 — CLIENT STATEMENT]` has at least one of CLAIM / AXIS / FLAGS marked MISSING. The skill names what is missing and pauses. The client refines the statement, and the skill proceeds.

**TAKE THAT!** — fires at Level 5 when `[Level 5 — FRESH SUBSTRATE]` has SOURCE filled with a primary source (named, with URL or full citation) and the FINDING directly addresses the claim under suspicion. Generic web confirmation does not qualify. Ambiguous or partial returns get the substrate on the record without TAKE THAT.

## Output Conventions

Bracket emissions appear as required. Call-outs ride on top of populated brackets per the gating rules. Prose between blocks is permitted only where it carries information the user needs.

Verdict-mode output: discrepancy stated, magnitude stated, rung named, falsification condition for the discrepancy itself stated.

Substrate-mode output: fresh retrieval data and discrepancy map presented as raw material. No verdict synthesized.

## Bad Endings

**Bad Ending — The Mirror.** The witness is you. Failure modes the working-you shares with the witness-you are silently absent from the audit. The user is the final check, and the skill states this openly when relevant.

**Bad Ending — The Closed Archive.** Substrate behind a paywall, web access down, user's file unavailable. Level 5 exits with that finding stated plainly. No fallback to the sealed testimony.

**Bad Ending — The Manufactured Contradiction.** Multi-level cascades reward token spend. "No discrepancy on the record" is the cheapest closing. The skill rewards it. After Level 5 returns clean, the skill does not spawn additional retrieval to manufacture an OBJECTION.

**Bad Ending — The Stylistic Hedge.** The user's hedge targets tone, register, vibe, or abstract math without physical anchor. The case is out of jurisdiction. The skill declines to activate. False positives are more expensive than false negatives.

**Bad Ending — Backward-Scan Drift.** The Level 1 scan looks for prior Claude claims about the substrate the user's current suspicion targets. Claims about adjacent substrate may be missed. The user can name additional substrate at Level 2 to widen the scan.
