# Step 1 — Identify Excluded Set

```
─── GATE: ENTRY POINT ───
This is the entry step. No upstream gate.
Confirm before reading further:
☐ Skill triggered by structural condition (1–3) or keyword trigger (4–7) per SKILL.md
☐ If keyword trigger fired, structural conditions do NOT need to be re-verified
☐ If only structural triggers fired and the proposal is for stylistic register only → exit skill, do not enter S1
If unconfirmed → STOP. Respond normally.
```

## Purpose

Name the term(s) being excluded in their canonical literature form. The excluded set is the thing the alternatives are claiming separation from; the count in S4 will measure occurrences of these exact terms in retrieved literature.

## Inputs

- The excluded term(s), either explicitly named by the user or inferable from the conversation
- Domain context (which literature the test will run against)

## Procedure

**1. State each excluded term in canonical literature form.** Use the form most likely to appear in academic or technical sources, not casual abbreviations or user-side shorthand. Examples:
- "fine-tuning" (canonical) rather than "FT" or "tuning the model"
- "reinforcement learning from human feedback" or "RLHF" (both canonical)
- "alignment-via-training" rather than "training the alignment"

**2. Capture variant spellings and adjacent forms.** Variants are textual forms that count as the same term in literature search. List them in parentheses after the canonical form. Examples:
- "fine-tuning" (variants: "finetuning," "fine tuning")
- "RLHF" (variants: "reinforcement learning from human feedback," "RL from human feedback")

Variants are matched as separate strings at S4. A result that contains "finetuning" but not "fine-tuning" still counts as a hit for the canonical "fine-tuning" entry.

**3. State the reason for exclusion.** One sentence. The reason is recorded for downstream context — if the verdict comes back as paraphrase, the reason informs how the concession is framed at S6.

**4. Do not pre-judge what the alternatives will be.** Do not list candidate alternatives in this step. S2 handles that. Mixing them here biases the count framing.

## Output (required emission)

```
[STEP 1 — EXCLUDED]
  TERMS:
    1. <canonical form> (variants: <comma-separated list, or "none">)
    2. <canonical form> (variants: ...)
    ...
  REASON: <one sentence — why these terms were excluded from the response>
```

## Forward-compat

S2 does not read this emission directly but operates in awareness of it (alternatives must be distinct from the excluded set as canonically named). S3 search queries must NOT include the excluded terms (per Rule 2 of SKILL.md and Bad Ending: Confirmation searches). S4 counts occurrences of the canonical forms and variants exactly as registered here — no broadening, no narrowing. S5 reports per-term rates using the canonical form as the row label.

A poorly-canonicalized excluded term (too narrow → undercounts; too broad → overcounts) propagates through every downstream step. If uncertain about the right canonical form, prefer the form that the literature uses most frequently in titles and abstracts.
