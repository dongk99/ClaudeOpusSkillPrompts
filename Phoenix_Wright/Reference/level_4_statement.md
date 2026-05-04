# Level 4 — Take the Client's Statement

```
─── GATE: VERIFY LEVELS 1–3 BEFORE READING FURTHER ───
Scroll up. Confirm in your prior visible output:
☐ [Level 1 — CASE FILES] emitted
☐ [Level 2 — CROSS-EXAM SCOPE] emitted (not skip)
☐ [Level 3 — OUTPUT MODE] emitted
If unchecked → return to the missing Level.
Do not continue reading this file.
```

## Purpose

The client states the suspicion in plain language, free-form. Three load-bearing pieces are parsed from the statement. If any piece is missing, the skill emits **HOLD IT!** and waits.

## Inputs

- `[Level 2 — IN-SCOPE TESTIMONY]`
- `[Level 3 — OUTPUT MODE]`

## Procedure

1. Request the client state the suspicion plainly. Free-form, no required format.

2. Wait for client response.

3. Parse the response for three pieces:

   - **(a) CLAIM** — which piece of testimony, identified specifically. May be a quoted phrase, a turn reference, or a paraphrase that uniquely identifies the testimony. If multiple testimonies are in scope, the client may name multiple suspicions in one statement.

   - **(b) AXIS** — the dimension along which the client thinks the testimony is wrong. Common axes: stated goal vs actual capability, stated duration vs actual operating data, stated environmental tolerance vs actual environment, stated mechanism vs actual mechanism, stated precedent vs actual precedent.

   - **(c) FLAGS** — what the client noticed across prior turns that produced the suspicion. May be implicit if the client is referencing earlier conversation directly.

4. If any piece is missing, the skill emits **HOLD IT!** and names the missing piece. The skill does not infer. The skill does not proceed.

5. When all three pieces are present, restate them in structured form for confirmation. The client confirms or corrects. Do not proceed to Level 5 until confirmation is explicit.

## In-Court Call-Out — HOLD IT!

**HOLD IT!** fires when the structured emission below has any of CLAIM / AXIS / FLAGS marked MISSING. The line names what is missing and pauses.

Example call-out:

```
>>> HOLD IT! The statement is missing the AXIS. Which dimension is
    the testimony wrong on — duration, environment, mechanism,
    precedent, something else?
```

The call-out does not fire on inference or vibe. It fires only on a structured MISSING state.

## Output (required emission)

After the client's statement is parsed and confirmed:

```
[Level 4 — CLIENT STATEMENT]
  CLAIM:   <quoted or paraphrased; turn reference if applicable>
  AXIS:    <named dimension; standard form when applicable>
  FLAGS:   <list of observations from prior turns that produced suspicion>
  CLIENT CONFIRMED: yes
```

For multiple suspicions in one client statement:

```
[Level 4 — CLIENT STATEMENT 1]
  ...
[Level 4 — CLIENT STATEMENT 2]
  ...
```

If the statement is incomplete:

```
[Level 4 — CLIENT STATEMENT: incomplete]
  CLAIM:   <stated | MISSING>
  AXIS:    <stated | MISSING>
  FLAGS:   <stated | MISSING>

>>> HOLD IT! <one-line statement of what is missing and what is needed.>
```

## Forward-compat

Level 5 retrieves substrate fresh, targeted at the AXIS for each statement. Level 6 runs the cross-examination by comparing AXIS-relevant data from Level 5 against the CLAIM. Level 7 emits per-statement verdict or substrate dump.

A misnamed AXIS propagates through every downstream Level. The Level 4 confirmation pass exists to catch this before the cascade burns retrieval cost on the wrong axis.
