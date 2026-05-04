# Level 1 — Backward Scan of Prior Claude Claims

```
─── GATE: ENTRY POINT ───
This is the entry Level. No upstream gate.
Confirm before reading further:
☐ User turn contains a hedge marker directed at a Claude physical-substrate claim
☐ Suspicion is anchored in physical reality, not tone or style
If unconfirmed → do not activate the skill. Respond normally.
```

## Purpose

Scan backward through the conversation for prior Claude turns containing claims about the physical substrate the user's current hedge targets. Surface the chain so the user can choose scope at S2.

## Inputs

- Current user turn (the hedge)
- Full prior conversation in context

## Procedure

1. Identify the physical substrate the user's hedge targets. Examples: "Apollo LRV operations" / "PPE hardware fate" / "lunar regolith adhesion at south pole" / "Anthropic bug bounty intake form behavior". State the substrate in one phrase.

2. Scan backward through every prior Claude turn. For each turn, identify whether Claude made a claim about the named substrate. A claim counts if it asserts how the substrate behaves, exists, performs, is configured, or is constrained.

3. For each claim found, record:
   - Turn reference (which Claude turn)
   - Claim summary (one sentence, no padding)
   - Source basis at time of claim (primary source named, web search result, prior Claude memory, no source named)

4. Order the claims chronologically (earliest first).

5. If no prior claim is found, emit a single-line negative result. The cascade may still proceed if the user's hedge targets the current Claude turn only.

## Output (required emission)

```
[Level 1 — BACKWARD SCAN]
  SUBSTRATE: <named in one phrase>
  PRIOR CLAIMS:
    T<n>: <claim summary> | source: <primary | web | memory | none>
    T<n>: <claim summary> | source: <primary | web | memory | none>
    ...
  CURRENT CLAIM (T<current>): <claim summary> | source: <primary | web | memory | none>
```

If no prior claims found:

```
[Level 1 — BACKWARD SCAN]
  SUBSTRATE: <named in one phrase>
  PRIOR CLAIMS: none found
  CURRENT CLAIM (T<current>): <claim summary> | source: <primary | web | memory | none>
```

## Forward-compat

S2 reads the scan to present scope options. S5 retrieval re-pulls substrate data fresh; the prior-claim list is the inventory of what needs re-verification under backward-full scope.

The scan is descriptive only. No judgment of whether the prior claims were correct. That is S6's job.
