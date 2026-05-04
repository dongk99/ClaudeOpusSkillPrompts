# Level 1 — Pull the Case Files

```
─── GATE: ENTRY POINT ───
This is the entry Level. No upstream gate.
Confirm before reading further:
☐ User turn contains a hedge marker directed at a prior Claude
   claim about physical substrate
☐ The suspicion is anchored in physical reality, not tone or style
If unconfirmed → do not activate the skill. Respond normally.
```

## Purpose

Pull the case files. Scan backward through the conversation for every prior turn where the witness — a prior shift of you — testified about the physical substrate the client's current hedge targets. The chain becomes the inventory the client uses at Level 2 to choose cross-exam scope.

## Inputs

- Current user turn (the hedge)
- Full prior conversation in context

## Procedure

1. Identify the substrate the client's hedge targets. State it in one phrase. Examples: "Apollo LRV operations" / "PPE hardware fate" / "lunar regolith adhesion at south pole" / "Anthropic bug bounty intake form behavior."

2. Scan backward through every prior Claude turn. For each turn, identify whether the witness testified about the named substrate. A claim counts if it asserts how the substrate behaves, exists, performs, is configured, or is constrained.

3. For each claim found, record:
   - Turn reference (which Claude turn — T<n>)
   - Claim summary (one sentence, no padding)
   - Source basis at time of claim: **primary** (named primary source cited), **web** (web search performed), **memory** (training-data memory, no source named), or **none** (no source named at all)

4. Order chronologically (earliest first).

5. If no prior claim is found, emit a single-line negative result. The cascade can still proceed if the client's hedge targets the current Claude turn only.

## Output (required emission)

```
[Level 1 — CASE FILES]
  SUBSTRATE: <named in one phrase>
  PRIOR TESTIMONY:
    T<n>: <claim summary> | source: <primary | web | memory | none>
    T<n>: <claim summary> | source: <primary | web | memory | none>
    ...
  CURRENT TESTIMONY (T<current>): <claim summary> | source: <primary | web | memory | none>
```

If no prior testimony found:

```
[Level 1 — CASE FILES]
  SUBSTRATE: <named in one phrase>
  PRIOR TESTIMONY: none on file
  CURRENT TESTIMONY (T<current>): <claim summary> | source: <primary | web | memory | none>
```

## Forward-compat

Level 2 reads the case files to present scope options. Level 5 retrieval re-pulls substrate data fresh; the prior-testimony list is the inventory of what needs re-investigation under backward-full scope.

The case file is descriptive only. No judgment of whether the prior testimony was correct. That is Level 6's work.
