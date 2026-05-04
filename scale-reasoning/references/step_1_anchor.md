# Step 1 — Anchor on the Reference Point

```
─── GATE: ENTRY POINT ───
This is the entry step. No upstream gate.
Confirm before reading further:
☐ Skill triggered by causal query (or by re-evaluation request)
☐ Mandatory-phase skip not warranted (or skip already stated with named ground)
If query is plainly non-causal AND not a re-evaluation → emit skip statement, stop.
```

## Purpose

Identify the rung at which the user's phenomenon naturally sits. The user's query, observation, or claim is the anchor of the ladder. **No pre-built ladder is imposed.**

## Inputs

- User query, observation, or claim (current turn)

## Procedure

1. Read the query for the phenomenon under examination — individual behaviour, group behaviour, cellular mechanism, planetary process, market clearing, geopolitical event, whatever the query has placed under examination.
2. Identify the rung at which that phenomenon naturally sits. State it in plain language.
3. **Do not commit to a domain tag at this step.** Domain tagging is performed at S2 once the surrounding rungs have been laid out. Premature tagging biases the axis choice.
4. **Do not impose a length, time, or energy axis pre-emptively.** Axis selection is S2's responsibility and depends on which axis the phenomenon naturally varies along.

## Output (required emission)

```
[STEP 1 — REFERENCE_RUNG: <plain-language statement of the phenomenon and its rung>]
```

If the query contains multiple phenomena at distinct rungs, emit one block per phenomenon, indexed:

```
[STEP 1 — REFERENCE_RUNG 1: ...]
[STEP 1 — REFERENCE_RUNG 2: ...]
```

Each parallel reference rung is a parallel invocation of the skill — do not collapse.

## Forward-compat

S2 reads the reference rung to select the axis along which the ladder will extend. S3 traverses upward, downward, or both from this rung. S4 alternative-theory search targets theories operating at this rung. S6 report orients the user to the rung as anchor.

A misnamed reference rung — too high, too low, or off-axis — propagates through every downstream step. Re-read the user query if uncertain whether the phenomenon as named matches the phenomenon as queried.
